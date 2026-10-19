# Lateral Line

Zeabur 兩次資安事故的觀察文章,掛在 `unclereal.com/library/lateral-line`。

## 檔案

```
web/
  index.html                      文章列表頁
  01-zeabur-2026-08/index.html    第一篇：一把鑰匙走了多遠
  02-zeabur-2026-10/index.html    第二篇：門修好了，人還在嗎
worker.js        路徑前綴剝除 + 借閱記錄（見 docs/adr）
wrangler.jsonc   Worker + assets 設定
```

加新一篇的慣例：在 `web/` 底下新增一個 `<slug>/index.html` 資料夾,不用改 `worker.js`。

## 開發

```
npm install
npm run dev
```

## 部署

```
npm run deploy
```

部署到 `unclereal.com/library/lateral-line*`,細節見 `docs/adr/0001-worker-assets-path-prefix.md`、`docs/adr/0002-borrow-gate.md`。

部署後要到 `library` 的 `/library/admin` 後台登記書目(title/subtitle/category/tags/path=`/library/lateral-line`/capacity),才會出現在 `library` 首頁的書目列表——這一步是 owner 手動操作,不含在這個 repo 的部署流程裡。
