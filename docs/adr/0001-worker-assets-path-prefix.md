# Worker + assets 部署,掛在 unclereal.com/library/lateral-line

**Status**: accepted

**背景**:完整背景與考慮過的替代方案見 `library` 專案的 ADR-0001(同一次集中式路徑前綴改版的一部分,`learn-lateral-line-zeabur` 是其中一個獨立掛載的子站)。跟 `jwks`、`retirement-planning-101` 不同的是,這個專案是新建的,沒有舊的 Pages 部署要migrate。

**決定**:
- 直接以 Worker + assets 部署(`wrangler.jsonc` + `assets.directory`),不走 `wrangler pages deploy` / Git 自動部署。
- Worker Route 設為 `unclereal.com/library/lateral-line*`(`zone_name: "unclereal.com"`),優先權高於 `library` 本身的 catch-all。
- 不跟 `library`、`learn-jwks`、`retirement-planning-101` 合併成單一 Worker,維持獨立部署生命週期。
- 內容放進 `web/<entry-slug>/index.html`(資料夾 + index.html),不是平面的 `<entry-slug>.html`——這是為了讓 Workers Assets 預設的 `html_handling`(對資料夾 `index.html` 才會對應結尾帶斜線的網址)剛好產出 `/library/lateral-line/01-zeabur-2026-08/` 這種目錄式網址,不用額外寫轉址規則。

**後果**:
- 之後要加新一篇,照同樣的「資料夾 + index.html」慣例放進 `web/` 底下即可,不需要改 Worker 邏輯。
- 跟其他兩個子站一樣,要手動(或另外設定 Workers Builds)跑 `wrangler deploy`,沒有 Git push 自動部署。
