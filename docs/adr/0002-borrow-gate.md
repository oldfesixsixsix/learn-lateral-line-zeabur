# 進書不設門檻,登入時順手記一筆借閱

**Status**: accepted

**背景**:`library` 的借閱系統(見 `oldfesixsixsix/library` 的 `docs/adr/0003-borrowing-system.md`)決定內容不設存取門檻,登入只是順手記一筆借閱紀錄,借閱狀態查核由各下游 Worker 自己做,不是 `library` 當 gateway 擋在前面轉發。

**決定**:
- `worker.js` 加 JWT 驗證(驗 `Path=/library` cookie 裡的 token),但驗證結果**不影響放行**——沒帶 cookie、cookie 無效、或驗證失敗,都一樣正常顯示內容。
- 只有驗證成功時才順手查/寫 `loan` 表:沒有這本書的有效借閱紀錄、且名額還夠,就自動記一筆;名額滿了就跳過不記,一樣放行看內容。
- 跟 `library` 共用同一個 D1 資料庫(`book`/`loan` 表)。`BOOK_ID = "lateral-line"`,01、02 兩篇共用同一本書、同一個借閱名額上限,不是各自獨立登記。

**後果**:
- 從「純靜態、沒有 API」變成需要 JWT 驗證 + D1 寫入,但這層複雜度只影響「記不記借閱」,不影響內容是否放行,出錯的風險上限是「漏記一筆」,不是打不開內容。
- `book` 表裡這本書的目錄資料(title/subtitle/category/tags/path/capacity)要由 owner 到 `library` 的 `/admin` 後台手動登記,不是這裡的 migration 或程式碼處理的範圍。
