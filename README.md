# 我的收藏圖鑑（GitHub Pages 範本）

一個放在 GitHub Pages 的靜態收藏圖鑑網站。資料和圖片**全部放在自己的 repo**，
所以圖片走 GitHub 的 CDN，不會像免費圖床那樣時好時壞。

## 檔案結構

```
my-dex/
├── index.html      ← 網站本體（不用改，除非要調樣式）
├── data.json       ← 你的卡片資料（要編輯的就是這個）
├── images/         ← 所有圖片放這裡
└── README.md       ← 本說明
```

## 一、怎麼上線到 GitHub Pages

1. 在 GitHub 建一個**公開（Public）** repo，例如 `my-dex`。（免費帳號的 Pages 只支援公開 repo）
2. 把這個資料夾裡的所有檔案上傳進去（index.html、data.json、images/ 整個資料夾）。
3. repo 頁面 → **Settings → Pages** → Source 選 `Deploy from a branch`，
   Branch 選 `main`、資料夾選 `/ (root)`，按 Save。
4. 等 1～2 分鐘，網址就是 `https://<你的帳號>.github.io/my-dex/`。

## 二、怎麼加／改卡片（編輯 data.json）

`data.json` 的結構：

```json
{
  "siteTitle": "我的收藏圖鑑",      // 網站標題
  "subtitle": "來幫寶寶點點名 ✧",   // 副標
  "logo": "images/logo.svg",       // 上方 logo 圖
  "tabs": [
    {
      "name": "兔兔",                // 分頁名稱
      "icon": "images/icon-usagi.svg", // 分頁小圖示
      "color": "#f2a7bd",           // 這個分頁的主題色（勾選邊框、分頁底色）
      "items": [
        {
          "type": "娃娃",            // 類型（同類型會被分在一組）
          "name": "兔兔 S",          // 卡片名稱
          "img": "images/usagi-s.svg", // 圖片路徑（相對於網站根目錄）
          "size": "10cm"            // 尺寸（顯示在名稱下方，可留空 "")
        }
      ]
    }
  ]
}
```

**加一張卡片** = 在對應分頁的 `items` 陣列裡多加一個 `{ ... }`。
**加一個角色分頁** = 在 `tabs` 陣列裡多加一個 `{ name, icon, color, items }`。

> 注意 JSON 格式：每個項目之間要有逗號，最後一個項目後面**不要**加逗號，
> 否則整頁會讀不出來。改完可貼到 <https://jsonlint.com> 檢查格式。

## 三、怎麼放圖片

1. 把圖片檔（.png / .jpg / .webp / .svg 都可以）放進 `images/` 資料夾。
2. 在 `data.json` 裡把 `img` 指到它，例如圖片叫 `images/兔兔大娃.png`，就寫
   `"img": "images/兔兔大娃.png"`。
3. 建議：圖片先壓縮到寬度約 300～500px、每張 < 100KB，載入更快。

## 四、圖片為什麼比原本那種穩

`index.html` 裡的 `loadImage()` 做了三件事：

1. **延遲載入**（`loading="lazy"`）：捲到才載，不會一次幾十張同時打，降低尖峰失敗。
2. **自動重試**：載失敗會隔 0.5 秒、1 秒重試共 2 次。
3. **佔位圖**：真的載不出來就顯示灰色「圖片載入失敗」，不會破圖。

加上圖片放在自己 repo（同源、GitHub CDN），失敗率會遠低於用 postimg.cc 之類的免費圖床。

## 五、本機預覽（可選）

直接雙擊 `index.html` 會因為瀏覽器安全限制讀不到 `data.json`。
要在本機預覽，先在這個資料夾開一個小伺服器：

```
python -m http.server 8777
```

再用瀏覽器開 `http://localhost:8777/`。（上線到 GitHub Pages 後就沒這問題。）
