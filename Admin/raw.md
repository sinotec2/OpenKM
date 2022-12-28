
### okmdb之外部存取

openKM基本上是以一個mysql的資料庫(okmdb)為核心在運作，其動態資料內容則是另存在/var/lib/mysql/ibdata1及\$TOMCAT\_HOME/repository下，因此只要service
tomcat啟動，任何時間讀取其資料庫，只有讀取的權限，如果要寫入必須在openKM系統內寫入。

能夠存取mysql資料庫的方式有很多，直接以mysql程式讀取，其指令如網友介紹[^19]，或其他第三方軟體。以下介紹python2的MYSQLdb模組。程式如下：

kuang\@sino4 \~/MyPrograms/openkm

\$ cat rd\_okmdb.py

import **MySQLdb**

from pandas import \*

db = MySQLdb.connect(host=\"localhost\", user=\"kuang\",
passwd=\"\*\*\*\*\", db=\"okmdb\",
charset=\'utf8\',**port=8080,unix\_socket=\"/var/lib/mysql/mysql.sock\"**
)

cursor = db.**cursor**()

cursor.execute(\"SELECT \* from OKM\_ACTIVITY\")

data = cursor.fetchall()

cursor.execute(\"describe OKM\_ACTIVITY\")

cols = cursor.**fetchall**()

cols=\[cols\[i\]\[0\] for i in xrange(len(cols))\]

dd={}

for i in xrange(len(cols)):

dd.update({cols\[i\]:\[data\[j\]\[i\] for j in xrange(len(data))\]})

df=DataFrame(dd)

pivot\_table(df.loc\[df.ACT\_ACTION==\'LOGIN\'\],index=\'ACT\_USER\',values=\'ACT\_ACTION\',aggfunc=\'count\').reset\_index().sort\_values(\'ACT\_ACTION\',ascending=False).reset\_index(drop=True)

pivot\_table(df.loc\[df.ACT\_ACTION.map(lambda x:\'DOCUMENT\' in
x)\],index=\'ACT\_PATH\',
values=\'dum\',aggfunc=\'count\').reset\_index().sort\_values(\'dum\',ascending=False).reset\_index(drop=True).head(10)

#### MySQLdb的安裝[^20]：如果pip不能成功，可以嘗試conda install。MySQLdb模組的使用方式[^21]。

#### db之連結

如果沒有特別指定，port會選3306，而socket會設在/temp目錄下。由於okmdb有自己的設定方式，不是內設位置，因此必須要按照okmdb的內容來連結。

#### cursor：類似DOS的prompt(c:/\>)概念，此處稱之為cursor(mysql\>)。括弧內的命令即為一般mysql指令。

#### okmdb所有表格的項目，在okmdb目錄下(連原創者也無法讀取，須sudo ls /var/lib/mysql/okmdb)，由某個表中讀取內容，須先在mysql內SELECT，再將cursor的內容fetchall()出來。存成pandas dataframe形式。

#### 執行pivot\_table

範例中將印出登入最多之使用者，以及最熱門檔案。

ACT\_USER ACT\_ACTION

0 kuang 43

1 4294 14

2 okmAdmin 10

3 6729 5

4 4935 3

5 penny 2

6 3886 1

7 6790 1

dum ACT\_PATH

0 41 /okm:personal/kuang/筆記/python.doc

1 22 /okm:personal/kuang/筆記/smoke.doc

2 17 /okm:root/有關openKM/OpenKM User Guide.pdf

3 16 /okm:root/有關openKM/okm-6.3-com.pdf

4 11 /okm:personal/4294/addusers.bsh

5 10 /okm:root/空氣污染防制及健康風險/1215C\_期末報告chap5.doc

6 10 /okm:personal/kuang/使用手冊/docs.openkm.com.html

7 9 /okm:personal/kuang/使用手冊/098FY005515002-001.pdf

8 9 /okm:personal/kuang/使用手冊/CALPUFF5\_UsersGuide.pdf

9 9 /okm:root/人員培訓心得報告/2018歐洲能資源循環利用考察心得報告.pdf

#### 執行Pyforms-web程式：Okmdb分析結果之網路展示

Pyforms-web網路伺服器(runserver)基本上是使用者觸發之執行程式，而該網頁並沒有使用者的篩選、是(內部)公開的資訊彙集處。為避免過度執行okmdb的讀取及分析，因此藉由crontab定期(上班時間之各小時)來進行統計分析並產生分析結果表(設定詳下述)，然後使用者透過pyforms程式來讀取，以一定期更新的方式進行。

執行結果如圖所示：目前設定了6個分頁，採用了pyforms-web
/orquistra範例模板，各分頁分別是檔案時間(AnalysisTime)、最大檔案目錄(LargesPath)、最常登入技術組(MostFreqDivisons)、最受歡迎文件(MostFreqGets)、最常登入群組(MostFreqGroups)、以及最常登入使用者(MostFreqUsers)。

由於模板範例是文字條列，pyforms的文字並不能使用html上的設定，而必須另外使用ControlList方法來做：

self.\_tableGet = ControlList(

\'檔案\',

horizontal\_headers=\[\'no\',\'開啟次數\',\'檔案\'\],

select\_entire\_row=True,

readonly=True,

)

self.\_tableGet.value = \[l.split() for l in lines\[-10:\]\]

self.formset = \[

{ \...,

\'MostFreqGets\': \[segment(\'\_tableGet\')\], \...

![](media/image16.png){width="5.799305555555556in"
height="3.2645833333333334in"}

### openKM的定期作業

#### mysql的備份

必須使用mysqldump指令，才可以在沒有停下mysql服務的情況下，完整備份運作中的資料庫。

cd /home/openkm/tomcat/mysql\_okmdb

> mysqldump \--user=root \--password=\'sino2019\' okmdb \> \\
> /home/openkm/tomcat/mysql\_okmdb/okmdb.sql

自動執行方式採用/etc/crontab設定進行。

#### tomcat之備份

tomcat者設定、repository的內容、以及logs檔案，因為隨時在變動中，必須在深夜沒有增加檔案時，進行備份。備份方式採crontab設定rsync指令進行。

rsync -a /home/openkm/tomcat/\* /autofs/master/openkm/tomcat\_sino4

（-a：封裝備份模式，相當於
-rlptgoD，遞迴備份所有子目錄下的目錄與檔案，保留連結檔、檔案的擁有者、群組、權限以及時間戳記[^22]。）

由於自動化備份不能輸入密碼，因此使用者openkm必須在sino4與master之間設成免密登入，透過autofs的設定，就可以定期備份了。

重建系統之後，可以運用mysql（mysql -u root -p database\_name \<
backup.sql）和rsync指令（對調來源與目的位置）來回復檔案及設定即可。

#### 上班天/時間okmdb的讀取及分析(sino4:/etc/crontab設定每小時執行)

\$ cat /home/kuang/MyPrograms/openkm/rd\_okmdb.bsh

DOW=\$(date +%u)

H=\$(date +%H)

if \[ \$DOW -gt 0 \] && \[ \$DOW -le 5 \];then

if \[ \$H -ge 8 \] && \[ \$H -le 17 \];then

cd /home/kuang/MyPrograms/openkm

rm -f rd\_okmdb.txt

LC\_ALL=\"zh\_TW.UTF-8\" ./rd\_okmdb.py \>rd\_okmdb.txt

if \[ \$H -eq 17 \];then

ymd=\`date \--rfc-3339=\'date\'\`

cp rd\_okmdb.txt rd\_okmdb.txt\_\$ymd

fi

fi

fi

#### 定時批次作業(cron table)

一如linux的crontable，openKM程式內也有crontable，可以每小時(\@hourly)、每天(\@daily)、每週(\@weekly)、crontab的內容可以參考官網設定與範例[^23]，以下為bean
shell每天執行du的範例。

\[openkm\@sino4 rd\_openkm.log\]\$ cat du.bsh

import org.slf4j.Logger;

import org.slf4j.LoggerFactory;

import com.openkm.api.\*;

import com.openkm.bean.\*;

import com.openkm.util.\*;

Logger log = LoggerFactory.getLogger(\"**com.openkm.scripting.du**\");

int MAX\_DEPTH = Integer.MAX\_VALUE;

void nodeTask(String path, int depth) throws Exception {

for (Folder fld : OKMFolder.getInstance().getChildren(null, path)) {

ContentInfo ci = OKMFolder.getInstance().getContentInfo(null,
fld.getPath() );

log.info(FormatUtil.formatSize(ci.**getSize**()) + \'\\t\' +
fld.**getPath**());

if (depth \< MAX\_DEPTH) {

nodeTask(fld.getPath(), depth + 1);

}

}}

log.info(\"\*\*\*\*\* DU BEGIN \*\*\*\*\*\");

nodeTask(\"/okm:root\", 0);

log.info(\"\*\*\*\*\* DU END \*\*\*\*\*\");

crontab設成每天執行，其結果將會出現在\$TOMCAT/logs/openkm.log檔案內，marker會是日期及com.openkm.scripting.du。因此只要在逐時python程式內讀取openkm.log內容(結果為du.csv)貼在網頁即可。

\[openkm\@sino4 rd\_openkm.log\]\$ cat rd\_log.py

\#!python 3

from pandas import \*

import subprocess

logP=\'/home/openkm/tomcat/logs/\'

log=logP+\'openkm.log\'

cmd=\'date \--rfc-3339=\"date\"\'

today=subprocess.check\_output(cmd,shell=True).strip(b\'\\n\').decode(\"utf-8\")

du\_mark=\'com.openkm.scripting.du\'

cmd=\'grep \'+today+\' \'+log+\'\|grep \'+du\_mark+\'\|grep -v \" DU
\"\'

lis=subprocess.check\_output(cmd,shell=True).split(b\'\\n\')

a=\' - \'

i=lis\[0\].decode(\"utf-8\").index(a)+3

lis=\[j\[i:\].decode(\"utf-8\").split(\'\\t\') for j in lis if
len(j)\>0\]

size=\[lis\[i\]\[0\] for i in range(len(lis))\]

path=\[lis\[i\]\[1\] for i in range(len(lis))\]

df=DataFrame({\'size\':size,\'path\':path})

df\[\'level\'\]=\[a.count(\'/\') for a in df.path\]

kk={\'GB\':1000000000,\'MB\':1000000,\'KB\':1000,\'B\':1}

df\[\'num\'\]=\[float(a.split()\[0\])\*kk\[a.split()\[1\]\] for a in
df\[\'size\'\]\]

df=df.sort\_values(\[\'level\',\'num\'\], ascending=\[True,
False\]).reset\_index(drop=True)

del df\[\'num\'\]

df.set\_index(\'level\').to\_csv(logP+\'du.csv\')

 Glossary
=========

  ------- ------------------------------------
  APIs    Application Programming Interface
  COM     Component Object Model
  GDI     Graphical Data Interface
  GUI     Graphical User Interface
  GUIDs   globally unique identifiers
  ODBC    Open Database Connectivity
  OLE     Object Linking and Embedding
  MDI     Multiple Document Interface
  MFC     Microsoft Foundation Class Library
  SDI     Single Document Interface
  SDK     Software Development Kit
  TLB     type library file
  ------- ------------------------------------

附錄A參考文獻與資料來源

[^1]: 依據2019/7/26日官網WHO IS ONLINE：In total there are374users
    online :: 4 registered, 0 hidden and 370guests (based on users
    active over the past 5 minutes)•Most users ever online was1263on Mon
    Jan 28, 2019 7:29 pm。其forum發表情形：STATISTICS•Total posts 31953/
    Total topics 6010/ Total members 3419•功能限制。其市佔情形：aimed at
    all industries, regardless of company size. --has made over
    6,500installations worldwide--Community version has a monthly
    average of 5,000 downloads.--Buyers:•ABC hospitals/ Cherokee Nation/
    Deloitte/ DGT/ Factor Energia•Modria/ Sernageomin/ United States
    Government/ Ypergas/
    Zoetis。詳見[[http://200.200.121.74:8080/OpenKM/index.jsp?uuid=1544f8fa-e45a-40a6-bd99-5d2fbef832a1]{.underline}](http://200.200.121.74:8080/OpenKM/index.jsp?uuid=1544f8fa-e45a-40a6-bd99-5d2fbef832a1)

    [[http://200.200.121.74:8080/OpenKM/webdav/okm:root/有關openKM/有關KM決策的建議.pdf]{.underline}](http://200.200.121.74:8080/OpenKM/webdav/okm:root/有關openKM/有關KM決策的建議.pdf)

[^2]: Knowledge management software,
    https://en.wikipedia.org/wiki/Knowledge\_management\_software

[^3]: https://www.goodfirms.co/blog/top-10-free-and-open-source-knowledge-management-software

[^4]: 專業版
    [[https://docs.openkm.com/kcenter/view/okm-6.4/]{.underline}](https://docs.openkm.com/kcenter/view/okm-6.4/)
    公開版[[https://docs.openkm.com/kcenter/view/okm-6.3-com/]{.underline}](https://docs.openkm.com/kcenter/view/okm-6.3-com/)

[^5]: Southeast Asian Ministers of Education Organization Secretariat
    [[http://www.seameo.org/seameoweb2/images/stories/Programmes\_Projects/OpenKM/OpenKM%20User%20Guide.pdf]{.underline}](http://www.seameo.org/seameoweb2/images/stories/Programmes_Projects/OpenKM/OpenKM%20User%20Guide.pdf)

[^6]: [[https://docs.openkm.com/kcenter/view/okm-6.3-com/quick-start.html]{.underline}](https://www.evernote.com/OutboundRedirect.action?dest=https%3A%2F%2Fdocs.openkm.com%2Fkcenter%2Fview%2Fokm-6.3-com%2Fquick-start.html)

[^7]: http://terms.naer.edu.tw/detail/1679254/?index=7

[^8]: [[https://www.openkm.com/en/features.html]{.underline}](https://www.evernote.com/OutboundRedirect.action?dest=https%3A%2F%2Fwww.openkm.com%2Fen%2Ffeatures.html)




[^19]: 凍仁的筆記，MySQL
    語法匯整[[http://note.drx.tw/2012/12/mysql-syntax.html]{.underline}](http://note.drx.tw/2012/12/mysql-syntax.html)

    SQL INSERT
    INTO，http://gn02214231.pixnet.net/blog/post/200632246-sql-insert-into

    Mysql
    新增、修改、刪除、查詢，http://lin147.pixnet.net/blog/post/125095427-mysql-新增、修改、刪除、查詢。MySQL
    教材：刪除/修改資料 https://bcc16.ncu.edu.tw/A/mysql/09.shtml

[^20]: MySQL-python 1.2.5, https://pypi.org/project/MySQL-python/

[^21]: MySQLdb User\'s Guide,
    http://mysql-python.sourceforge.net/MySQLdb.html

[^22]: https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/

[^23]: [[https://docs.openkm.com/kcenter/view/okm-6.3-com/crontab.html]{.underline}](https://docs.openkm.com/kcenter/view/okm-6.3-com/crontab.html)
    https://docs.openkm.com/kcenter/view/okm-6.3-com/script-samples-for-crontab\-\--basic-file-system-document-importer.html
