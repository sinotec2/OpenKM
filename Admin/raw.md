管理者權限功能
--------------

### 安裝

#### openKM主程式

openKM官網[^9]有詳細的安裝過程說明，還有其他周邊軟體的安裝或設定，注意事項：

##### 磁碟機空間：由於openKM系統內不能局部選擇磁碟機存取位置，因此必須事先將\$TOMCAT\_HOME/repository放在足夠大的磁碟機，以免事後搬遷(可以由Administration→statistic看到系統資源使用的情況，詳下述)。

##### Openoffice以及Tesseract的安裝。安裝之後要找到其檔案所在位置，以便在openKM裏逐一設定，如Configuration之check。

system.swftools.pdf2swf

OK - /home/openkm/tomcat/bin/pdf2swf

system.imagemagick.convert

OK - /usr/bin/convert

system.ocr

OK - **/usr/bin/tesseract**

system.openoffice.path

OK - **/opt/openoffice4/program/soffice.bin**

##### 加入使用者：初始登入okmAdmin(pw:admin)，再增加管理者之使用者帳密。批次增加帳密可參考下述。

##### 有關中文之介面[^10]：可下載[^11]後在Configure→default.lang處設定，之後可在Administration→Language處修改局部之翻譯。

##### 有關mail server：openKM需要有mail server，因此需要在啟動tomcat時，同時將postfix啟動，(至少)使notify功能能作用。

##### Window系統上安裝之注意事項

###### mysql版本

雖然官網有介入MySQL8版本的注意及修改事項，但經測試結果發現，java(新版connector/J)並不能連結上資料庫平台，還是安裝舊版MySQL比較穩定能搭配。因此如果伺服器上有別的mysql運作需求將會遭遇困難，符合伺服器必須是專用(dedicated)之原則。

###### okmAdmin登入後trash目錄無法產生之問題

此問題應為OpenKM.cfg內重複新增(create)hibernate.hbm2ddl所置，將其改為none即可。

###### openoffice目錄設定：不需要到**"program"(C:\\Program Files (x86)\\OpenOffice 4)**

###### tomcat版本與中文顯示

網友曾提及tomcat
8內設字形並非UTF8的問題，並且建議將tomcat.connector.uri.encoding設成UTF-8，然而此舉在centos卻不需要，且無益於中文字的顯示，顯示可能還有其他因素無法正確顯示中文字型。

#### Mysql的管理

##### 給定root的密碼：mysqladmin -u root password

##### 給定其他使用者的帳密[^12]

##### 給定特定的socket: \--socket=\"/var/lib/mysql/mysql.sock\"

#### JBOSS的安裝

JBOSS是早期或window所需的虛擬機器平台(JVM)。由於目前最新的版本[^13](
boss-as-7.1.1.Final)
與java8並不相容，必須使用較早版本。安裝過程也可以參考openKM早期版本的介紹[^14]。

可以由redhat官網下載(須註冊[^15])後，按照7.1版方式安裝即可。

由於openKM已經佔用了8080的端口，因此要在standalone.xml檔[^16]中將8080改成其他端口。同時也要注意將127.0.0.1改成intra
net IP位址。

啟動前後，要記得加入使用者帳密。JBOSS要求的密碼必須是8碼，同時要求至少一碼非英數，要特別注意。

### 公共空間權限與內容的管理

不論是哪一層角色或群組，使用者存取檔案等各項動作均有紀錄可供追蹤。

#### 有關「管理者」

「管理者」administrator有最大的權限，甚至包括個人空間。如果有資料是不願意讓管理者看到、處理的，請不要出現在KM。

有關使用者增、刪、群組角色增刪、群組設定(指派)，檔案的批次上(下)載等系統內作業，都必須是「管理者」。

#### 有關ROLE\_USER可以瀏覽(下載)之內容

ROLE\_USER看來是最低的權限，但不能刪除，因為登入者必須都有一個ROLE\_USER群組[^17]，可以看待為訪客。意謂在okm:root根目錄，必須要開放ROLE\_USER至少可以讀，KM系統也必須要充實訪客可以參觀的內容，達到「跨領域」交流的目標。至於其他則必須設為限制瀏覽，正面表列以下建議：

##### sinotech KM(可下載)的內容

##### 跨部門會需要查找的重要專業訊息：重要計畫環評報告、環境品質年報、氣象年報、新聞剪影等、研討會報告

##### 對公司內各部門開放之專題演講或課程

##### 同仁在公司內、外發表之期刊文章、學術演講簡報

##### 有關公司本身的即時資訊：

###### 公司、部門重要發表、簡介、簡報參考訊息等，(如果有)年度CSR，也可以陳列於此。

###### openKM官網連結、手冊、基本統計、openKM動態資訊等等。

#### ROLE\_USER限制瀏覽項目建議如下：

##### 計畫成果與半成果：按照現行O槽與Y槽管制架構，跨部門是不能瀏覽的。

#### 有關部門群組、跨部門專案群組與KM之管理

按照公司制度部門經理轄下有2\~3位技術經理不等，各管理專門的技術領域，同樣延續ROLE\_USER的管理概念，部門也必須設有每一位成員可以瀏覽的內容、以及各技術組之範疇內容。

部門(department)如ROLE\_E1、ROLE\_E2、ROLE\_WW、ROLE\_ER，以2碼約定。技術組(division)如AIR、EIA、WWc(減碳)、WWs(管線)、WWp(廠站)等。

跨部門專案是特例，由計畫經理管理，同樣以下必須設有共同與副經理所轄範疇。建議以計畫編號做為群組代碼。如為跨計畫常設性的研發領域，則直接以專題方式辦理。

部門經理必須管理部門成員可以瀏覽、與限制瀏覽的範疇。

#### 有關技術群組、專案群組與KM之管理

按照現行制度，技術經理以下管轄4\~5位計畫主管(如air1\~air5、eia1\~eia5)，各有管理計畫1\~5個不等。同樣技經也必須率定技術組必須要能看到的範疇，以及各組自行管理的範疇。

#### 計畫主管層級之管理

為利工作推動，計畫主管必須要求所轄工程師提供重要的技術文件，以供同組其他工程師工作所需。如為可以公開到上層跨組(部)的，經上層主管同意後，以打開上層跨組(部)其他同仁的權限。

### 系統資源管理

「管理面板」→statistic內容雖然不多，但也都與系統資源的現況與最佳狀況的管理有關。

![](media/image14.png){width="5.799305555555556in"
height="4.642361111111111in"}

#### 檔案大小與個數:如有垃圾桶待清空，可以通知該使用者清空垃圾桶。如有個人資料夾過大，同樣提醒其有效管理個人資源。

#### 系統記憶體使用比例：

如果伺服器開啟太久、無效登入太多，系統記憶體會持續降低，有效提昇的作法，即為定期停止伺服器再開啟即可。

目前已經設定系統在每天同步備份後，重新啟動，以保持記憶體可能在安全範圍。

#### Java虛擬(JVM)記憶體容量

#### 磁碟機空間：如果檔案久未查詢，則將其納入永久保存，此處只要留下摘要、連結、或詮釋資料即可。

### 批次新增或修改使用者設定

「管理面板」中使用者項目下，按下綠加號即可新增一使用者，但一次只能一筆。論壇中對於批次建使用者的需求多所質疑，因此解法也不多，總之初次建立、由別的系統轉過來、或者是日常使用者管理等情況，都會遇到需要批次建立的問題。

解決的原則就是利用openKM的內建程式(Script)來做。

#### 基本指令[^18]

> import com.openkm.api.\*;
>
> OKMAuth.getInstance().createUser(null,
> \"3454\",\"sksun3454\",\"sksun\@mail.XXX\",\"孫XX\", true);
>
> OKMAuth.getInstance().assignRole(null,\"3454\",\"ROLE\_USER\");
>
> OKMAuth.getInstance().deleteUser(null,\"3454\");

createUser的引數依序是ID、密碼、email、以及名稱。名稱雖然可以接受中文，中文也可以在一般的檔案屬性中順利搜索、運作，但在openKM比較高階的管理者介面，其實是無法解析的。如果必要可以刪除，改以英數文字較為穩定。

指定角色(assignRole)分開執行，因為一個使用者可以有很多角色，指定時只要分次、分批疊加上去就可以了。

刪除使用者所需要的引數就只要ID即可。刪除後檔案的作者會有何變化，目前未知。

#### 產生批次檔

網友介紹以VB在excel內作業，以下為python的範例。不論什麼工具，目標就是將資料表填入程式碼中。

/cluster/miniconda/envs/py37/bin/python

\# coding=\'utf8\'

\"\"\"this program generate the scripts for openKM add\_users and
assign\_roles\"\"\"

import os

from pandas import \*

from pypinyin import pinyin, lazy\_pinyin

**\#這一段將公司分機查詢之部門員編、email等表格讀入。**

**\#其中也利用了grep的技巧，把資料從複雜的表格中讀出來。**

**dpt=\'SEC\_EE.html\'**

**dpt=\'EE1.html\'**

os.system(\'grep \\\>Email \'+dpt+\' \>email.txt\')

with open(\'email.txt\', \'r\') as f:

ll = \[l for l in f\]

email = \[l.split(\'\>\')\[6\].split(\'\<\')\[0\] for l in ll\]

chr = \"\'\<td style=\\\"text-align:center\\\"
class=\\\"ng-binding\\\"\>\'\"

os.system(\'grep \' + chr + \' \'+dpt+\' \| grep -v \"\>\<\" \| grep -v
\"環境\" \| grep -v \"理\" \> emp.txt\')

with open(\'emp.txt\', \'r\') as f:

ll = \[l for l in f\]

empno = \[l.split(\'\>\')\[1\].split(\'\<\')\[0\] for l in ll\]

df = DataFrame({\'empno\': empno, \'email\': email})

\#由於資料管理員(部門行政人員)並不在工程師名單中，因此需要另外加進去。

\#此外英文拼音在KM中可以完全解析，而中文名稱則更直接。

dfA = read\_csv(\'../pub\_count.csv\')

dfA.loc\[len(dfA), \'emp\'\] = \'3851\'

dfA.loc\[dfA.emp == \'3851\', \'nam\'\] = \'yangzhenzhen\'

name\_cn = \[list(dfA.loc\[dfA.emp == i, \'nam\'\])\[0\] for i in
df.empno\]

name\_en = \[\]

for i in name\_cn:

ss = lazy\_pinyin(i)

s = \'\'

if len(ss) \> 0:

for j in ss:

s += j

name\_en.append(s)

df.set\_index(\'empno\').to\_csv(\'email.csv\')

\#產生程式碼、寫出程式檔案

c, n, q = \',\', \'\\n\', \'\"\'

imp = \'import com.openkm.api.\*;\'

user = \[\'OKMAuth.getInstance().createUser(null, \', \' true);\\n\'\]

role = \[\'OKMAuth.getInstance().assignRole(null,\', \');\\n\'\]

delu = \[\'OKMAuth.getInstance().deleteUser(null,\', \');\\n\'\]

user\_id=\[q+i+q+c for i in empno\]

user\_pw=\[q+e.split(\'@\')\[0\]+i+q+c for i,e in zip(empno,email)\]

user\_em=\[q+e+q+c for e in email\]

user\_nm=\[q+e+q+c for e in name\_cn\]

with open(\'addusers.bsh\', \'w\') as f:

f.write(\'import com.openkm.api.\*;\'+n)

for i in range(len(user\_id)):

id,pw,em,nm=user\_id\[i\],user\_pw\[i\],user\_em\[i\],user\_nm\[i\]

f.write(user\[0\]+id+pw+em+nm+user\[1\])

with open(\'asgroles.bsh\', \'w\') as f:

f.write(\'import com.openkm.api.\*;\'+n)

for i in range(len(user\_id)):

id=user\_id\[i\]

\# if id==\'3886\' or id==\'6790\':continue

f.write(role\[0\]+id+q+\'ROLE\_USER\'+q+role\[1\])

with open(\'del\_users.bsh\', \'w\') as f:

f.write(\'import com.openkm.api.\*;\'+n)

for i in range(len(user\_id)):

id=user\_id\[i\]\[:-1\]

f.write(delu\[0\]+id+delu\[1\])

注意：目前尚未找到是否有"createRole指令，因此如果要新增角色或群組，可能還是必須手動新增，再由程式指定。

#### 執行批次檔

在「管理面板」開啟scripting，將程式檔Load進來，或者是直接選取、複製貼在黃色記事板上，按下「Evaluate」即執行。若無錯誤訊息，且顯示執行時間，則為成功。

#### 驗證執行結果

開啟Users檢視是否如預期新增使用者並完成角色指定。

### 批次上、下載

openKM的批次上、下載只開放在「管理面板」，因此一般使用者是無法使用的。而openKM並不是一般的檔案系統，因此也無法以檔案總管、OS等外掛方式來設定。上載(import)畫面如下，上載也類似，只是順序不同。

![](media/image15.png){width="5.799305555555556in"
height="4.642361111111111in"}

#### openKM一般進、出目錄是按左邊的目錄名稱，確定要上、下載(Select)是按右側的綠色勾。

#### 由於程式限制，目前版本是不會辨認中文目錄的。因此不論是來源或者是目標目錄名稱，都必須先修改成英數文字才能辨識。上、下載後再恢復中文目錄名稱。

#### 批次上、下載是經由localhost的作業系統連結到檔案系統，因此檔案目錄必須是在sino4上，或者是sino4可以連結到的位置。由於sino4檔案系統與公司其他系統之間是存在界面的，傳檔的方案有：

##### window系統經由sftp與sino4進行檔案上、下載到openkm使用者目錄下，或者其他公共空間(目前在sino4:/home下的公共空間有aermod、backup、camxruns、cybee、py\_exam等)，要注意openKM的權限範圍是使用者openkm範圍，不能存取其他私人目錄。

##### 先建立SAMBA由window系統複製到工作站(master)，sino4再經由/autofs/master來抓取資料。由於autofs的特性是動態連結，須先以openkm身份在sino4上存取/autofs/master內容，系統才知道有哪些目錄，才會在openKM系統中顯示。

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

[^9]: https://docs.openkm.com/kcenter/view/okm-6.3-com/installing-on-redhat-and-centos.html

[^10]: OpenKM安装（CentOS6）[[http://www.openkm.net/1409.html]{.underline}](http://www.openkm.net/1409.html)

[^11]: File:OpenKM 6
    zh-TW.sql[[https://www.openkm.com/wiki/index.php/File:OpenKM\_6\_zh-TW.sql]{.underline}](https://www.openkm.com/wiki/index.php/File:OpenKM_6_zh-TW.sql)

[^12]: 說明MySQL如何修改密碼與忘記密碼時如何重設密碼[[https://emn178.pixnet.net/blog/post/87659567-mysql]{.underline}](https://emn178.pixnet.net/blog/post/87659567-mysql)修改密碼與忘記密碼重設

[^13]: ScaleBuz, WHAT IS JBOSS AND HOW TO INSTALL JBOSS ON CENTOS 6 ?
    [[https://scalebuzz.com/what-is-jboss-and-how-to-install-jboss-on-centos-6/]{.underline}](https://scalebuzz.com/what-is-jboss-and-how-to-install-jboss-on-centos-6/)

[^14]: Configure JBoss service,
    [[https://www.openkm.com/wiki/index.php/Configure\_JBoss\_service]{.underline}](https://www.openkm.com/wiki/index.php/Configure_JBoss_service)

[^15]: [[https://developers.redhat.com/products/eap/download]{.underline}](https://developers.redhat.com/products/eap/download)

[^16]: /opt/jboss-eap-6.3/bin/standalone/configuration/standalone.xml

[^17]: As is explained in wiki and in some forum post, all users must
    have UserRole ( mandatory ) is a role for connection that must not
    be propagated to repository because all users have it. It\'s
    administrator job to change default okm:root security configuration
    for it purpose and add there new roles,
    https://forum.openkm.com/viewtopic.php?t=4024

[^18]: Creating users from scripts,
    https://docs.openkm.com/kcenter/view/okm-6.3-com/creating-users-from-scripts.html

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
