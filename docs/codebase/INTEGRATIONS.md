# External Integrations

## Core Sections (Required)

### 1) Integration Inventory

| System | Type (API/DB/Queue/etc) | Purpose | Auth model | Criticality | Evidence |
|--------|--------------------------|---------|------------|-------------|----------|
| rclone (vendored AAR) | Cloud storage protocol engine | Connects to MEGA, Filen, Drime, Koofr, Nextcloud, WebDAV, SMB, NFS, S3, etc. (per `size.md`) | Per-provider OAuth/credentials, `[TODO]` exact storage location | High | `app/libs/rclone.aar`, `size.md` |
| Google Drive API | Cloud storage API | Google Drive integration | OAuth 2.0 (client ID/secret via `local.properties`/env) | High | `app/build.gradle.kts:64-90` (`googleDriveTvClientId`, etc.) |
| Dropbox API | Cloud storage API | Dropbox integration | OAuth (app key/secret via `local.properties`/env) | High | `app/build.gradle.kts:88-90` (`dropboxAppKey`, `dropboxAppSecret`) |
| Microsoft Graph / OneDrive (via MSAL) | Cloud storage API | OneDrive integration | MSAL (Microsoft Identity Client) | Medium | `gradle/libs.versions.toml` (`msal = "8.2.3"`, `msal-android`) |
| Google Play Billing | Payments API | In-app purchases (Google flavor only) | Google Play account | Medium | `app/src/google/java/za/kilowatch/ultimatefilemanager/billing/BillingManager.kt` |
| Firebase Analytics | Analytics/telemetry | Usage analytics | Firebase project config (`google-services.json`, not read here) | Low/Medium | `gradle/libs.versions.toml` (`firebaseAnalytics`), `build.gradle.kts:151` (`google-gms-google-services` plugin) |
| Shizuku / libsu | Local privileged-execution API (not external network) | Root/elevated file operations on-device | Shizuku permission (`moe.shizuku.manager.permission.API_V23`) | High (for root features) | `app/src/main/AndroidManifest.xml` (Shizuku permissions), `gradle/libs.versions.toml` (`shizuku`, `libsu`) |
| Local HTTP/FTP/SFTP server (`remote/FileServer.kt`) | Self-hosted network API | Lets the UFM-Windows desktop companion (and any browser) browse/transfer device files over LAN | Custom (details `[TODO]`) | High | `app/src/main/java/za/kilowatch/ultimatefilemanager/remote/FileServer.kt` |
| UFM-Windows → Android device | REST-like HTTP calls (via `reqwest`) | Desktop app talks to the on-device `FileServer` to browse/upload/download files | `[TODO]` — token/pairing model not yet confirmed; device pairing exists (`save_paired_devices`/`load_paired_devices` commands) | High | `UFM-Windows/src-tauri/src/lib.rs:6-15`, `UFM-Windows/src-tauri/src/discovery.rs` |

### 2) Data Stores

| Store | Role | Access layer | Key risk | Evidence |
|-------|------|--------------|----------|----------|
| Room database | Local persistence (`[TODO]` exact entity/table purpose — indexing? favorites?) | AndroidX Room | `[TODO]` | `gradle/libs.versions.toml` (`room`, `androidx-room-runtime/compiler/ktx`) |
| Device local filesystem | Primary "data store" for a file manager app | Android Storage Access Framework + root (`libsu`/Shizuku) for elevated access | Broad storage permissions (`MANAGE_EXTERNAL_STORAGE`) increase blast radius of bugs | `app/src/main/AndroidManifest.xml` (storage permissions) |
| Local JSON files (paired devices) | UFM-Windows persists paired device list | `save_paired_devices`/`load_paired_devices` Tauri commands | `[TODO]` confirm file location/encryption | `UFM-Windows/src-tauri/src/lib.rs:9-10` |

### 3) Secrets and Credentials Handling

- Credential sources: OAuth client IDs/secrets for Google Drive and Dropbox are read from `local.properties` (developer machine) with environment-variable fallback, and a placeholder string (`"YOUR_SECRET_HERE"`/`"YOUR_KEY_HERE"`) if neither is set — `app/build.gradle.kts:64-90`.
- Hardcoding checks: No production secret values are hardcoded in the reviewed build script; only placeholder strings. `[TODO]` — confirm no `google-services.json` or `local.properties` files with real keys were accidentally committed (not checked in this pass).
- Rotation or lifecycle notes: `[TODO]` — unknown.

### 4) Reliability and Failure Behavior

- Retry/backoff behavior: `[TODO]` — not confirmed for rclone/cloud calls or the local `FileServer`.
- Timeout policy: `[TODO]`.
- Circuit-breaker or fallback behavior: `[TODO]`.

### 5) Observability for Integrations

- Logging around external calls: `[TODO]` — dedicated `CrashReportManager.kt` exists for crash reporting but its scope (network calls vs uncaught exceptions only) is `[TODO]`.
- Metrics/tracing coverage: Firebase Analytics is integrated (`gradle/libs.versions.toml`), but exact events tracked are `[TODO]`.
- Missing visibility gaps: `[ASK USER]` whether cloud/network integration failures are surfaced to users or only logged.

### 6) Evidence

- `app/build.gradle.kts`
- `gradle/libs.versions.toml`
- `app/libs/rclone.aar`
- `app/src/main/java/za/kilowatch/ultimatefilemanager/remote/FileServer.kt`
- `UFM-Windows/src-tauri/src/lib.rs`
- `size.md`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
