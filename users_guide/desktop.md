---
layout: default
title: 桌面、搜尋及看板
parent: Users Guide
nav_order: 2
date: 2022-12-27
last_modified_date: 2022-12-27 14:27:14
---

# 桌面、文件搜索、個人看板

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

- 登入openKM後畫面(`桌面`)第一行為主功能表、下拉選單
  - `文件(Files)`與一般軟體之`檔案`功能類似
  - `編輯(Edit)`有關檔案安全性、編輯、屬性、與訂閱
  - `工具(Tools)`系統語言、外觀、選項等設定
  - `收藏夾(Bookmarks)`會記錄看過文件的位置，
  - `樣板(Templates)`功能未開放。
- 第二行左側為小工具，下載、列印、開目錄、上傳檔案等等
- 第二行右側有3個頁面(tab)
  - `桌面(Desktop)`、`文件搜尋(Search)`、`個人看板(Dashboard)`，可以自由切換。
  - 管理者另還有`管理面板(Admins)`，來控制整個系統。

![entry_adm1](https://github.com/sinotec2/openKM/blob/gh-pages/assets/image/entry_adm1.png?raw=true)

## 桌面(Desktop)

- 包括左側[目錄架構](#目錄架構)、檔案列表、檔案內容(Preview)等3個區塊與介面。

### 目錄架構

- 「公共文件」(Taxonomy知識分類學)：按檔案內容的知識屬性分類 
  - 建議至少按照Sinotech KM分法，以承接其內容，有必要則另創。
  - Sinotech KM分法
    - 污水處理廠、污水管線工程、自來水工程、水污染整治
    - 環境影響評估及監測 、噪音污染評估防制及設計、空氣污染防制及健康風險、溫減碳排等
    - 土壤及地下水污染調查評估與整治
    - 廢棄物管理規劃、廢棄物處理設施營管服務、產業能資源整合輔導、焚化廠效能提昇整建 
- 「自定義分類」(categories)
  - 可自創檔案夾，不能新增檔案，只能連結。
  - 按屬性、可跨組、共通性的檔案放在這裡。
  - 「新建資料夾」→到「我的文件」選取文件→按右鍵「加以自定義分類」→選取該資料夾。類似公開筆記或個人書架的概念。
- 「詮釋資料」(metadata)將檔案予以評等、分級之後，將在此處彙整。可視為經評鑑後的檔案。
- 「詞典」(Thesaurus同義詞，用來提供關鍵詞選擇用)
- 「樣板」(Template功能未知)
- 「我的文件」存放個人文件的地方，除了ROLE_ADMIN之外，即使開放別人也讀不到，必須複製到公共空間。
- 電子郵件
  - 電子郵件的設定在「工具」→「選項」→「用戶配置」，範例：
  - master /sino4都具有寄信功能

```bash
(i) mail server: mail.sinotech.com.tw
(ii) Mail user name:openkm
(iii) Mail user password:yck4139
(iv) Mail folder:/var/spool/mail
```

- mail.sinotech.com.tw(mail.sinotech-eng.com或imap.gmail.com)(如下圖, deprecated)

```bash
(i) mail server: mail.sinotech.com.tw
(ii) Mail user name:yckuang
(iii) Mail user password:***** (在申請email帳號時設定的密碼)
(iv) Mail folder:Inbox
```

- 經測試必須「Mail configuration OK」


![dash_board](https://github.com/sinotec2/openKM/blob/gh-pages/assets/image/dash_board.png?raw=true)