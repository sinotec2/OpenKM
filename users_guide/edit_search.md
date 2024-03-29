---
layout: default
title: 搜索與編輯
parent: Users Guide
nav_order: 4
date: 2022-12-27
last_modified_date: 2022-12-27 16:30:56
---

# 搜索與編輯

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

- 雖然OpenKM的設計並不是為著文件的編輯，然而系統中也提供了一個編輯的邏輯，所謂的編輯就是
  1. 下載一份文件到自己的電腦上，
  2. 同時加上「編輯中」的註記，
  3. 在本地編輯完成後，再上載覆蓋原來檔案。

## 編輯檔案

- 雙擊並非單純下載，乃為開啟編輯。
- 再檔案的上方按下右鍵，即會下載一份到本地電腦
- 被設定編輯的檔案，會再前方出現紙張和鉛筆，別人即無法修改，並會在個人面板及下方出現編輯檔案數字。
- 雙擊後即會下載到個人電腦的「下載」目錄。
- 修改完後，再OpenKM原來檔案位置、點選檔案、按右鍵選更新Update，同樣進入上載畫面，留下註解，
- 將會覆蓋原來檔案。
- 注意
  - 系統不會確認本地電腦究竟能不能編輯檔案。如PDF檔。
  - 

## 搜尋功能

- 此處說明的是全文查找，文件或文件夾的查找，可以詳見[資料夾及文件查找](https://sinotec2.github.io/OpenKM/users_guide/desktop/#資料夾及文件查找)
- 全文搜索可以在右方放大鏡中執行，也可以在「文件搜索」頁面中進行進階搜索。
- 「文件搜索」頁面右上方選擇搜尋與結果顯示的方式，中間則是搜尋的條件，有基本、進階、與詮釋資料搜尋等。
- 「文件搜索」頁面右下方為搜索結果，找到檔案之後，點選訂閱(subscribe)就會在個人看板中出現了。
- 也可以將滑鼠移到檔名底線，會出現檔案所在的目錄歸屬，單擊則會進入該目錄。點選檔案名稱，會進入「桌面」可以點選預覽畫面。
- 搜索結果儲存的方式有2種(Stored searches)
  - 保存搜索
    - 如果特別勾選「作為用戶信息保存」填寫名稱後，點選「保存搜索」就會存到左側。
    - 下次進入後，點選該名稱後，就會出現過去搜索的結果，同時也會出現在「個人看板」上。
  - 如果沒有勾選「作為用戶信息保存」，就只會出現在左側上方。

![search2](https://github.com/sinotec2/OpenKM/blob/gh-pages/assets/image/search2.png?raw=true)

### 中文查找

- 中文字詞的前後要加雙引號及星號如`"*健康*"`，
- 如為分開的2個詞，每個都需要加上雙引號及星號如`"*健康*","*評估*"` (結果會是交集)
  