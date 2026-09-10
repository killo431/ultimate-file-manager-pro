---
applyTo: "app/**/*.kt,app/**/*.kts"
description: "Kotlin/Android development standards for the UFM mobile and TV app"
---
# Kotlin & Android coding standards

Apply the repository-wide guidance from `../copilot-instructions.md` to all code.

## General Guidelines
- Follow official Kotlin coding conventions (`camelCase` for functions/properties, `PascalCase` for classes/objects, `UPPER_SNAKE_CASE` for constants).
- Prefer immutability (`val` over `var`) and null-safety idioms over defensive nullability checks.
- Use data classes for simple value holders and sealed classes/interfaces to model restricted state (e.g. UI/network results).
- Prefer coroutines and structured concurrency over raw threads or callbacks for asynchronous work.
- Keep Composable/UI code, ViewModels, and data/repository layers separated according to the existing architecture in `app/src`.

## Android/Gradle Specifics
- Respect the existing product flavor structure (`foss`, `google`, `amazon`, `nonfoss` × `mobile`, `tv`); never leak `google`/`nonfoss`-only dependencies (Play Services, Firebase) into `foss` source sets.
- Keep TV-specific UI (D-pad navigation, Leanback patterns) isolated to the `tv` flavor/source sets rather than branching at runtime.
- Prefer Gradle version catalogs (`libs.versions.toml`) over hardcoded dependency versions.
- Avoid adding new closed-source SDKs or telemetry to FOSS source sets, consistent with the project's privacy-first policy.

## Error Handling
- Use `Result`, sealed classes, or exceptions consistently with the surrounding module — do not mix styles within the same feature.
- Never silently swallow exceptions from I/O, network, or file-system operations; log or surface them appropriately.
- Validate user-provided paths and network inputs (SMB/SFTP/FTP/WebDAV/S3 credentials, remote hosts) before use.

<!-- No exact Kotlin/Android match was found in github/awesome-copilot/instructions at the time of writing; this file follows general Kotlin/Android community conventions. -->
