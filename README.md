# Personal Media Archiver (PMA)

**Development, Issues & Pull Requests:**  
https://github.com/aa22396584/personal-media-archiver

**Mirrors:**  
[GitLab](https://gitlab.com/aa22396584/personal-media-archiver) ·
[Codeberg](https://codeberg.org/ImL1s/personal-media-archiver)


> **Why this GitHub home?** Public development moved here from [`ImL1s/personal-media-archiver`](https://github.com/ImL1s/personal-media-archiver) because that GitHub account is currently restricted (anonymous visitors get 404 on the profile and many assets). This is the same project. Please open Issues and Pull Requests here.

**Language**: [English](./README.en.md) · **繁體中文**

[![CI](https://github.com/aa22396584/personal-media-archiver/actions/workflows/ci.yml/badge.svg)](https://github.com/aa22396584/personal-media-archiver/actions/workflows/ci.yml)
[![Release APK](https://github.com/aa22396584/personal-media-archiver/actions/workflows/release.yml/badge.svg)](https://github.com/aa22396584/personal-media-archiver/actions/workflows/release.yml)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Platform: Android 7+](https://img.shields.io/badge/Platform-Android%207%2B-3DDC84?logo=android&logoColor=white)](#平台支援)

> 個人媒體保存工具 — Android、無後端、本機優先、開源精神。

PMA 是一個 **個人媒體歸檔（archiver）**，不是通用下載器。它的核心理念是：
讓使用者把**自己合法擁有或公開可存取的媒體**整理、保存到自己手機上，由使用者
自行掌握與管理。所有資料只在裝置本機，沒有後端、沒有帳號、沒有遙測。

## 功能

- 🎯 **政策分類器**：URL 貼上後自動判定為 ALLOW / WARN / BLOCK
- ⛔ **DRM/付費牆內容直接阻擋**（Netflix、Disney+、Spotify、Apple Music 等）
- 📥 **兩條下載路徑**：
  - 直連媒體 URL：純 Dart `dio` 高速下載
  - 公開社交平台（YouTube、Twitter/X、Threads、TikTok、Instagram、Bilibili、
    Vimeo、SoundCloud、Twitch 等 1000+ 站）：透過 [youtubedl-android](https://github.com/junkfood02/youtubedl-android) 內建的 yt-dlp 擷取
- 📚 **本機媒體庫**：JSON-based 索引，支援開啟外部播放器
- 🔗 **Android share intent**：其他 app 分享 URL/cookies 到 PMA 自動接收；URL 走政策三段制（ALLOW 自動下載 / WARN 預填首頁等用戶確認 / BLOCK 拒絕），cookies 自動匯入
- 🛡️ **預設零遙測**：沒有 analytics、沒有 crash reporting、沒有雲端同步
- 🎨 **Material 3 Expressive + Atkinson Hyperlegible**：無障礙、可讀性高

## 截圖

實機 Samsung Galaxy Note 9（Android 10, 1080×2220）：

| 首頁 / 新增 | 佇列 | 媒體庫 |
|:---:|:---:|:---:|
| <img src="docs/screenshots/01-home.png" width="240" alt="Home tab"> | <img src="docs/screenshots/02-queue.png" width="240" alt="Queue tab"> | <img src="docs/screenshots/03-library.png" width="240" alt="Library tab"> |
| **設定** | **政策 ALLOW（YouTube → 綠）** | **政策 BLOCK（Netflix → 紅）** |
| <img src="docs/screenshots/04-settings.png" width="240" alt="Settings tab"> | <img src="docs/screenshots/05-policy-allow-youtube.png" width="240" alt="Policy ALLOW for YouTube"> | <img src="docs/screenshots/06-policy-block-netflix.png" width="240" alt="Policy BLOCK for Netflix"> |

### 端到端真實流程

直連 mp4 下載 → 佇列 → 媒體庫，以及 share intent 三段政策（BLOCK / WARN / ALLOW）+ 開源授權頁，全部在 Note 9 真機跑通：

| 下載中（77%） | 已完成入媒體庫 | 開源授權頁 |
|:---:|:---:|:---:|
| <img src="docs/screenshots/e2e-03-queue-active.png" width="240" alt="Queue downloading 77%"> | <img src="docs/screenshots/e2e-05-library-with-file.png" width="240" alt="Library with completed file"> | <img src="docs/screenshots/e2e-10-license-page.png" width="240" alt="Open-source license page"> |
| **Share BLOCK（Hami → 紅）** | **Share WARN（unknown → 橘 + 預填）** | **Share ALLOW（YouTube → 綠 + 自動入佇列）** |
| <img src="docs/screenshots/e2e-06-share-block-hami.png" width="240" alt="Share intent BLOCK for Hami"> | <img src="docs/screenshots/e2e-07-share-warn-unknown.png" width="240" alt="Share intent WARN for unknown host"> | <img src="docs/screenshots/e2e-08-share-allow-youtube.png" width="240" alt="Share intent ALLOW for YouTube"> |

## 使用守則（重要）

```
本工具僅供保存使用者已獲授權存取之媒體內容。
不支援也不協助規避 DRM、付費牆、登入保護、驗證機制或其他來源網站的技術限制。
使用者應自行確認其操作符合所在地法律、著作權規範及來源網站／服務條款。
```

## 範圍邊界（明確不做）

- ❌ 不繞過 DRM / 付費牆 / 任何 anti-circumvention 機制
- ❌ 不集中化帳號或 cookies、不雲端同步
- ❌ 不上 Google Play 完整版（如需 Play 版會做 Lite — 移除 yt-dlp 擷取）
- ❌ 不做 iOS 版（Apple 5.2.3 直接禁止第三方媒體下載）
- ❌ 不做遠端熱更新功能碼
- ❌ 不提供帳號池、不代理請求

## 平台支援

- ✅ Android 7.0+ (API 24+)
- ABI：arm64-v8a、armeabi-v7a、x86_64
- 建議透過 **GitHub Release** 或 **F-Droid**（未來）取得 APK

## 編譯

```bash
# 1. 安裝 fvm 並設定 Flutter stable
fvm use stable --force

# 2. 取得依賴
fvm flutter pub get

# 3. Debug APK
fvm flutter build apk --debug

# 4. Release APK（請自行設定 signing config）
fvm flutter build apk --release
```

首次 build 會下載 youtubedl-android 的 maven 套件與內含 Python runtime，APK 大
小約 80-150 MB（這是讓社交平台真的能下載的代價）。

## 架構

```
lib/
├── main.dart
├── app.dart                       # MaterialApp + Material 3 主題
├── theme/app_theme.dart           # Teal + Atkinson Hyperlegible
├── core/
│   ├── models/                    # DownloadTask / MediaItem / PolicyDecision
│   ├── policy/                    # SourcePolicy 來源分類器
│   ├── download/                  # 純 Dart dio 下載引擎
│   ├── library/                   # 本機媒體索引（JSON）
│   └── extractors/                # YtDlpBridge — platform channel wrapper
├── state/                         # Riverpod providers
└── ui/
    ├── pages/                     # 首頁 / 佇列 / 媒體庫 / 設定
    └── widgets/                   # PolicyBanner / TaskCard / MediaCard / EmptyState
android/
└── app/src/main/kotlin/.../MainActivity.kt   # YoutubeDL.init + MethodChannel/EventChannel
```

## 第三方相依

- [youtubedl-android](https://github.com/junkfood02/youtubedl-android) (GPL-3.0)
  — 提供 yt-dlp 引擎與 ffmpeg 整合
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) (Unlicense)
- [Flutter](https://flutter.dev) / [Dart](https://dart.dev) / [Riverpod](https://riverpod.dev)
- Google Fonts — Atkinson Hyperlegible

> ⚠️ 注意：youtubedl-android 為 **GPL-3.0**，整個應用程式因此實際上以 GPL-3.0
> 散布；本專案核心碼採 Apache-2.0，但發佈成品需符合 GPL-3.0。

## 隱私聲明

- Cookies / token 只保存於本機，可隨時清除
- 預設不啟用任何遙測
- 預設不向任何第三方分析服務傳送資料
- 媒體檔案儲存於 app 私有外部目錄，解除安裝後一併刪除

## 已驗證的端到端流程（真機 e2e）

在 Samsung Galaxy Note 9（Android 10、SM-N960F）上完整跑通：

| 測試項 | 結果 |
|---|---|
| App 啟動、Material 3 Expressive 主題、繁體中文 UI | ✅ |
| 政策分類器：直連 mp4 ALLOW（綠）| ✅ |
| 政策分類器：Netflix BLOCK（紅）| ✅ |
| 政策分類器：YouTube/Vimeo/Streamable ALLOW（綠，標 yt-dlp）| ✅ |
| 直連 HTTP 下載：Big Buck Bunny 1MB mp4，SHA-256 與原檔位元完全一致 | ✅ |
| yt-dlp 自動更新：bundled 2025.11.12 → GitHub STABLE 2026.03.17 | ✅ |
| yt-dlp 下載：Streamable `me irl.mp4`（2.9 MB，valid ISO Media MP4）| ✅ |
| 錯誤處理：YouTube bot challenge、Vimeo login required 等錯誤完整顯示在 UI | ✅ |
| 佇列管理：進行中、完成、失敗、清除已完成 | ✅ |
| 媒體庫：列出下載完成檔案、開啟外部播放器、刪除 | ✅ |
| 設定：政策強度切換、並行下載數調整、yt-dlp 版本顯示、手動觸發更新 | ✅ |
| YouTube cookies 匯入 + 真機 wiring (logcat 確認 yt-dlp 收到 `--cookies` flag) | ✅ |
| Share intent: ALLOW URL → 自動 enqueue + 綠色 SnackBar + 切佇列 tab | ✅ |
| Share intent: WARN URL → 預填首頁 textfield + 橘色 SnackBar 要 user 確認 | ✅ |
| Share intent: BLOCK URL → 紅色 SnackBar 拒絕（含原因） | ✅ |
| Share intent: Firefox cookies → 自動 importFromContent + 綠 SnackBar | ✅ |
| 失敗 task 顯示「匯入 cookies」按鈕（點擊跳設定 tab） | ⏳ mobile-mcp UI 驗 |
| 媒體庫 sort（時間/檔名/大小）+ search filter | ✅ |
| YouTube URL + 未匯入 cookies hint 卡片 | ⏳ mobile-mcp UI 驗 |
| Cookies 過期偵測（>5 個月警告） | ⏳ mobile-mcp UI 驗 |
| 開源授權頁（設定 → 開源授權 → showLicensePage） | ✅ |
| 台灣 OTT BLOCK（Hami/Catchplay/Viu/Vidol/MOD 等） | ✅ Hami 驗 |

### 已知 yt-dlp 限制（非整合問題）
- **YouTube**：2024 之後 YouTube 加強反 bot；許多影片需要 cookies 才能下載。
  PMA 已支援「手動匯入 cookies.txt」緩解（見下方）。
- **Vimeo**：少數 ID 需要登入。yt-dlp 已顯示具體錯誤。
- **TikTok**：可能依 IP 區域被擋（IP-block）。

## 進階：YouTube cookies 取得

PMA 提供 **兩條路徑** — 純手機（推薦）/ PC + adb 配合（給有桌機的 user）。

### 🥇 方案 B — 純手機智慧匯入（推薦）

設定 → 進階 → YouTube cookies → 「+」→ 選「智慧匯入（用 Firefox）」

PMA 會引導 3 步驟 onboarding：
1. **裝 Firefox + cookies extension**（一次性）
   - Play Store 裝 Firefox for Android（[`org.mozilla.firefox`](https://play.google.com/store/apps/details?id=org.mozilla.firefox)）
   - 在 Firefox 內打開 `addons.mozilla.org` 搜「cookies.txt」裝 extension（如 [cookies.txt](https://addons.mozilla.org/firefox/addon/cookies-txt/)）
2. **登入 youtube.com**：PMA 自動跳 Firefox 打開 youtube → 登入你的 Google 帳號
3. **匯出 + 分享回 PMA**：點 extension export → Android 分享選單 → 選 PMA → 自動匯入完成 SnackBar

之後 cookies 過期重做 step 2-3 即可（不必再裝 extension）。

**為什麼是 Firefox？** Chrome 自 2024/07 月加 app-bound encryption，外部 app 無法讀 cookies；Chrome 行動版也不支援 extension。Firefox 是 2026 年唯一可行的選擇。

### 🥈 方案 A — 桌面 export + adb push

YouTube 因 Google 反爬機制（含 bot challenge / PoToken）越來越擋無 cookies 的下載
請求。PMA 採取「手動匯入 cookies.txt」方案，最尊重用戶隱私 — 不在 app 內登入、
不要求授權；cookies 完全儲存在 app 私有目錄，解除安裝即刪。

### 步驟

1. **桌面瀏覽器安裝 cookies 匯出擴充**
   - Chrome：[Get cookies.txt LOCALLY](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)
   - Firefox：[cookies.txt](https://addons.mozilla.org/firefox/addon/cookies-txt/)

2. **訪 youtube.com 並登入**（建議使用 throwaway Google 帳號避免主帳號被盯）。
3. **點擴充 → Export → 下載 cookies.txt**。
4. **把 cookies.txt 傳到 Android**
   - USB：`adb push cookies.txt /sdcard/Download/`
   - 或用 Google Drive / 即時通訊 App 分享到手機
5. **在 PMA app**：設定 → 進階 → YouTube cookies → 匯入 → 選 cookies.txt。
6. **重新嘗試 YouTube 下載**。若仍失敗，cookies 可能已過期 — 重做步驟 2-5。

### 注意事項

- Cookies 約 6 個月過期，建議定期重新匯出
- 不要分享 cookies 給他人（含 session token，可被冒用）
- 設定 → 進階 → YouTube cookies → 刪除：可隨時清除
- 失敗訊息含 `[NEEDS_COOKIES]` 表示此來源需要 cookies；UI 會引導至設定頁

## 發佈 / Release

PMA 的 APK 由 GitHub Actions 自動 build + 發佈到 [Releases](https://github.com/aa22396584/personal-media-archiver/releases) 頁面。

**觸發新 release**（maintainer）：

```bash
# 1. 確認 main 分支 analyze + test 全綠
fvm flutter analyze && fvm flutter test

# 2. bump pubspec.yaml 的 version 欄位（例：0.1.0+1 → 0.2.0+2）
#    semver: MAJOR.MINOR.PATCH+BUILD_NUMBER

# 3. commit 變更
git add pubspec.yaml CHANGELOG.md
git commit -m "chore: bump to v0.2.0"
git push

# 4. 打 tag 並 push — 觸發 release.yml
git tag v0.2.0
git push origin v0.2.0
```

GitHub Actions 會：
1. 重跑 analyze + test
2. 跑 `flutter build apk --release --split-per-abi` + universal APK
3. 把 4 個 APK（arm64-v8a / armeabi-v7a / x86_64 / universal）上傳到對應 tag 的 GitHub Release
4. 自動產生 release notes（從上次 tag 以來的 commit）

**簽名說明**：CI 出來的 APK 用 Flutter debug keystore 簽，可 sideload 但無法上 Google Play。要做 production signing 需另存 keystore 至 GitHub Secrets 並改 `android/app/build.gradle.kts` signing config。

---

## 支持

如果這個專案幫你省了點時間，可以[請我喝杯咖啡](https://buymeacoffee.com/iml1s)。

## License

Apache-2.0；發佈成品因含 youtubedl-android 必須以 GPL-3.0 散布。
詳見 `LICENSE`。
