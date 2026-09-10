<!-- Inspired by: https://github.com/github/awesome-copilot/blob/main/instructions/security-and-owasp.instructions.md -->
---
applyTo: "**"
description: "Security best practices for the UFM Android app and Windows companion"
---
# Security standards

Apply the repository-wide guidance from `../copilot-instructions.md`. This project pairs devices over a local network and handles user file systems and credentials, so security-sensitive code requires extra scrutiny.

## Core Principles
- Never commit secrets, API keys, or signing credentials to source control; use Gradle/Cargo secret mechanisms and `.gitignore` appropriately.
- Treat all network input (LAN discovery broadcasts, pairing requests, remote file listings/uploads) as untrusted until validated.
- Preserve the existing PIN + TLS certificate-pinning pairing flow between the Android app and Windows companion; do not weaken or bypass it for convenience.
- Validate and sanitize file paths from remote sources (SMB/SFTP/FTP/WebDAV/S3, paired devices) to prevent path traversal.
- Keep the FOSS build free of telemetry, analytics, and closed-source SDKs — this is both a privacy commitment and a security-surface reduction.

## Platform-Specific
- **Android**: follow Android security best practices for storage permissions (scoped storage), and avoid exporting components unnecessarily.
- **Rust/Tauri**: restrict the Tauri capabilities/permissions manifest (`src-tauri/capabilities`) to the minimum needed; validate all IPC command arguments from the frontend.
- **React/TypeScript**: avoid rendering unsanitized remote content (file names, previews) in ways that could enable injection.

## Review Expectations
- Flag any change to authentication, pairing, TLS handling, or credential storage for extra review scrutiny before merge.
