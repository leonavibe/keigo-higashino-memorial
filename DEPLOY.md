# 部署到 GitHub Pages

## 檔案清單

| 檔案 | 用途 | 可否省略 |
|---|---|---|
| `index.html` | 網站本體，資料與程式都內嵌在裡面 | 否 |
| `og-image.jpg` | 分享預覽圖（1200×630），必須與 index.html 同一層 | 否 |
| `robots.txt` | 允許搜尋引擎、拒絕 AI 訓練與整站抓取工具 | 否 |
| `sitemap.xml` | 給搜尋引擎的網址清單 | 否 |
| `404.html` | 找不到頁面時的畫面 | 可 |
| `.nojekyll` | 關掉 GitHub 的 Jekyll 處理，避免底線開頭的檔案被吃掉 | 建議留 |

---

## 上線前必做：換掉網址佔位符

三個檔案裡都有 `YOUR-USERNAME.github.io/higashino-memorial` 這個假網址，**部署前一定要換成實際網址**，否則 canonical 會指到不存在的地方，反而傷 SEO。

在專案資料夾執行（把 `你的帳號` 和 `你的repo` 換掉）：

```bash
sed -i '' 's|YOUR-USERNAME.github.io/higashino-memorial|你的帳號.github.io/你的repo|g' index.html robots.txt sitemap.xml
```

Linux 的話去掉 `-i` 後面的 `''`：

```bash
sed -i 's|YOUR-USERNAME.github.io/higashino-memorial|你的帳號.github.io/你的repo|g' index.html robots.txt sitemap.xml
```

換完檢查一次，應該一筆都找不到：

```bash
grep -r "YOUR-USERNAME" .
```

> 如果之後要接自訂網域，同一組指令再跑一次，把 `你的帳號.github.io/你的repo` 換成 `example.com` 即可，另外記得在 repo 根目錄放一個 `CNAME` 檔案，內容就是網域本身。

---

## 部署步驟

```bash
git init
git add .
git commit -m "東野圭吾作品紀念館"
git branch -M main
git remote add origin https://github.com/你的帳號/你的repo.git
git push -u origin main
```

接著到 repo 的 **Settings → Pages**：

- Source 選 **Deploy from a branch**
- Branch 選 **main**、資料夾選 **/ (root)**
- 存檔後等一兩分鐘，網址會顯示在同一頁

---

## 上線後檢查

1. 開網址，確認深色／淺色切換、搜尋、篩選、作品彈窗都正常
2. 把網址貼到 Facebook 或 LINE，確認預覽圖出得來
   - 沒出圖的話去 [Facebook 分享偵錯工具](https://developers.facebook.com/tools/debug/) 按 Scrape Again
3. 開 `你的網址/robots.txt`、`你的網址/sitemap.xml`，確認讀得到
4. 到 [Google Search Console](https://search.google.com/search-console) 驗證網站所有權，送出 sitemap
5. 用 [Rich Results Test](https://search.google.com/test/rich-results) 檢查結構化資料有沒有錯

---

## 之後要改內容時

所有資料都在 `index.html` 裡的 `const DATA = {...}` 這一行（很長的一行 JSON）。改完直接 commit + push，GitHub Pages 會自動重新部署。

改動比較大時，建議用腳本改而不是手動編輯那一行：

```python
import re, json
p = 'index.html'
src = open(p, encoding='utf-8').read()
m = re.search(r'const DATA = (\{.*?\});\n', src, re.S)
D = json.loads(m.group(1))

# ... 在這裡改 D ...

src = src[:m.start()] + 'const DATA = ' + json.dumps(D, ensure_ascii=False) + ';\n' + src[m.end():]
open(p, 'w', encoding='utf-8').write(src)
```

改完可以順手驗一次資料完整性：slug 不重複、`hasTv` 與改編清單一致、系列 `count` 與實際筆數相符。
