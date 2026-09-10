---
applyTo: "**"
description: "Code review standards for pull requests in this repository"
---
# Code review standards

Apply the repository-wide guidance from `../copilot-instructions.md`. This is a strict-standards project — reviews should be thorough and formal review gates should be respected before merge.

## What to Check
- Correctness of the change against its stated intent, including edge cases (empty directories, permission errors, network timeouts, malformed remote paths).
- Test coverage for new/changed behavior; flag missing tests rather than assuming they exist.
- Adherence to the language-specific instruction files (Kotlin, Rust/Tauri, React/TypeScript) and the security and performance guidance.
- No leaked secrets, credentials, or closed-source dependencies introduced into FOSS build paths.
- Cross-platform impact: if a change touches the pairing/transfer protocol, confirm both the Android and Windows companion sides are updated consistently.

## Review Etiquette
- Prefer specific, actionable feedback over general comments; suggest concrete fixes where possible.
- Separate blocking issues (correctness, security) from suggestions (style, minor performance) so authors can prioritize.
- Approve only once tests pass and review comments are resolved or explicitly deferred with justification.
