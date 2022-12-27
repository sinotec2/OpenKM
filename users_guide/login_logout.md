---
layout: default
title: 系統登入與登出
parent: Users Guide
nav_order: 1
date: 2022-12-27
last_modified_date: 2022-12-27 11:00:45
---

# 系統登入與登出

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>
---

## 背景

- 使用手冊除了官網([6.4][6.4]、[7.1][7.1])之外，亦有泰國機關所摘寫的英文版手冊([OKM6.4][OKM6.4])

## 登入系統

### 網址

- 開啟瀏覽器(chrome/firefox/IE/safari等)
  1. 網址: http://200.200.121.74:8080/OpenKM
  2. 或網址: http://200.200.12.191:8080/OpenKM
- 說明：
  1. 開頭200.200.XXX.XXX均為「內部」網站，與公司資訊安全之管理(檔案進出公司防火牆)「無關」下載後送出公司外部時、或由外部獲得資料上載到KM，進出公司之際還是必須遵守公司資安設定。
  2. 12.191(master)為備份、屬試驗性質，必要時才開放，請勿存放文件。

![](https://github.com/sinotec2/Focus-on-Air-Quality/raw/main/assets/images/CAM-chemSpec.png)


### 使用者帳密

1. 名稱:員工編號(4碼)，例如4139
2. 密碼:email的帳號(@左方之名稱)加上員編，例如:yckuang@mail.sinotech-eng.com，密碼就先設成yckuang4139。
3. 登入後在「工具」「選項」中修改。
4. 「第一次」瀏覽器會詢問是否儲存密碼：
  - 如果是個人桌機，此處強列建議儲存密碼，會省事很多。
  - 如果瀏覽器不能儲存密碼，可能原因：
    1. 瀏覽器設定為對所有網站、永不儲存密碼，或
    2. 瀏覽器設定對特定網站不儲存密碼(第一次詢問時答否，瀏覽器就會被設定永不儲存該一網站的密碼)，
    3. 必須進入瀏覽器的「設定」中將儲存密碼打開，或在永不儲存特定網站處，將本網站排除。
5. openKM系統中修改密碼：

- 進入系統後
  1. 在「工具」(Tools)→
  2. 「選項」(Preferences)→
  3. 「用戶配置」，可更改個人密碼、信箱(下述)等設定。
- 唯「角色」的設定是管理者權限範圍，被分配到某一(些)群組，須由管理者來修改。

### 語言

- 有4種可以選。
- 主要影響為畫面指令
- 即使選英文、檔名如為中文字也可以顯示，只有在進行翻譯是才會有差別。

###	登入

- 按下Login鍵之後就可以登入了。
- 重複登入是否造成錯誤
  - 系統允許重複、多端口登入、可以方便檢視。
  - 因系統為即時更新，即使重複登入，系統只會回應最新的更新、修改等指令。

[6.4]: <https://docs.openkm.com/kcenter/view/okm-6.4/> "OpenKM(2016)Documentation for OpenKM"
[7.1]: <https://docs.openkm.com/kcenter/view/okm-7.1/> "OpenKM(2016)Documentation for OpenKM"
[OKM6.4]: <https://www.seameo.org/seameoweb2/images/stories/Programmes_Projects/OpenKM/OpenKM%20User%20Guide.pdf> "Southeast Asian Ministers of Education Organization Secretariat (SEAMEO Secretariat) OpenKM Users Guide"
