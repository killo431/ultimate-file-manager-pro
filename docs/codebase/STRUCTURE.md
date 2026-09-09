# Codebase Structure

## Core Sections (Required)

### 1) Top-Level Map

| Path | Purpose | Evidence |
|------|---------|----------|
| `app/` | Main Android application module (Kotlin, single Gradle module `:app`) | `settings.gradle.kts` (`include(":app")`) |
| `app/src/main/` | Shared source set: Kotlin code, native C/C++ (FFmpeg), resources, manifest | `app/src/main/AndroidManifest.xml`, `app/src/main/cpp/`, `app/src/main/java/` |
| `app/src/amazon/`, `app/src/google/`, `app/src/foss/`, `app/src/nonfoss/` | Product-flavor-specific source sets (differ in billing/update/licensing backend) | Corresponding `AndroidManifest.xml` + `java/` per folder |
| `app/src/mobile/`, `app/src/tv/` | Form-factor-specific source sets (phone/tablet vs Android TV, e.g. `LEANBACK_LAUNCHER`) | `app/src/tv/AndroidManifest.xml:36` |
| `app/src/test/`, `app/src/androidTest/` | JVM unit tests and instrumented (on-device) tests | see Testing section below |
| `app/libs/rclone.aar` | Vendored prebuilt Android library implementing rclone cloud-storage protocols | `app/libs/rclone.aar` |
| `UFM-Windows/` | Separate companion desktop app (Tauri) for managing the Android device from a PC | `UFM-Windows/package.json`, `UFM-Windows/src-tauri/` |
| `docs/codebase/` | Generated codebase-knowledge documentation (this directory) | — |
| `.github/skills/` | Bundled Copilot skill(s), including `acquire-codebase-knowledge` | `.github/skills/acquire-codebase-knowledge/SKILL.md` |
| `gradle/`, `gradlew`, `build.gradle.kts`, `settings.gradle.kts` | Gradle wrapper and root build configuration | — |
| `size.md` | Explains the app's ~180MB installed size (multimedia engine, cloud engine, etc.) | `size.md` |

### 2) Entry Points

- Main Android runtime entry: `Activity` with `MAIN`/`LAUNCHER` intent-filter in `app/src/main/AndroidManifest.xml:101` (`category android:name="android.intent.category.LAUNCHER"`).
- TV-specific entry: `app/src/tv/AndroidManifest.xml:36` uses `LEANBACK_LAUNCHER` category for Android TV home screen.
- `[TODO]` exact Activity class name for the launcher entry was not confirmed from the manifest excerpt read; verify by inspecting the `<activity>` element wrapping the intent-filter at `app/src/main/AndroidManifest.xml:~95-105`.
- UFM-Windows frontend entry: `UFM-Windows/src/main.tsx` (Vite/React root, referenced by `UFM-Windows/index.html`).
- UFM-Windows native/backend entry: `UFM-Windows/src-tauri/src/main.rs` calls into `UFM-Windows/src-tauri/src/lib.rs::run()`, which registers all Tauri commands (`UFM-Windows/src-tauri/src/lib.rs:16-31`).

### 3) Module Boundaries

| Boundary | What belongs here | What must not be here |
|----------|--------------------|------------------------|
| `app/src/main/java/za/kilowatch/ultimatefilemanager/storage/` | Local file browsing UI/logic (e.g. `FileBrowserActivity.kt`, `StorageBrowserActivity.kt`) | Network/cloud-specific protocol code |
| `.../network/` | LAN/network browsing, device pairing, local server pairing (`NetworkBrowserActivity.kt`, `PairingServer.kt`) | Local-only storage logic |
| `.../remote/` | Remote file server implementation (`FileServer.kt`) exposed for PC/Web access | UI screens |
| `.../viewer/` | In-app file/media viewers (text, video via `UFMPlayerActivity.kt`, syntax highlighting) | Storage/browsing logic |
| `.../billing/` | Google Play Billing / donation flows, flavor-specific (`google/.../BillingManager.kt`, `foss/.../SupporterLoyaltyActivity.kt`) | Core file-management logic |
| `.../settings/` | App settings screens and preference managers | Business logic unrelated to configuration |
| `.../archive/` | Archive (zip/7z/rar/etc.) handling | Media/viewer logic |
| `.../sync/`, `.../recycle/`, `.../scanner/`, `.../indexing/`, `.../smartsort/`, `.../widget/`, `.../onboarding/`, `.../support/`, `.../update/`, `.../notepad/`, `.../receiver/` | Feature-specific packages (self-explanatory by name) | Cross-cutting utilities (see `util/`) |
| `.../util/` | Shared helpers (e.g. `DeviceUtils.kt`) | Feature-specific business logic |
| `UFM-Windows/src-tauri/src/commands.rs` | Tauri command handlers (file listing, upload/download, device discovery) invoked from the frontend | UI rendering |
| `UFM-Windows/src-tauri/src/discovery.rs` | Local network device discovery | Command/business logic |
| `UFM-Windows/src/components/` | React UI components (`FilePane.tsx`, `DeviceSelector.tsx`, `ProgressBar.tsx`, `SideloadZone.tsx`) | Rust/native logic |
| `UFM-Windows/src/hooks/` | React hooks incl. `useUfmApi.ts` (wraps Tauri `invoke` calls) and `translations.ts` | Component rendering logic |

### 4) Naming and Organization Rules

- Android package root: `za.kilowatch.ultimatefilemanager`, organized by feature (`storage`, `network`, `viewer`, `billing`, etc.) rather than by architectural layer — `app/src/main/java/za/kilowatch/ultimatefilemanager/*`.
- Kotlin file naming: PascalCase matching the primary class, suffixed by role (`*Activity.kt`, `*Fragment.kt`, `*Manager.kt`, `*Test.kt`) — e.g. `StorageBrowserActivity.kt`, `SettingsBackupManager.kt`, `NaturalSortTest.kt`.
- Product flavors (`amazon`/`google`/`foss`/`nonfoss`) and form factors (`mobile`/`tv`) are modeled as parallel Gradle source sets under `app/src/<flavor>/java/...`, mirroring the same package structure as `main`.
- UFM-Windows: React components in PascalCase `.tsx` files under `src/components/`; hooks in camelCase `use*.ts` files under `src/hooks/` — `UFM-Windows/src/hooks/useUfmApi.ts`.
- No TypeScript path aliases configured (`UFM-Windows/tsconfig.json` has no `paths` key) — imports are relative.

### 5) Evidence

- `settings.gradle.kts`
- `app/src/main/AndroidManifest.xml`
- `app/src/tv/AndroidManifest.xml`
- `UFM-Windows/src-tauri/src/lib.rs`
- `UFM-Windows/src/main.tsx`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
