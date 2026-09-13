# Personal Media Archiver (PMA)

**Language**: **English** · [繁體中文](./README.md)

[![CI](https://github.com/aa22396584/personal-media-archiver/actions/workflows/ci.yml/badge.svg)](https://github.com/aa22396584/personal-media-archiver/actions/workflows/ci.yml)
[![Release APK](https://github.com/aa22396584/personal-media-archiver/actions/workflows/release.yml/badge.svg)](https://github.com/aa22396584/personal-media-archiver/actions/workflows/release.yml)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Platform: Android 7+](https://img.shields.io/badge/Platform-Android%207%2B-3DDC84?logo=android&logoColor=white)](#platform-support)

> A personal media archiving tool — Android, no backend, local-first, open-source ethos.

PMA is a **personal media archiver**, not a general-purpose downloader. Its core
philosophy: let users save **media they legally own or that is publicly accessible**
to their own phone, fully under their own control. All data stays on-device — no
backend, no accounts, no telemetry.

## Features

- 🎯 **Policy classifier**: paste a URL, instantly classified as ALLOW / WARN / BLOCK
- ⛔ **DRM / paywall content blocked outright** (Netflix, Disney+, Spotify, Apple Music, etc.)
- 📥 **Two download paths**:
  - Direct media URLs: pure Dart `dio` high-speed download
  - Public social platforms (YouTube, Twitter/X, Threads, TikTok, Instagram,
    Bilibili, Vimeo, SoundCloud, Twitch, 1000+ sites): via
    [youtubedl-android](https://github.com/junkfood02/youtubedl-android)'s bundled yt-dlp
- 📚 **Local media library**: JSON-based index, opens external players
- 🔗 **Android share intent**: receive URLs / cookies shared from other apps;
  URLs run through the 3-tier policy (ALLOW auto-download / WARN prefill home
  for user confirmation / BLOCK reject), cookies auto-import
- 🛡️ **Zero telemetry by default**: no analytics, no crash reporting, no cloud sync
- 🎨 **Material 3 Expressive + Atkinson Hyperlegible**: accessibility-first, high readability

## Screenshots

Real device — Samsung Galaxy Note 9 (Android 10, 1080×2220):

| Home / New | Queue | Library |
|:---:|:---:|:---:|
| <img src="docs/screenshots/01-home.png" width="240" alt="Home tab"> | <img src="docs/screenshots/02-queue.png" width="240" alt="Queue tab"> | <img src="docs/screenshots/03-library.png" width="240" alt="Library tab"> |
| **Settings** | **Policy ALLOW (YouTube → green)** | **Policy BLOCK (Netflix → red)** |
| <img src="docs/screenshots/04-settings.png" width="240" alt="Settings tab"> | <img src="docs/screenshots/05-policy-allow-youtube.png" width="240" alt="Policy ALLOW for YouTube"> | <img src="docs/screenshots/06-policy-block-netflix.png" width="240" alt="Policy BLOCK for Netflix"> |

### End-to-end real flow

Direct mp4 download → queue → library, plus the three share-intent policy
verdicts (BLOCK / WARN / ALLOW) and the open-source license page — all
exercised on a real Note 9:

| Downloading (77%) | Completed in library | License page |
|:---:|:---:|:---:|
| <img src="docs/screenshots/e2e-03-queue-active.png" width="240" alt="Queue downloading 77%"> | <img src="docs/screenshots/e2e-05-library-with-file.png" width="240" alt="Library with completed file"> | <img src="docs/screenshots/e2e-10-license-page.png" width="240" alt="Open-source license page"> |
| **Share BLOCK (Hami → red)** | **Share WARN (unknown → orange + prefill)** | **Share ALLOW (YouTube → green + auto-enqueue)** |
| <img src="docs/screenshots/e2e-06-share-block-hami.png" width="240" alt="Share intent BLOCK for Hami"> | <img src="docs/screenshots/e2e-07-share-warn-unknown.png" width="240" alt="Share intent WARN for unknown host"> | <img src="docs/screenshots/e2e-08-share-allow-youtube.png" width="240" alt="Share intent ALLOW for YouTube"> |

## Acceptable Use (important)

```
This tool is intended solely for archiving media content that the user has
authorized access to. It does not support nor assist in circumventing DRM,
paywalls, login protection, anti-bot mechanisms, or any other technical
restrictions imposed by the source.
Users must independently verify that their operation complies with local
laws, copyright regulations, and the terms of service of the source site.
```

## Scope Boundaries (explicit non-goals)

- ❌ No DRM / paywall / anti-circumvention bypass
- ❌ No centralized accounts or cookies, no cloud sync
- ❌ Not on Google Play full version (a Lite variant without yt-dlp extraction may ship)
- ❌ No iOS version (Apple 5.2.3 prohibits third-party media downloaders)
- ❌ No remote hot-update of feature code
- ❌ No account pool, no request proxying

## Platform Support

- ✅ Android 7.0+ (API 24+)
- ABIs: arm64-v8a, armeabi-v7a, x86_64
- Recommended distribution: **GitHub Releases** or **F-Droid** (future)

## Build

```bash
# 1. Install fvm and set Flutter stable
fvm use stable --force

# 2. Pull dependencies
fvm flutter pub get

# 3. Debug APK
fvm flutter build apk --debug

# 4. Release APK (configure your own signing config)
fvm flutter build apk --release
```

First build downloads youtubedl-android's Maven package and bundled Python
runtime, so APK size will be ~80-150 MB (the cost of making social-platform
extraction actually work).

## Architecture

```
lib/
├── main.dart
├── app.dart                       # MaterialApp + Material 3 theme
├── theme/app_theme.dart           # Teal + Atkinson Hyperlegible
├── core/
│   ├── models/                    # DownloadTask / MediaItem / PolicyDecision
│   ├── policy/                    # SourcePolicy classifier
│   ├── download/                  # Pure Dart dio download engine
│   ├── library/                   # Local media index (JSON)
│   └── extractors/                # YtDlpBridge — platform channel wrapper
├── state/                         # Riverpod providers + share_url_handler
└── ui/
    ├── pages/                     # Home / Queue / Library / Settings
    └── widgets/                   # PolicyBanner / TaskCard / MediaCard / EmptyState
android/
└── app/src/main/kotlin/.../MainActivity.kt   # YoutubeDL.init + MethodChannel/EventChannel
```

## Third-party Dependencies

- [youtubedl-android](https://github.com/junkfood02/youtubedl-android) (GPL-3.0)
  — provides the yt-dlp engine and ffmpeg integration
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) (Unlicense)
- [Flutter](https://flutter.dev) / [Dart](https://dart.dev) / [Riverpod](https://riverpod.dev)
- Google Fonts — Atkinson Hyperlegible

> ⚠️ Note: youtubedl-android is **GPL-3.0**, so compiled APK distributions
> of this project inherit GPL-3.0. Source code authored in this repository
> remains under Apache-2.0; distributed binaries must comply with GPL-3.0.

## Privacy Statement

- Cookies / tokens are stored on-device only; clearable at any time
- No telemetry enabled by default
- No data transmitted to any third-party analytics service by default
- Downloaded media lives in the app's private external directory and is
  deleted when the app is uninstalled

## End-to-end Verification (real device)

Fully verified on a Samsung Galaxy Note 9 (Android 10, SM-N960F):

| Test | Result |
|---|---|
| App launch, Material 3 Expressive theme, 繁體中文 UI | ✅ |
| Policy classifier: direct mp4 → ALLOW (green) | ✅ |
| Policy classifier: Netflix → BLOCK (red) | ✅ |
| Policy classifier: YouTube / Vimeo / Streamable → ALLOW (green, marked yt-dlp) | ✅ |
| Direct HTTP download: Big Buck Bunny 1MB mp4, SHA-256 byte-identical to source | ✅ |
| yt-dlp auto-update: bundled 2025.11.12 → GitHub STABLE 2026.03.17 | ✅ |
| yt-dlp download: Streamable `me irl.mp4` (2.9 MB, valid ISO Media MP4) | ✅ |
| Error handling: YouTube bot challenge, Vimeo login required surfaced cleanly in UI | ✅ |
| Queue management: in-progress, complete, failed, clear-completed | ✅ |
| Library: list completed files, open external player, delete | ✅ |
| Settings: strictness toggle, parallel-downloads slider, yt-dlp version display, manual update | ✅ |
| YouTube cookies import + on-device wiring (logcat confirms yt-dlp gets `--cookies` flag) | ✅ |
| Share intent: ALLOW URL → auto-enqueue + green SnackBar + jump to queue tab | ✅ |
| Share intent: WARN URL → prefill home textfield + orange SnackBar for user consent | ✅ |
| Share intent: BLOCK URL → red SnackBar reject (with reason) | ✅ |
| Share intent: Firefox cookies → auto importFromContent + green SnackBar | ✅ |
| Failed task shows "Import cookies" button (jumps to settings tab) | ⏳ mobile-mcp UI verify |
| Library sort (time / filename / size) + search filter | ✅ |
| YouTube URL + missing-cookies hint card | ⏳ mobile-mcp UI verify |
| Cookies expiry detection (>5 months warning) | ⏳ mobile-mcp UI verify |
| Open-source license page (Settings → Open-source licenses → showLicensePage) | ✅ |
| Taiwan OTT BLOCK (Hami / Catchplay / Viu / Vidol / MOD etc.) | ✅ Hami verified |

### Known yt-dlp limitations (not integration issues)
- **YouTube**: post-2024, YouTube tightened anti-bot; many videos require
  cookies to download. PMA supports manual `cookies.txt` import to mitigate
  (see below).
- **Vimeo**: a small set of IDs require login. yt-dlp surfaces the exact error.
- **TikTok**: may be IP-blocked by region.

## Advanced: obtaining YouTube cookies

PMA offers **two paths** — phone-only (recommended) / desktop + adb (for power users).

### 🥇 Method B — Phone-only smart import (recommended)

Settings → Advanced → YouTube cookies → "+" → "Smart import (via Firefox)"

PMA walks you through a 3-step onboarding:
1. **Install Firefox + cookies extension** (one-time)
   - Install Firefox for Android from Play Store
     ([`org.mozilla.firefox`](https://play.google.com/store/apps/details?id=org.mozilla.firefox))
   - Open `addons.mozilla.org` in Firefox, search "cookies.txt", install one
     (e.g. [cookies.txt](https://addons.mozilla.org/firefox/addon/cookies-txt/))
2. **Log in to youtube.com**: PMA opens YouTube in Firefox → log in to your
   Google account
3. **Export + share back to PMA**: tap the extension's export → Android share
   sheet → select PMA → "Auto-imported" SnackBar appears

If cookies expire later, redo steps 2-3 (no need to reinstall the extension).

**Why Firefox?** Chrome added app-bound encryption in July 2024 making cookies
unreadable by external apps; Chrome mobile also lacks extension support.
Firefox is the only viable option in 2026.

### 🥈 Method A — Desktop export + adb push

YouTube's increasingly aggressive anti-bot (bot challenge / PoToken) blocks
cookie-less requests. PMA's "manual cookies.txt import" approach respects
user privacy — no in-app login, no auth flow; cookies live in the app's
private directory and are deleted on uninstall.

### Steps

1. **Install a cookies-export extension on your desktop browser**
   - Chrome: [Get cookies.txt LOCALLY](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc)
   - Firefox: [cookies.txt](https://addons.mozilla.org/firefox/addon/cookies-txt/)

2. **Visit youtube.com and log in** (recommend a throwaway Google account
   to avoid flagging your main).
3. **Click extension → Export → download cookies.txt**.
4. **Transfer cookies.txt to Android**
   - USB: `adb push cookies.txt /sdcard/Download/`
   - Or share via Google Drive / messaging apps
5. **In PMA**: Settings → Advanced → YouTube cookies → Import → select cookies.txt
6. **Retry the YouTube download.** If still failing, cookies may have expired
   — repeat steps 2-5.

### Notes

- Cookies typically expire in ~6 months; re-export periodically
- Don't share cookies with others (session tokens can be impersonated)
- Settings → Advanced → YouTube cookies → Delete: clears at any time
- Error messages containing `[NEEDS_COOKIES]` indicate the source requires
  cookies; the UI will guide you to the settings page

## Release

PMA APKs are automatically built and published to the
[Releases](https://github.com/aa22396584/personal-media-archiver/releases) page by GitHub Actions.

**Cutting a new release** (maintainer):

```bash
# 1. Ensure analyze + test pass on main
fvm flutter analyze && fvm flutter test

# 2. Bump the `version` field in pubspec.yaml (e.g. 0.1.0+1 → 0.2.0+2)
#    semver: MAJOR.MINOR.PATCH+BUILD_NUMBER

# 3. Commit
git add pubspec.yaml CHANGELOG.md
git commit -m "chore: bump to v0.2.0"
git push

# 4. Tag and push — triggers release.yml
git tag v0.2.0
git push origin v0.2.0
```

GitHub Actions will then:
1. Re-run analyze + tests
2. Run `flutter build apk --release --split-per-abi` + universal APK
3. Upload the four APKs (arm64-v8a / armeabi-v7a / x86_64 / universal) to a
   GitHub Release for the tag
4. Auto-generate release notes (from commits since the previous tag)

**Signing**: APKs produced by CI are signed with Flutter's debug keystore —
suitable for sideload but not for Google Play distribution. To enable
production signing, store a keystore in GitHub Secrets and update the
signing config in `android/app/build.gradle.kts`.

## License

Apache-2.0; compiled distributions must comply with GPL-3.0 because of the
youtubedl-android runtime dependency. See `LICENSE`.
