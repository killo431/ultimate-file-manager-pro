---
applyTo: "**"
description: "Documentation standards for the UFM codebase"
---
# Documentation standards

Apply the repository-wide guidance from `../copilot-instructions.md`.

## General Guidelines
- Document all public APIs: Kotlin public classes/functions with KDoc, Rust public items with `///` rustdoc comments, and exported TypeScript functions/hooks with JSDoc/TSDoc where behavior isn't obvious from the signature.
- Prefer self-explanatory code and names over comments; reserve comments for explaining *why*, not *what* (non-obvious algorithms, security-sensitive decisions, external constraints).
- Keep `README.md`, `UFM-Windows/README.md`, and `size.md` up to date when a change affects build steps, supported platforms, or app size/footprint.
- Update `CHANGELOG.md` for user-facing changes, following the existing format.

## Feature-Level Documentation
- When adding a new network protocol, pairing mechanism, or cross-platform feature, document the protocol/flow (e.g. in code comments or a short doc) so both the Android and Windows companion implementations can be kept in sync.

<!-- Based loosely on: https://github.com/github/awesome-copilot/blob/main/instructions/self-explanatory-code-commenting.instructions.md -->
