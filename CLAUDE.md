# 展場點單小幫手（showpad）

創二十有限公司在酒展／展場用的點單 POS。主要品牌 Soul & Spirits（S&S）。

## 架構
- 整個 app 只有一個檔案：`index.html`（約 3,900 行、400KB），HTML／CSS／JS 全部寫在裡面。不要拆成多個檔案，也不要加 build 步驟。
- 推到 `main` 後 Netlify 會自動部署，push 等於正式上線。
- 主要在 iPhone Safari 上使用，也有平板／電腦版排版。改 UI 一定要顧到 iPhone 窄螢幕。
- 雲端同步：Firebase Realtime Database（專案 `genesis20-4cdab`），用 compat SDK 在執行時載入（`loadScript`），登入用 Firebase Auth。連不上時退回 localStorage 離線使用。
- 外部載入的東西：Firebase SDK、SheetJS（xlsx 0.18.5，匯入 ECOUNT 用）、Google Fonts（Huninn）。
- 商品圖片以 base64 WebP 直接寫在檔案裡（約 190KB）。不要整段重寫或重新輸出這些字串，修改時避開它們。

## 重要規則
- 每次改版都要更新 `APP_VER`（格式：`'YYYY-MM-DDx · 一句話說明'`），方便在手機上確認是不是新版。
- 匯出的 ECOUNT 銷貨單是用「品號」逐行對應 ECOUNT 品項主檔匯入，品號格式和匯出欄位不能隨便改。
- 也有 ezPrint 出貨標籤匯出（`shipRows`）。
- Firebase 同步曾經修過不少 bug（ua 時間戳、ready/seen/prime 的 gating）。改到 `save`、`boot`、`migrate`、同步相關程式時要特別小心，不能讓資料互相覆蓋。
- LINE 查單功能（客人輸入 4 碼訂單編號）由 Firebase Cloud Function 讀同一個資料庫，訂單資料結構改了會影響它。

## 工作方式
- 只改需要改的地方，不要整份檔案重寫。
- commit 訊息用繁體中文，寫清楚改了什麼、為什麼改。
- 使用者用 iPhone 操作、偏好白話直接的說明。
