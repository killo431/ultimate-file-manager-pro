# Technology Stack

## Core Sections (Required)

### 1) Runtime Summary

This repository contains two independent products:

**A. Main Android app (repo root)**

| Area | Value | Evidence |
|------|-------|----------|
| Primary language | Kotlin | `app/build.gradle.kts`, `gradle/libs.versions.toml` (`kotlin = "2.2.10"`) |
| Runtime + version | Android, minSdk 26 | `app/build.gradle.kts:111` (`minSdk = 26`), `compileSdk`/`targetSdk` set via version catalog (`app/build.gradle.kts:103,112`) |
| Package manager | Gradle (Kotlin DSL) | `build.gradle.kts`, `settings.gradle.kts`, `gradlew` |
| Module/build system | Gradle multi-module (single `:app` module) | `settings.gradle.kts` (`include(":app")`) |

**B. UFM-Windows companion app (`UFM-Windows/`)**

| Area | Value | Evidence |
|------|-------|----------|
| Primary language | TypeScript (frontend) + Rust (backend) | `UFM-Windows/package.json`, `UFM-Windows/src-tauri/Cargo.toml` |
| Runtime + version | Tauri 2 desktop shell (React 19 frontend, Rust `edition = "2021"`) | `UFM-Windows/src-tauri/Cargo.toml` (`tauri = "2"`, `edition = "2021"`) |
| Package manager | npm (Node) + Cargo (Rust) | `UFM-Windows/package-lock.json`, `UFM-Windows/src-tauri/Cargo.lock` |
| Module/build system | Vite (frontend bundler) + `cargo`/`tauri build` | `UFM-Windows/vite.config.ts`, `UFM-Windows/src-tauri/Cargo.toml` |

### 2) Production Frameworks and Dependencies

Android app (only high-impact production deps; full list in `gradle/libs.versions.toml`):

| Dependency | Version | Role in system | Evidence |
|------------|---------|-----------------|----------|
| AndroidX core/appcompat/material | 1.10.1 / 1.6.1 / 1.14.0 | Base UI toolkit | `gradle/libs.versions.toml` |
| Kotlin Coroutines | 1.7.3 | Async work | `gradle/libs.versions.toml` (`coroutines`) |
| Room | 2.6.1 | Local database/persistence | `gradle/libs.versions.toml` (`room`) |
| AndroidX Media3 (ExoPlayer) | 1.10.1 | Audio/video playback (HLS/DASH/RTSP) | `gradle/libs.versions.toml` (`media3`) |
| rclone (bundled AAR) | n/a (vendored) | Cloud storage protocol engine (S3, WebDAV, SMB, etc.) | `app/libs/rclone.aar` |
| FFmpeg (native, via JNI) | vendored headers/`.so` | Native media decode/thumbnailing | `app/src/main/cpp/ffmpeg_include/*`, `app/src/main/jniLibs/*` |
| Firebase Analytics | 23.0.0 | Analytics | `gradle/libs.versions.toml` (`firebaseAnalytics`) |
| Google Play Billing (via `billing` package) | n/a shown in catalog directly | In-app purchases/donations | `app/src/google/java/.../billing/BillingManager.kt` |
| MSAL (Microsoft Identity) | 8.2.3 | OneDrive/Microsoft auth | `gradle/libs.versions.toml` (`msal`) |
| OkHttp | 5.4.0 | HTTP client | `gradle/libs.versions.toml` (`okhttp`) |
| Shizuku / libsu | 13.1.5 / 6.0.0 | Root/elevated-privilege file operations | `gradle/libs.versions.toml` (`shizuku`, `libsu`) |
| zip4j / commons-compress / xz / zstd-jni / junrar | various | Archive format support (ZIP/7Z/RAR/TAR/ZSTD) | `gradle/libs.versions.toml` |
| Coil3 (+avif/jxl/gif/svg/apng codecs) | 3.4.0 | Image loading incl. AVIF/JPEG XL/APNG | `gradle/libs.versions.toml` |
| Google OSS Licenses plugin | 0.10.6 | Open-source license attribution screen | `gradle/libs.versions.toml` (`ossLicensesPlugin`) |

UFM-Windows (Tauri companion desktop app):

| Dependency | Version | Role in system | Evidence |
|------------|---------|-----------------|----------|
| react / react-dom | ^19.1.0 | UI framework | `UFM-Windows/package.json` |
| @tauri-apps/api, @tauri-apps/plugin-opener | ^2 | Frontend↔Rust bridge | `UFM-Windows/package.json` |
| lucide-react | ^1.24.0 | Icon set | `UFM-Windows/package.json` |
| tauri, tauri-plugin-opener (Rust) | 2 | Native shell/runtime | `UFM-Windows/src-tauri/Cargo.toml` |
| reqwest, tokio | 0.12 / 1 | HTTP client + async runtime (talks to Android device's local server) | `UFM-Windows/src-tauri/Cargo.toml` |
| serde, serde_json | 1 | Serialization for Tauri commands | `UFM-Windows/src-tauri/Cargo.toml` |
| get_if_addrs | 0.5 | Local network interface discovery (device pairing) | `UFM-Windows/src-tauri/Cargo.toml` |

### 3) Development Toolchain

| Tool | Purpose | Evidence |
|------|---------|----------|
| JUnit 4 / AndroidX JUnit / Espresso | Unit + instrumented Android testing | `gradle/libs.versions.toml` (`junit`, `junitVersion`, `espressoCore`); `app/src/test`, `app/src/androidTest` |
| KSP (Kotlin Symbol Processing) | Annotation processing (Room) | `gradle/libs.versions.toml` (`ksp`), `app/build.gradle.kts` plugin block |
| app.cash.licensee | License compliance checking | `build.gradle.kts:154` |
| TypeScript compiler (`tsc`) | Type-checking UFM-Windows before build | `UFM-Windows/package.json` (`"build": "tsc && vite build"`) |
| Vite | Dev server/bundler for UFM-Windows | `UFM-Windows/vite.config.ts` |
| Cargo | Rust build/test | `UFM-Windows/src-tauri/Cargo.toml` |

No `.eslintrc`/`.prettierrc`/ktlint/detekt config files were found in the repository — `[TODO]` confirm whether linting is enforced via IDE settings only (see `.idea/codeStyles/`).

### 4) Key Commands

```bash
# Android app
./gradlew assembleDebug
./gradlew test              # JVM unit tests
./gradlew connectedAndroidTest  # instrumented tests (requires device/emulator)

# UFM-Windows
cd UFM-Windows
npm install
npm run dev      # vite dev server
npm run build    # tsc && vite build
npm run tauri build
```

### 5) Environment and Config

- Config sources: `app/build.gradle.kts` reads `local.properties` and environment variables for OAuth client IDs/secrets (Google Drive, Dropbox) — `app/build.gradle.kts:64-90`.
- Required env vars (Android, optional — fall back to placeholder strings if unset): `GOOGLE_DRIVE_TV_CLIENT_ID`, `GOOGLE_DRIVE_MOBILE_DEBUG_CLIENT_ID`, `GOOGLE_DRIVE_MOBILE_RELEASE_CLIENT_ID`, `GOOGLE_DRIVE_TV_CLIENT_SECRET`/`GOOGLE_DRIVE_CLIENT_SECRET`, `DROPBOX_APP_KEY`, `DROPBOX_APP_SECRET` — `app/build.gradle.kts:64-90`.
- No `.env.example`/`.env.template` found for UFM-Windows; `[TODO]` confirm whether it requires any runtime secrets.
- Deployment/runtime constraints: multiple Android product flavors — `amazon`, `google`, `foss`, `nonfoss`, `mobile`, `tv` (source sets under `app/src/*`) — build variants differ in billing/update mechanisms.

### 6) Evidence

- `app/build.gradle.kts`
- `gradle/libs.versions.toml`
- `settings.gradle.kts`
- `UFM-Windows/package.json`
- `UFM-Windows/src-tauri/Cargo.toml`
- `UFM-Windows/tsconfig.json`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
