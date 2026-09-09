# Testing Patterns

## Core Sections (Required)

### 1) Test Stack and Commands

- Primary test framework: JUnit 4 (`junit = "4.13.2"`) for JVM unit tests; AndroidX Test JUnit (`1.1.5`) + Espresso (`3.5.1`) for instrumented tests — `gradle/libs.versions.toml`.
- Assertion/mocking tools: JUnit's built-in assertions; no separate mocking library (e.g. Mockito/MockK) found in `gradle/libs.versions.toml`.
- Commands:

```bash
./gradlew test                  # JVM unit tests (app/src/test)
./gradlew connectedAndroidTest  # Instrumented tests (app/src/androidTest), requires device/emulator
```

### 2) Test Layout

- Test file placement pattern: separate `app/src/test/java/...` (JVM/unit) and `app/src/androidTest/java/...` (instrumented) source sets, mirroring the production package path under `za/kilowatch/ultimatefilemanager/`.
- Naming convention: `<SubjectClassName>Test.kt`, e.g. `BatchRenameDiffTest.kt`, `NaturalSortTest.kt`, `RootDetectorTest.kt`.
- Setup files and where they run: `[TODO]` — no shared test base class or setup file was identified in this pass.

### 3) Test Scope Matrix

| Scope | Covered? | Typical target | Notes |
|-------|----------|-----------------|-------|
| Unit | Yes | Business/utility logic: archive extraction, batch rename, sort/filter, natural sort, hidden files, root detection, media/text-editor format support | `app/src/test/java/za/kilowatch/ultimatefilemanager/{archive,storage,settings,util,viewer,support}/*Test.kt` |
| Integration | No dedicated evidence found | — | `[TODO]` confirm |
| E2E | Minimal — only the default `ExampleInstrumentedTest.kt` template present | UI smoke test | `app/src/androidTest/java/za/kilowatch/ultimatefilemanager/ExampleInstrumentedTest.kt` |

### 4) Mocking and Isolation Strategy

- Main mocking approach: `[TODO]` — no mocking library present in the dependency catalog; tests likely exercise plain Kotlin/JVM logic (e.g. `NaturalSortTest`, `BatchRenameDiffTest`) rather than mocking Android framework classes.
- Isolation guarantees: `[TODO]`.
- Common failure mode in tests: `[TODO]`.

### 5) Coverage and Quality Signals

- Coverage tool + threshold: `[TODO]` — no coverage tool (e.g. Jacoco) configuration found.
- Current reported coverage: `[TODO]`.
- Known gaps/flaky areas: The default template tests (`ExampleUnitTest.kt`, `ExampleInstrumentedTest.kt`) are still present alongside real tests, suggesting these are unused boilerplate rather than gaps — `app/src/test/java/za/kilowatch/ultimatefilemanager/ExampleUnitTest.kt`, `app/src/androidTest/java/za/kilowatch/ultimatefilemanager/ExampleInstrumentedTest.kt`.
- UFM-Windows (Tauri/React): no test files, test framework, or test script were found in `UFM-Windows/package.json` or `UFM-Windows/src-tauri/Cargo.toml` — `[TODO]`/`[ASK USER]` whether the desktop companion app is intentionally untested at this stage.

### 6) Evidence

- `gradle/libs.versions.toml`
- `app/src/test/java/za/kilowatch/ultimatefilemanager/`
- `app/src/androidTest/java/za/kilowatch/ultimatefilemanager/`
- `UFM-Windows/package.json`

## Extended Sections (Optional)

Not populated — default mode used per skill instructions.
