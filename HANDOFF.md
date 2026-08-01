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
- 已完成：接上 GoatCounter 瀏覽計數（無 cookie），CSP 依 count.js 實際行為最小放行；
  CSS 撞名稽核（183 條規則跨元件比對）＋清掉 14 個死 class／27 條規則
- 已完成：Search Console 驗證＋要求建立索引；FB／LINE 預覽圖正常；GoatCounter 已收到資料；
  ld+json 經 Schema Validator 零錯誤零警告（Rich Results Test 回報「未偵測到項目」屬正常，
  Person／WebSite 本就不產生複合式結果）
- 已完成：購書連結全站稽核——1003 個連結逐一實機驗證，43 部作品補上 47 個缺漏版本、
  修正 3 個標錯的「初版」；可點連結 924→1001、資料有卻渲染不出來的 77→0
- 進行中：無
- 驗收現況：Chrome 實機全綠（2026-08-01）——兩區搜尋與全部篩選組合實測與資料層吻合、
  彈窗／ESC／深淺色／深層連結正常、彈窗表格零溢出；console 零錯誤（唯一例外來自擴充功能）

## 下一步（接手的人從這裡開始）
1. Search Console 的 sitemap 那列誤填成 `/sitemap.xml`（開頭斜線讓它去抓網域根目錄的 404），
   用無痕視窗重送、只填 `sitemap.xml`。**不急**：robots.txt 已有絕對網址的 `Sitemap:` 宣告
2. （選配）替 106／107／108 加一句給讀者的口徑說明（純體驗考量，資料本身無誤）

## 地雷（別踩）
- **106／107／108 三個數字都對，不要校正也不要再問**（使用者 2026-08-01 確認）：106＝生前
  已發表；107＝106＋遺作《永遠的記憶》，是「作品數」的正確值；108＝107＋《微不足道的
  蓄意》(舊作重編選集，非新作，但日本官網獨立列一部，故條目數多一)
- **元件樣式一律加容器前綴**——踩過兩次通用選擇器外洩：`table{min-width:820px}` 撐出彈窗
  橫向捲軸；新聞卡 `.nw` 撞到表格 no-wrap 的 `.nw`。現為 `.tbl-wrap table`、`.news .nw`，
  表格 no-wrap 改名 `.nbr`
- 作品總表欄寬是 `table-layout:fixed` + `.tbl-wrap th/td:nth-child(n)` 百分比指定，合計必須
  100%；增刪欄要同步改。彈窗 `.edtbl` 是獨立表格，兩者不共用規則
- 出版類別／出版平台／影音平台的選項由資料即時推導，不是寫死清單。`platsOf()` 必須與
  `edLinks()` 保持同一套判定，否則會出現「篩得到卻點不到連結」
- 全站資料都在 index.html 內嵌的 `const DATA = {...}` **單獨一行**（約 1000 行檔案裡的一行）。
  不要手動編輯那行，用 DEPLOY.md 末尾的 Python 腳本改
- `edLinks()` 把 `work.links` 併進 `editions[0]` 且同平台不覆蓋——出新版卻沒建 edition，
  連結就被舊版蓋掉而永遠點不到（曾累積 77 個）。**新增版本一律連 edition 一起建**
- 書店（books.com.tw／eslite／hyread）對 curl 與 WebFetch 一律 403，只有 Chrome 實機能過；
  同網域可用頁內 `fetch()` 批次抓，誠品的欄位標籤有換行要先剝空白
- CSP 是 `default-src 'none'`，逐項白名單：inline script／style、Google Fonts、gc.zgo.at、
  leonavibe.goatcounter.com(connect-src 與 img-src)。**加外部資源要同步改那個 meta**，否則靜默被擋
- 搜尋到的 `TODO`／`PLACEHOLDER`／`待補` 多半是 CSS class 名、HTML placeholder 或 fallback
  文案，不是未完成標記；`NaN` 撞到 `naniwa-2012`、`sk-` 撞到 `mask-2019`
- 5 部作品無台灣版本與購書連結、1 筆改編 `verified:false`、2 處「待確認」會顯示給讀者，
  都是真實現況不是 bug
- 母 repo（~/Code）以 `*/` 忽略本目錄，本專案是獨立 repo，git 操作不互相污染
- 本 repo 用 leonavibe 身分（repo-local config）。`gh` 的 active 帳號是**全域**狀態、會被其他
  工作階段切走，push 前先 `gh auth switch --user leonavibe`（已被 403 擋過一次）

## 待補（原作者確認）
- 資料來源與整理方式（作品簡介、演員角色表出自何處）／是否有授權疑慮
- 是否長期維護、更新頻率

## 主辦權
單線（claude）
