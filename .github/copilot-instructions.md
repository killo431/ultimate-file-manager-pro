# Ultimate File Manager Pro — Copilot Instructions

## Project Overview
Ultimate File Manager Pro (UFM) is a dual-pane file manager shipped across three targets from one repository:

- **Android Mobile / Android TV** — Kotlin app in `app/`, built with Gradle product flavors (`foss`, `google`, `amazon`, `nonfoss` × `mobile`, `tv`).
- **Windows Companion** — a Tauri 2 desktop app in `UFM-Windows/`, combining a Rust backend (`src-tauri/`) with a React 19 + TypeScript + Vite frontend (`src/`).

The Windows companion pairs with the Android/TV app over the local network (mDNS discovery, PIN + TLS pairing) for wireless file transfer and remote APK sideloading. The FOSS edition in this repository excludes proprietary cloud SDKs and telemetry.

## Tech Stack
- **Android**: Kotlin, Gradle (Kotlin DSL), AndroidX/Jetpack, KSP, Google Play Services (non-FOSS flavors only), Firebase (non-FOSS flavors only).
- **Windows Companion backend**: Rust, Tauri 2 (`src-tauri/`), Cargo.
- **Windows Companion frontend**: React 19, TypeScript, Vite, `lucide-react`.
- **Networking**: SMB, SFTP, FTP, WebDAV, S3-compatible storage; local LAN discovery and TLS-pinned pairing between the Windows companion and the Android app.

## Conventions
- **Naming**: Kotlin follows standard Kotlin/Android conventions (`PascalCase` for classes, `camelCase` for functions/properties, `UPPER_SNAKE_CASE` for constants). Rust follows `snake_case` for functions/variables and `PascalCase` for types, per `rustfmt`/`clippy` defaults. TypeScript/React follows `camelCase` for variables/functions and `PascalCase` for components.
- **Structure**: Keep Android flavor-specific code isolated under the appropriate Gradle source sets rather than branching with runtime flags. Keep Tauri IPC command definitions in `src-tauri/src` colocated with the feature they back, and keep the React frontend organized by feature/screen under `UFM-Windows/src`.
- **Error handling**: Rust code must propagate errors with `Result<T, E>` (no unhandled `unwrap()`/`expect()` in production paths). Kotlin should use sealed result types or exceptions consistent with existing patterns in the module being touched. React/TypeScript should surface errors to the UI rather than swallowing them silently.
- **Privacy first**: Never add closed-source SDKs, analytics, or telemetry to the FOSS build paths (`foss` flavor, `UFM-Windows`).

## Workflow
- Commit messages should be clear and scoped to one logical change; reference issue numbers where applicable.
- Open PRs against `main`; keep Android and Windows companion changes in separate PRs unless a change spans both by necessity (e.g. a shared pairing protocol change).
- This is a strict-standards project: prefer strong typing, comprehensive tests for changed behavior, and request review before merging non-trivial changes.
- Reference specific instruction files for detailed standards:
  - Kotlin/Android guidelines: `.github/instructions/kotlin.instructions.md`
  - Rust/Tauri guidelines: `.github/instructions/rust.instructions.md`
  - React/TypeScript guidelines: `.github/instructions/react-typescript.instructions.md`
  - Testing: `.github/instructions/testing.instructions.md`
  - Security: `.github/instructions/security.instructions.md`
  - Documentation: `.github/instructions/documentation.instructions.md`
  - Performance: `.github/instructions/performance.instructions.md`
  - Code review: `.github/instructions/code-review.instructions.md`

## Meta-tooling note
This repository's `.github/agents/`, `.github/skills/`, and `.github/workflows/` directories also contain generic Copilot-ecosystem meta-tooling (skill/plugin validation, contribution automation) unrelated to UFM's own build. The project-specific guidance above and the files it references take precedence for anything touching `app/` or `UFM-Windows/`.
