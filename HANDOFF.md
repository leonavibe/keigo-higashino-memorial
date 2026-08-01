# HANDOFF — keigo-higashino-memorial（東野圭吾作品紀念館）
更新：2026-08-01／claude

## 目前目標
單檔靜態網站，整理東野圭吾 108 部作品的簡介、系列脈絡、影視改編與台灣購書／OTT 連結。
已於 GitHub Pages 公開上線：https://leonavibe.github.io/keigo-higashino-memorial/

## 狀態
- 已上線：public repo + Pages（main / root），四個入口皆 200、http 301 轉 https、
  canonical／sitemap／og:image 全指向正式網址
- 已完成：作品總表改用「出版類別＋出版平台」篩選並加「有台灣出版」；影視改編加搜尋框、
  影音平台篩選、「有平台上架」勾選，移除筆數，白夜行(tbd)置頂
- 已完成：接上 GoatCounter 瀏覽計數（無 cookie），CSP 依 count.js 實際行為最小放行
- 已完成：CSS 撞名稽核（183 條規則跨元件比對，無殘留）＋清掉 14 個死 class／27 條規則
- 已完成：Search Console 驗證＋要求建立索引；FB／LINE 預覽圖正常；GoatCounter 已收到資料；
  ld+json 經 Schema Validator 零錯誤零警告（Rich Results Test 回報「未偵測到項目」屬正常，
  Person／WebSite 本就不產生複合式結果）
- 進行中：無
- 驗收現況：Chrome 實機全綠（2026-08-01）——兩區搜尋與全部篩選組合實測筆數與資料層吻合、
  彈窗／ESC／深淺色／深層連結正常、66 個彈窗表格零溢出；console 零錯誤
  （唯一例外來自瀏覽器擴充功能，非本站）

## 下一步（接手的人從這裡開始）
1. Search Console 的 sitemap 那列填成 `/sitemap.xml`（多了開頭斜線），Google 因此去抓
   `leonavibe.github.io/sitemap.xml`(404) 而顯示「無法擷取」。該列刪不掉也送不出新的，
   疑似瀏覽器擴充干擾——用無痕視窗重送，只填 `sitemap.xml` 不加斜線。
   **不急**：robots.txt 已有絕對網址的 `Sitemap:` 宣告，Google 照樣找得到
2. （選配）替 106／107／108 三個數字加一句對讀者的口徑說明（純體驗考量，資料本身無誤）

## 地雷（別踩）
- **106／107／108 三個數字都對，不要校正也不要再問**（使用者 2026-08-01 確認並要求跳過）：
  106（分享圖）＝生前已發表的作品；107（首頁 stats）＝106＋8/5 上市的遺作《永遠的記憶》，
  是「作品數」的正確值；108（作品總表）＝107＋《微不足道的蓄意》——那是講談社文庫選三篇
  舊作的重編選集，非新作，但日本官網獨立列為一部，故條目數多一
- **元件樣式一律加容器前綴**——已踩過兩次通用選擇器外洩：`table{min-width:820px}` 把彈窗
  表格撐出橫向捲軸；新聞卡的 `.nw` 撞到表格 no-wrap 的 `.nw`，讓日期格長出邊框與底色。
  現為 `.tbl-wrap table`、`.news .nw`，表格 no-wrap 已改名 `.nbr`
- 作品總表欄寬是 `table-layout:fixed` + `.tbl-wrap th/td:nth-child(n)` 百分比指定，
  合計必須 100%；增刪欄要同步改。彈窗 `.edtbl` 是獨立表格，兩者不共用規則
- 出版類別／出版平台／影音平台的選項都由資料即時推導（只列出實際有內容的），
  不是寫死清單。`platsOf()` 必須與彈窗的 `edLinks()` 保持同一套判定，否則會出現
  「篩得到卻點不到連結」
- 全站資料都在 index.html 內嵌的 `const DATA = {...}` **單獨一行**（約 1000 行檔案裡的一行）。
  不要手動編輯那行，用 DEPLOY.md 末尾的 Python 腳本改
- CSP 是 `default-src 'none'`，逐項白名單：inline script／style、Google Fonts、
  gc.zgo.at(GoatCounter 腳本)、leonavibe.goatcounter.com(connect-src 與 img-src，
  分別對應 sendBeacon 與其 img 後備)。**加任何外部資源都要同步改那個 meta**，否則靜默被擋
- 搜尋到的 `TODO`／`PLACEHOLDER`／`待補` 多半是 CSS class 名、HTML placeholder 屬性、
  或 UI 的 fallback 文案，不是未完成標記；`NaN` 是撞到 `naniwa-2012`、`sk-` 是撞到 `mask-2019`
- 5 部作品無台灣版本與購書連結、1 筆改編 `verified:false`、2 處「待確認」字樣會顯示給
  讀者——都是真實現況不是 bug
- 母 repo（~/Code）以 `*/` 忽略本目錄，本專案是獨立 repo，git 操作不互相污染
- 本 repo 用 leonavibe 身分（repo-local config）。`gh` 的 active 帳號是**全域**狀態、
  會被其他工作階段切走，push 前先 `gh auth switch --user leonavibe`（已被 403 擋過一次）

## 待補（原作者確認）
- 資料來源與整理方式（作品簡介、演員角色表出自何處）／是否有授權疑慮
- 是否長期維護、更新頻率

## 主辦權
單線（claude）
