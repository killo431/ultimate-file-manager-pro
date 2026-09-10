---
applyTo: "**"
description: "Testing standards and practices across the Android app and Windows companion"
---
# Testing standards

Apply the repository-wide guidance from `../copilot-instructions.md`. This is a strict-standards project: new and changed behavior must be covered by tests.

## General Guidelines
- Write unit tests for all new public functions, classes, and Tauri commands; add integration or instrumentation tests for cross-boundary behavior (network protocols, file I/O, IPC).
- Follow the test pyramid: prefer many fast unit tests, fewer integration tests, and a small number of end-to-end/instrumentation tests.
- Keep tests deterministic — mock network calls (SMB/SFTP/FTP/WebDAV/S3, LAN discovery, pairing) rather than depending on live services.
- Name tests descriptively so failures are self-explanatory without reading the implementation.

## Android
- Place unit tests in `app/src/test` and instrumentation tests in `app/src/androidTest`, following the existing structure.
- Test flavor-specific logic against the flavor it belongs to; do not assume FOSS-only behavior works identically in `google`/`amazon` flavors.

## Rust/Tauri
- Use `#[cfg(test)]` modules alongside the code under test for unit tests; use the `tests/` directory for integration tests.
- Cover error paths (invalid paths, network failures, malformed pairing data) as thoroughly as happy paths.

## React/TypeScript
- Test components and hooks for both success and error/loading states, especially around Tauri `invoke` calls.

## Before Submitting
- Run the smallest targeted test command that covers your change; escalate to the full suite only if targeted tests reveal broader impact.
