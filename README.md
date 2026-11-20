# 錢錢記帳 — Android 可安裝 PWA

這個專案以使用者原版 `ledger_v4-1.html` 為基礎，**不改動其核心記帳邏輯和既有 localStorage key**，只補上安裝資訊、圖示和離線使用支援。

## 檔案結構

```text
index.html
manifest.webmanifest
sw.js
icons/
  icon-192.png
  icon-512.png
```

## GitHub Pages 部署（手機亦可完成）

1. 前往 https://github.com/new，建立新的 **Public** repository，例如 `money-ledger`。**不要與原本 `podcast-saver` 放在同一個資料夾**。
2. 解壓縮 ZIP，將 `index.html`、`manifest.webmanifest`、`sw.js` 上傳 repository 根目錄；`icon-192.png`、`icon-512.png` 放進 `icons` 資料夾。GitHub 行動網頁可先新增一個 `icons/placeholder.txt` 建立資料夾，再進入該資料夾使用 `Add file → Upload files` 上傳 PNG；最後可刪除 placeholder。
3. `Commit changes`。
4. Repository → `Settings → Pages → Build and deployment`，Source 選 `Deploy from a branch`，Branch `main`，Folder `/(root)`，按 Save。
5. 網址通常是 `https://YOUR_USERNAME.github.io/money-ledger/`，以 GitHub 顯示的實際 Pages 網址為準。
6. 用 **Android Chrome** 開啟網址 → 右上角 `⋮` → **安裝應用程式** → 安裝。完成後主畫面可看到「錢錢」的金幣圖示。

> 安裝不需要 Android Studio、不需要 API key，也不需要 Google Play 商店。這是 PWA，不是原生 APK。

## 日後使用 AI 更新版本（核心做法）

### 1. 先備份真實記帳資料

在「錢錢記帳」App 點底部 **匯出文字檔**，把 TXT 備份到安全位置。GitHub 上的 HTML 程式碼不含你的交易紀錄。

### 2. 讓 AI 修改專案

把這個專案 ZIP 或新版 `index.html` 上傳到 ChatGPT、Claude、Cursor 等 AI 工具，附上需求（例如調整按鍵大小、增加統計圖表）。

可以直接複製下面這段指示：

> 請修改我上傳的「錢錢記帳」PWA 專案。保持既有記帳紀錄、付款方式、貨幣、編輯、匯出、匯入功能正常。不得更動既有 localStorage keys（ledger_history_v1、ledger_paytypes_v2、ledger_paylayout_v1、ledger_currencies_v1）或資料結構，除非提供向後相容遷移機制。維持 manifest.webmanifest 和 icons 路徑，更新 sw.js 中的 CACHE_NAME 版本號（例如 ledger-money-v2）。請提供改好的完整檔案和更新說明，不要清除使用者資料。

### 3. 更新 GitHub 上的程式碼

- 在 repository 的 `index.html` 點入，編輯或重新上傳覆蓋舊版；如 AI 有變更 `sw.js`、manifest 或 icons，也一起更新。
- **不要**把檔案放進另一層資料夾，也不要換 GitHub Pages 網址；沿用同一 repository 和 Pages URL。
- 點 `Commit changes`，稍後重新開啟 App。程式內的 service worker 採網路優先，可在有網路時下載最新 HTML，離線則使用快取；更新後若看不到變更，可關掉 App 再開一次。

### 4. 核對資料是否還在

- 測試原有記帳紀錄是否仍在。
- 測試新增、編輯、付款方式選擇、匯出。
- 如果資料不見，**不要立即點清空**，先確認是否開了不同網址、不同瀏覽器或不同安裝實例；可使用備份 TXT 匯入。

## 注意事項

- `localStorage` 依 **網站來源（origin）** 隔離。從本機 file:// HTML、Brave、Chrome、或不同 GitHub Pages 網址切換，不會自動搬移既有紀錄。
- 從 Brave 的本機 HTML 移到 Chrome 安裝的 PWA，請在舊環境先匯出 TXT，再到 PWA 匯入。原本的匯入功能預設將紀錄**附加**於目前資料，重複匯入可能造成重複交易。
- GitHub Pages 的 Public repository 代表網站程式碼是公開的；交易資料不會自動上傳至 GitHub。公開上傳前請確認 HTML 沒嵌入任何私人紀錄。
- 解除安裝或清除網站資料可能導致資料遺失，建議定期匯出備份。
- 如果修改記帳資料的儲存結構，務必在修改前備份，並讓 AI 提供向後相容的資料遷移。
- GitHub Pages 是靜態網站服務，無伺服器資料庫；離線快取更新受瀏覽器控制。
