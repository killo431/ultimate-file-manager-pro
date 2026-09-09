# Architecture

## Core Sections (Required)

### 1) Architectural Style

- Primary style: Feature-organized, Activity-centric Android app (not MVVM/Clean Architecture — no `viewmodel`/`domain`/`data` layer directories exist).
- Why this classification: Top-level Kotlin packages are named by feature (`storage`, `network`, `viewer`, `billing`, `settings`, `archive`, `sync`) rather than by layer, and large `*Activity.kt` files (e.g. `NetworkBrowserActivity.kt` at 7,803 lines, `FileBrowserActivity.kt` at 7,183 lines) contain substantial logic directly, rather than delegating to a ViewModel layer — `app/src/main/java/za/kilowatch/ultimatefilemanager/network/NetworkBrowserActivity.kt`, `app/src/main/java/za/kilowatch/ultimatefilemanager/storage/FileBrowserActivity.kt`.
- Primary constraints: (1) must support multiple product flavors (Amazon/Google/FOSS/non-FOSS) with different billing/update backends via flavor-specific source sets; (2) must support both phone/tablet and Android TV (Leanback) UI via `mobile`/`tv` source sets; (3) bundles a native FFmpeg build and a vendored rclone AAR, constraining build/dependency management (custom Gradle artifact transform to strip a conflicting class — `app/build.gradle.kts:1-46`).

### 2) System Flow

```text
[Launcher Activity, MAIN/LAUNCHER intent-filter] -> [Storage/Network browsing Activities+Fragments] -> [Feature managers: FileServer, PairingServer, Archive/Media handlers] -> [Native/vendored engines: FFmpeg (JNI), rclone.aar, Room DB] -> [UI update / file operation result]
```

Trace evidence: `app/src/main/AndroidManifest.xml:101` (LAUNCHER intent-filter) → `app/src/main/java/za/kilowatch/ultimatefilemanager/storage/FileBrowserActivity.kt` / `StorageBrowserActivity.kt` (local browsing) or `network/NetworkBrowserActivity.kt` (LAN/cloud browsing) → `remote/FileServer.kt` (exposes local files to the paired desktop app) → native layer (`app/src/main/cpp/ffmpeg_include/*`, `app/libs/rclone.aar`) for media decode / cloud protocol operations.

### 3) Layer/Module Responsibilities

| Layer or module | Owns | Must not own | Evidence |
|-----------------|------|--------------|----------|
| `storage/` | Local filesystem browsing, batch rename, search, twin-pane view | Network protocol code | `app/src/main/java/za/kilowatch/ultimatefilemanager/storage/*` |
| `network/` | LAN discovery, device pairing (`PairingServer.kt`), cloud browsing UI | Local-only file operations | `app/src/main/java/za/kilowatch/ultimatefilemanager/network/*` |
| `remote/` | Local HTTP file server (`FileServer.kt`, 4,292 lines) so the UFM-Windows desktop companion can browse/transfer files over the LAN | UI rendering | `app/src/main/java/za/kilowatch/ultimatefilemanager/remote/FileServer.kt` |
| `billing/` (flavor-specific) | Monetization: Google Play Billing (`google/.../BillingManager.kt`) vs donation links (`foss/.../SupporterLoyaltyActivity.kt`) | Core file-management logic | `app/src/google/java/.../billing/BillingManager.kt`, `app/src/foss/java/.../billing/SupporterLoyaltyActivity.kt` |
| `viewer/` | Media/text/archive viewing (`UFMPlayerActivity.kt` using Media3 ExoPlayer, syntax highlighting registry) | File-system mutation logic | `app/src/main/java/za/kilowatch/ultimatefilemanager/viewer/*` |
| `UFM-Windows/src-tauri/src/commands.rs` | Tauri `#[tauri::command]` handlers bridging the React frontend to local filesystem + HTTP calls against the Android device's `remote/FileServer.kt` | Frontend rendering | `UFM-Windows/src-tauri/src/lib.rs:5-31` |

### 4) Reused Patterns

| Pattern | Where found | Why it exists |
|---------|-------------|----------------|
| Flavor-based strategy pattern (parallel implementations per Gradle product flavor) | `billing/` implemented separately under `app/src/google/`, `app/src/foss/`, `app/src/amazon/` | Different distribution channels require different billing/update mechanisms |
| Command/bridge pattern (Tauri `invoke_handler!` macro registering RPC-like commands) | `UFM-Windows/src-tauri/src/lib.rs:16-31` | Standard Tauri approach for exposing Rust functions to the JS frontend |
| Vendored native engine wrapping (JNI + prebuilt AAR) | `app/src/main/cpp/ffmpeg_include/*`, `app/libs/rclone.aar` | Avoids reimplementing complex media/cloud-protocol engines in Kotlin |

### 5) Known Architectural Risks

- Several Activity classes exceed 2,000-7,800 lines (`NetworkBrowserActivity.kt`, `FileBrowserActivity.kt`, `StorageBrowserActivity.kt`, `CrashReportManager.kt`, `remote/FileServer.kt`), indicating logic is not delegated to smaller, testable units — risk of high coupling and difficult maintenance (see CONCERNS.md).
- No ViewModel/architecture-component layer visible between Activities and data sources; state management approach is `[TODO]` (needs deeper reading of individual Activities to confirm).

### 6) Evidence

- `app/src/main/AndroidManifest.xml`
- `app/src/main/java/za/kilowatch/ultimatefilemanager/network/NetworkBrowserActivity.kt`
- `app/src/main/java/za/kilowatch/ultimatefilemanager/remote/FileServer.kt`
- `UFM-Windows/src-tauri/src/lib.rs`
- `app/build.gradle.kts`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
