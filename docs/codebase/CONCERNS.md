# Codebase Concerns

## Core Sections (Required)

### 1) Top Risks (Prioritized)

| Severity | Concern | Evidence | Impact | Suggested action |
|----------|---------|----------|--------|------------------|
| High | Several Activity classes are extremely large (2,000–7,800+ lines), mixing UI, business logic, and I/O in single files | `app/src/main/java/za/kilowatch/ultimatefilemanager/network/NetworkBrowserActivity.kt` (7,803 lines), `storage/FileBrowserActivity.kt` (7,183 lines), `storage/StorageBrowserActivity.kt` (5,232 lines), `remote/FileServer.kt` (4,292 lines), `support/CrashReportManager.kt` (4,306 lines) | Hard to test, review, and safely modify; high regression risk for any change | Incrementally extract cohesive units (e.g. helpers, smaller classes) from the largest files during future feature work |
| Medium | No lint/formatter (ktlint/detekt/ESLint/Prettier) configuration found anywhere in the repo | Scan output: `LINTING AND FORMATTING CONFIG` section — none found | Style drift and preventable bugs (e.g. unused vars) not caught automatically | `[ASK USER]` whether the team wants to introduce ktlint/detekt and ESLint/Prettier |
| Medium | UFM-Windows (Tauri companion) has no test suite at all | No `test` script in `UFM-Windows/package.json`; no test files under `UFM-Windows/src` or `src-tauri/src` | Regressions in the desktop companion (file transfer, device pairing) are only caught manually | `[ASK USER]` priority of adding UFM-Windows tests |
| Low | `app/libs/rclone.aar` (17.9 MB) is a large vendored binary checked into the repo | `app/libs/rclone.aar: 17947.2KB` (scan CODE METRICS) | Repo bloat, harder to audit vendored code, no visibility into what version/patches are applied | `[ASK USER]` whether this should move to a proper dependency/artifact repository |

### 2) Technical Debt

| Debt item | Why it exists | Where | Risk if ignored | Suggested fix |
|-----------|----------------|-------|------------------|----------------|
| Unused example test boilerplate still present | Default Android Studio project template tests were never removed | `app/src/test/java/za/kilowatch/ultimatefilemanager/ExampleUnitTest.kt`, `app/src/androidTest/java/za/kilowatch/ultimatefilemanager/ExampleInstrumentedTest.kt` | Minor — clutters test output/reports | Delete once confirmed unused |
| Custom Gradle `StripFtpEncoder` artifact transform to work around a duplicate-class conflict from `ftpserver-core` | Upstream FTP server library ships a class the project needed to patch locally | `app/build.gradle.kts:1-46` | Fragile — depends on internal class names of a third-party JAR; could silently break on a dependency upgrade | Track upstream fix or replace the FTP server dependency; add a regression test/build check that fails loudly if the transform's target class disappears/changes |
| Placeholder OAuth credentials as fallback values | Enables building without secrets configured | `app/build.gradle.kts:64-90` (`"YOUR_SECRET_HERE"`, etc.) | Low — expected for open-source distribution, but could confuse contributors if cloud features silently fail | Document in README that these must be supplied via `local.properties`/env vars for cloud features to work |

### 3) Security Concerns

| Risk | OWASP category (if applicable) | Evidence | Current mitigation | Gap |
|------|----------------------------------|----------|---------------------|-----|
| Broad storage permissions (`MANAGE_EXTERNAL_STORAGE`, `QUERY_ALL_PACKAGES`) | A01: Broken Access Control (excessive privilege) | `app/src/main/AndroidManifest.xml` | Required for a file manager's core functionality; flagged with `tools:ignore` | `[ASK USER]`/`[TODO]` confirm these permissions are requested with clear user-facing rationale (Play Store policy compliance) |
| Local HTTP file server (`remote/FileServer.kt`) exposes device files over LAN | A01/A07 (auth/session) | `app/src/main/java/za/kilowatch/ultimatefilemanager/remote/FileServer.kt` | `[TODO]` — auth/pairing mechanism not confirmed from this pass | `[ASK USER]` what authentication (pairing token, PIN, etc.) protects this server from unauthorized LAN access |
| Root/elevated execution via Shizuku/libsu | A01 (privilege escalation surface) | `gradle/libs.versions.toml` (`shizuku`, `libsu`), manifest Shizuku permissions | Requires explicit user grant of Shizuku/root access | Standard risk for file managers offering root features; ensure root-only code paths are well isolated (`[TODO]` verify) |

### 4) Performance and Scaling Concerns

| Concern | Evidence | Current symptom | Scaling risk | Suggested improvement |
|---------|----------|-------------------|---------------|-------------------------|
| Very large single-file Activities implicated in browsing large folders (dual-pane, TV) | `size.md` mentions "silky-smooth... even when browsing huge folders with dual-pane view on low-powered Android TVs"; `storage/FileBrowserActivity.kt` (7,183 lines) | `[TODO]` — no profiling data available in this pass | Low-powered TV devices are an explicit target, so any inefficiency compounds | `[ASK USER]` whether folder listing/thumbnailing is paginated/virtualized already |

### 5) Fragile/High-Churn Areas

| Area | Why fragile | Churn signal | Safe change strategy |
|------|-------------|---------------|------------------------|
| `settings/SettingsActivity.kt`, `settings/SettingsBackupManager.kt` | Central configuration surface touched by many features | Appears in `HIGH-CHURN FILES` scan output (2 changes each in last 90 days, tied for top churn alongside several other files) | Add/extend unit tests before modifying (existing coverage: `RootPreferenceManagerTest.kt`, `PlayerPreferencesManagerTest.kt`) |
| `network/NetworkBrowserActivity.kt`, `network/PairingServer.kt` | Networking + device pairing logic, also very large file | High-churn list; also largest file in the codebase | Extract pairing/discovery logic into smaller, independently testable units before further changes |
| `viewer/UFMPlaybackService.kt`, `viewer/UFMPlayerActivity.kt`, `viewer/PlayerGestureController.kt` | Media playback stack (ExoPlayer/Media3) with gesture handling | High-churn list | Rely on existing `MediaFormatSupportTest.kt` and extend before touching playback logic |

### 6) `[ASK USER]` Questions

1. [ASK USER] Should the large Activity classes (`NetworkBrowserActivity.kt`, `FileBrowserActivity.kt`, `StorageBrowserActivity.kt`, `FileServer.kt`) be refactored into smaller components, or is this considered acceptable given the project's current stage?
2. [ASK USER] What authentication/pairing mechanism protects the local `FileServer` (used by the UFM-Windows companion) from unauthorized access on the same LAN?
3. [ASK USER] Is there a plan to add automated tests for the UFM-Windows (Tauri/React) companion app, which currently has none?
4. [ASK USER] Should ktlint/detekt (Kotlin) and ESLint/Prettier (TypeScript) be introduced, since no linting/formatting tooling is currently enforced?
5. [ASK USER] Should the vendored `app/libs/rclone.aar` (17.9 MB) be sourced from a dependency repository instead of being committed directly?

### 7) Evidence

- Scan output sections: `TODO / FIXME / HACK`, `HIGH-CHURN FILES`, `CODE METRICS`
- `app/build.gradle.kts`
- `app/src/main/AndroidManifest.xml`
- `app/src/main/java/za/kilowatch/ultimatefilemanager/remote/FileServer.kt`
- `UFM-Windows/package.json`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
