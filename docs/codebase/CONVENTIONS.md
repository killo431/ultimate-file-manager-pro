# Coding Conventions

## Core Sections (Required)

### 1) Naming Rules

| Item | Rule | Example | Evidence |
|------|------|---------|----------|
| Kotlin files | PascalCase matching primary class, role suffix | `StorageBrowserActivity.kt`, `SettingsBackupManager.kt`, `NaturalSortTest.kt` | `app/src/main/java/za/kilowatch/ultimatefilemanager/storage/StorageBrowserActivity.kt`, `app/src/test/java/za/kilowatch/ultimatefilemanager/util/NaturalSortTest.kt` |
| Kotlin packages | lowercase, feature-based, no separators | `network`, `storage`, `billing` | `app/src/main/java/za/kilowatch/ultimatefilemanager/*` |
| TS/React files | PascalCase for components, camelCase for hooks | `FilePane.tsx`, `useUfmApi.ts` | `UFM-Windows/src/components/FilePane.tsx`, `UFM-Windows/src/hooks/useUfmApi.ts` |
| Constants (Gradle build) | camelCase for Kotlin DSL properties | `appVersionCode`, `appVersionName` | `app/build.gradle.kts` |
| Env vars | SCREAMING_SNAKE_CASE | `GOOGLE_DRIVE_TV_CLIENT_ID`, `DROPBOX_APP_KEY` | `app/build.gradle.kts:64-90` |

### 2) Formatting and Linting

- Formatter: `[TODO]` — no `.editorconfig`, ktlint, or detekt config file found in the repository root or `app/`.
- Linter: `[TODO]` — no lint config files found by the scan; IDE code style is defined at `.idea/codeStyles/Project.xml` (project-specific IntelliJ/Android Studio style), but this is not an enforced CI/CLI tool.
- Most relevant enforced rules: `[TODO]` — none detected as enforced via build scripts.
- Run commands: `[TODO]` — no lint Gradle task or npm script found beyond TypeScript's own `tsc` type-check (`UFM-Windows/package.json`: `"build": "tsc && vite build"`).

### 3) Import and Module Conventions

- Import grouping/order: `[TODO]` — not verified from source (no enforced import-order lint config found).
- Alias vs relative import policy: UFM-Windows uses relative imports only; `tsconfig.json` has no `paths` alias configuration — `UFM-Windows/tsconfig.json`.
- Public exports/barrel policy: `[TODO]` — not confirmed.

### 4) Error and Logging Conventions

- Error strategy by layer: `[TODO]` — requires deeper reading of representative files; a dedicated `support/CrashReportManager.kt` (4,306 lines) exists for crash reporting, suggesting centralized crash handling — `app/src/main/java/za/kilowatch/ultimatefilemanager/support/CrashReportManager.kt`.
- Logging style and required context fields: `[TODO]`.
- Sensitive-data redaction rules: `[TODO]` — OAuth client secrets are read from `local.properties`/env vars with placeholder fallbacks (`"YOUR_SECRET_HERE"`), not hardcoded real secrets — `app/build.gradle.kts:64-90`.

### 5) Testing Conventions

- Test file naming/location rule: JVM unit tests under `app/src/test/java/.../<package>/<Name>Test.kt`, mirroring the production package structure; instrumented tests under `app/src/androidTest/java/...` — `app/src/test/java/za/kilowatch/ultimatefilemanager/storage/BatchRenameDiffTest.kt`.
- Mocking strategy norm: `[TODO]` — not confirmed; no mocking library (Mockito/MockK) found in the version catalog (`gradle/libs.versions.toml`), suggesting tests may favor plain JVM logic tests over mocked Android framework classes.
- Coverage expectation: `[TODO]` — no coverage tool/threshold config found.

### 6) Evidence

- `.idea/codeStyles/Project.xml`
- `app/src/test/java/za/kilowatch/ultimatefilemanager/`
- `UFM-Windows/tsconfig.json`
- `gradle/libs.versions.toml`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
