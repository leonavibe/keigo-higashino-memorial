# HANDOFF — keigo-higashino-memorial（東野圭吾作品紀念館）
更新：2026-07-31／claude

## 目前目標
單檔靜態網站，整理東野圭吾 108 部作品的簡介、系列脈絡、影視改編與台灣購書／OTT 連結。
已於 GitHub Pages 公開上線：https://leonavibe.github.io/keigo-higashino-memorial/

## 狀態
- 已完成：資料層與互動層全數驗收通過——slug／id 無重複、11 個系列宣告數與實際筆數全對、
  hasTv(71) 與改編清單完全一致、110 筆改編都對得到作品、無本機路徑或金鑰殘留
- 已完成：網址佔位符換為 `leonavibe.github.io/keigo-higashino-memorial`（index.html／robots.txt／
  sitemap.xml 共 8 處），換後 DATA 與 ld+json 重新解析通過
- 已完成：補上 `.nojekyll`（DEPLOY.md 列為建議項）
- 已完成：2026-07-31 建 public repo `leonavibe/keigo-higashino-memorial` 並開啟 Pages
  （main / root），線上驗證 `/`、`/robots.txt`、`/sitemap.xml`、`/og-image.jpg` 皆 200、
  http 自動 301 轉 https、canonical 與 sitemap 指向正確、404 頁正常
- 已完成：`og:image`／`twitter:image` 由相對路徑改為絕對網址（FB、LINE 官方要求）
- 進行中：無
- 驗收現況：Chrome 實機驗收全綠（2026-07-31）——搜尋中文／日文／簡介、無結果空狀態、
  系列篩選(加賀 13 筆)、只看有改編(71 筆)、作品彈窗、ESC 關閉、深淺色切換、深層連結 #w-<slug>
  皆正常；console 零錯誤（唯一例外來自瀏覽器擴充功能，非本站）

## 下一步（接手的人從這裡開始）
1. 貼網址到 FB／LINE 確認預覽圖出得來；沒出圖用 FB 分享偵錯工具按 Scrape Again
2. Google Search Console 驗證所有權並送出 sitemap；Rich Results Test 驗結構化資料
3. （選配）替「107 部」加一句口徑說明，讓讀者知道 108 筆中的繪本未計入

## 地雷（別踩）
- **107 vs 108 是刻意的，不要「修正」**：`works` 陣列 108 筆，首頁 stats 寫「107 部」，
  拆開是 長篇75＋短篇集27＋隨筆5＝107，第 108 筆是繪本《聖誕婆婆》不計入。
  使用者 2026-07-31 裁定保留此口徑。作品總表標題仍顯示「108 / 108 部」，兩數並存屬預期
- 作品總表欄寬是 `table-layout:fixed` + `.tbl-wrap th/td:nth-child(n)` 百分比手動指定，
  合計必須 100%；增刪欄要同步改。**表格 CSS 一律寫在 `.tbl-wrap` 底下**——彈窗的
  `.edtbl` 是獨立表格，曾因全域 `table{min-width:820px}` 洩漏而長出橫向捲軸
- **元件樣式一律加容器前綴**：已踩過兩次通用選擇器外洩——`table{min-width:820px}` 把彈窗
  表格撐出捲軸；新聞卡的 `.nw` 撞到表格 no-wrap 的 `.nw`，讓日期格長出邊框與底色。
  現已改為 `.tbl-wrap table`、`.news .nw`，表格 no-wrap 改名 `.nbr`
- 出版類別／出版平台／影音平台的選項都由資料即時推導（只列出實際有內容的），
  不是寫死清單。`platsOf()` 必須與彈窗的 `edLinks()` 保持同一套判定，否則會出現
  「篩得到卻點不到連結」
- 全站資料都在 index.html 內嵌的 `const DATA = {...}` **單獨一行**（約 1000 行檔案裡的一行）。
  不要手動編輯那行，用 DEPLOY.md 末尾的 Python 腳本改
- CSP 是 `default-src 'none'`，只放行 inline script／style 與 Google Fonts。
  加任何外部資源都要同步改 `<meta http-equiv="Content-Security-Policy">`，否則會被靜默擋掉
- 搜尋到的 `TODO`／`PLACEHOLDER`／`待補` 多半是 CSS class 名、HTML placeholder 屬性、
  或 UI 的 fallback 文案，不是未完成標記；`NaN` 是撞到 `naniwa-2012`、`sk-` 是撞到 `mask-2019`
- 5 部作品無台灣版本與購書連結（遺作《永遠的記憶》、《微不足道的蓄意》、《假面人生》、
  《夢想奔馳在都靈》、《聖誕婆婆》），UI 會顯示「待補」標籤——這是真實現況不是 bug
- 改編資料有 1 筆 `verified:false`（毒笑小說〈殺意使用說明書〉，播出年份待確認），
  另 1 筆《解憂雜貨店》韓國版導演姓名待確認，兩者都會顯示給讀者
- 母 repo（~/Code，miku4ocean/code-workspace）的 .gitignore 用 `*/` 忽略本目錄，
  本專案是獨立 repo，git 操作不會互相污染
- 本 repo 用 leonavibe 身分（repo-local git config），與其他專案的 miku4ocean 不同。
  操作前先 `gh auth switch --user leonavibe`

## 待補（原作者確認）
- 資料來源與整理方式（作品簡介、演員角色表出自何處）／是否有授權疑慮
- 是否長期維護、更新頻率

## 主辦權
單線（claude）
