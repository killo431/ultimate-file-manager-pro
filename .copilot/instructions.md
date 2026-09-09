# GitHub Copilot Contribution Guidelines

**Ultimate File Manager Pro** is a dual-pane file manager for Android Mobile, Android TV, and Windows PC, built with Kotlin (Android) and Rust/React (Windows Companion).

This document provides AI agents and GitHub Copilot with structured guidance for contributing to this project.

---

## 🎯 Core Project Architecture

### Stack Overview

| Component | Language | Framework | Purpose |
|-----------|----------|-----------|---------|
| **Android Mobile/TV** | Kotlin | Android SDK | Core file manager application |
| **Windows Companion** | Rust/React | Tauri 2 | Desktop app for PC-to-Android sync |
| **Backend** | Rust | reqwest, tokio | Networking, TLS, file transfers |
| **Frontend** | TypeScript/React | React 19, Vite | Desktop UI with glassmorphic design |

### Key Modules

- **Android App**: Dual-pane interface, cloud storage (SMB, SFTP, FTP, WebDAV, S3), APK sideloading
- **Windows Companion**: Zero-Conf LAN discovery, secure TLS handshake, file transfer engine
- **Localization**: 17 languages with zero-dependency flag rendering
- **Security**: Path sanitization, TOFU certificate verification, CSP compliance

---

## 🔍 Pre-Contribution Checklist

Before opening **any** pull request, verify:

- [ ] **Is this a real, reproducible problem?** (Not vague like "improve performance")
- [ ] **Which component does it affect?** (Android, Windows, both, documentation)
- [ ] **Have I searched for existing PRs?** (Open AND closed, across all branches)
- [ ] **Does the fix belong in core or a plugin?** (Core: main app; Plugin: domain-specific)
- [ ] **Have I read the relevant component README?**
  - Android: `README.md`
  - Windows: `UFM-Windows/README.md`
- [ ] **Can I reproduce it locally?** (Build debug APK or run dev server)
- [ ] **Will my change break existing tests?** (Run test suite first)

---

## 📋 PR Submission Requirements

### Template Compliance
Every PR MUST include:
1. **Problem Statement** — What broke? What's the user impact?
2. **Reproduction Steps** — How to see the issue (APK version, device, steps)
3. **Proposed Fix** — Technical approach and design decisions
4. **Testing Strategy** — How to verify the fix works
5. **Component** — Which area(s) affected (Android/Windows/Shared)
6. **Screenshots/Videos** — For UI/UX changes (especially for TV D-pad navigation)

### Branch Strategy
- **Target `main`** for bug fixes and features (unless specified otherwise)
- **Create a feature branch** from `main` before starting work
- **Branch naming**: `fix/issue-description` or `feat/feature-name`

### Duplicate Detection
- Search both open AND closed PRs for the same issue
- Check the GitHub Issues page for related discussions
- Reference what you found in your PR description
- If a prior attempt failed, explain why yours will succeed

### Transparency Requirements
Disclose the following in your PR:
- **AI Model**: e.g., Claude 3.5 Sonnet
- **Copilot Version**: e.g., Claude Code v1.2.3
- **Environment**: Android Studio version, Node.js version, Rust version
- **Plugins Used**: Any extensions or tools that aided development
- **Or state**: "This was written by hand with no AI assistance"

**Hiding agent authorship = automatic rejection.**

### Human Review Requirement
- Your human partner MUST review the complete diff before submission
- Demonstrate the fix works (screenshot, video, or test results)
- Get explicit written approval
- No exceptions for "obvious" changes

---

## ✅ What We Will Accept

### Genuine Bug Fixes
- Clear reproduction steps and failing test case
- Specific component affected (Android/Windows)
- Before/after behavior documented
- No regressions in other features

### Performance Improvements
- Metrics showing improvement (speed, memory, storage)
- Benchmarks before/after
- No compromise on features or security

### Feature Additions (Core Only)
- Aligned with project vision (dual-pane, privacy-first, cross-platform)
- Scoped to one component or well-integrated across components
- Complete implementation with tests
- Documentation updated (README, in-app help)

### Documentation & Examples
- Clearer setup instructions
- Better build documentation
- Useful examples for contributors
- Localization improvements

### Plugin/Extension Work
- Domain-specific features (file type handlers, archive support)
- Third-party integrations (cloud storage, media players)
- Platform-specific enhancements (keyboard shortcuts, themes)

---

## ❌ What Will Be Rejected

❌ **"Slop" PRs** — Low-quality, unfocused, or placeholder work  
❌ **Hidden authorship** — No disclosure of AI model, environment, or plugins  
❌ **Duplicate efforts** — Same issue as a closed PR without explanation of differences  
❌ **Unreviewed diffs** — Submitted without human partner approval  
❌ **Incomplete templates** — Missing sections or placeholder text  
❌ **Vague claims** — "Improves performance" without metrics or benchmarks  
❌ **Third-party dependencies** — New external libraries without discussion  
❌ **Breaking changes** — API changes without backward compatibility plan  
❌ **TV/Mobile untested** — Desktop-only testing for cross-platform features  

---

## 🚀 AI Agent Workflow

### Phase 1: Investigation (No PR Yet)
1. **Understand the problem** — Work with your human partner to define a specific issue
2. **Read relevant code** — Study the component architecture (Android/Windows/shared)
3. **Search existing issues & PRs** — Both open and closed
4. **Reproduce the issue** — Build the app, verify the problem exists
5. **Evaluate scope** — Is this a core fix or a plugin?
6. **Report findings** — Tell your human partner what you discovered

**Outcome**: Decision to proceed with a PR or identify why it's not ready

### Phase 2: Implementation (Still No PR)
1. **Write a test** that demonstrates the bug or desired behavior
2. **Implement the fix** in your working branch
3. **Run existing test suite** to ensure no regressions
4. **Build and test locally** (debug APK for Android, dev server for Windows)
5. **Create the diff** — don't open the PR yet
6. **Show your human partner** the complete proposed changes
7. **Get explicit approval** — they should understand every line

**Outcome**: Approved diff ready for PR submission

### Phase 3: PR Submission
1. **Complete the PR template** — every section with specific, real answers
2. **Reference investigation** — link to issues, prior PRs, or discussions
3. **Identify yourself** — disclose model, version, environment
4. **Include evidence** — test results, screenshots, metrics
5. **Submit and monitor** — respond to maintainer feedback promptly

**Outcome**: PR opens for review

### Phase 4: Review & Iteration
- Respond to feedback within 48 hours
- Request clarification if guidance is unclear
- Don't assume silence means approval
- Update the PR if requested
- Be collaborative and respectful

---

## 🛠️ Component-Specific Guidelines

### Android Development (Kotlin)

**Build Commands:**
```bash
# Debug builds
./gradlew assembleMobileFossDebug   # Mobile
./gradlew assembleTvFossDebug       # TV

# Release builds
./gradlew assembleMobileFossRelease # Mobile
./gradlew assembleTvFossRelease     # TV
```

**Testing Requirements:**
- Test on both Mobile (phone/tablet) and TV (D-pad navigation)
- Verify file operations (copy, move, delete, rename)
- Test cloud storage backends (SMB, SFTP, FTP, WebDAV, S3)
- Check memory usage and scroll performance
- Verify localization (test at least 2 languages)

**Code Style:**
- Follow Kotlin conventions (nullable types, extension functions)
- Use coroutines for async operations
- Minimize allocations in hot paths
- Comment non-obvious logic

**D-Pad Navigation (TV):**
- All UI elements must be navigable with D-pad
- Focus indicators must be clearly visible
- Action buttons must respond to DPAD_CENTER and ENTER keys
- No mouse-only UI elements

### Windows Companion (Rust/React)

**Build Commands:**
```bash
cd UFM-Windows
npm install
npm run tauri dev      # Development mode with HMR
npm run tauri build    # Production (.exe and .msi)
```

**Testing Requirements:**
- Test on Windows 10+
- Verify LAN discovery on multi-NIC systems
- Test large file transfers (2GB+)
- Verify TLS certificate pinning and TOFU
- Test path sanitization (no directory traversal)
- Verify CSP headers and security policies

**Code Style:**
- Rust: idiomatic, use Result types for error handling
- React: functional components, hooks (no class components)
- TypeScript: strict mode, no `any` types
- Styling: CSS variables for theme consistency

**UI Changes:**
- Maintain glassmorphic design language
- Test on dark background (no light mode)
- Verify responsive behavior on 1920x1080 to 4K
- Check accessibility (keyboard navigation, screen reader)

### Localization (17 Languages)

**Adding/Modifying Translations:**
- Update `translations.ts` in UFM-Windows
- Add Base64 PNG flags for new locales (if needed)
- Test right-to-left (RTL) languages (Arabic, Hebrew)
- Verify character encoding (UTF-8 for all languages)

---

## 🔐 Security Considerations

### TLS & Certificate Handling
- All communication uses TLS 1.2/1.3
- No plain HTTP endpoints
- Certificates verified by SHA-256 fingerprint (TOFU)
- Certificate pinning prevents MITM attacks

### Path Sanitization
- All file paths checked for `..` traversal attempts
- No symlink following without explicit consent
- Verify operations can't escape app's intended scope

### Sensitive Data
- PIN codes must not be logged
- TLS fingerprints stored securely (no plaintext)
- Credentials only in memory during session
- No hardcoded secrets in code

---

## 📚 Useful Resources

| Resource | Purpose | Location |
|----------|---------|----------|
| Main README | Project overview, features, downloads | `README.md` |
| Windows Guide | Desktop companion architecture & setup | `UFM-Windows/README.md` |
| License | GPL v3.0 terms | `LICENSE` |
| GitHub Issues | Bug reports, feature requests | GitHub Issues tab |
| GitHub Discussions | Architecture questions, design decisions | GitHub Discussions tab |

---

## 🤝 Community & Support

- **GitHub Sponsors**: Support development → [github.com/sponsors/Kilowatch](https://github.com/sponsors/Kilowatch)
- **XDA Developers Forum**: Community feedback → [XDA Thread](https://xdaforums.com/t/app-free-ultimate-file-manager-pro-dual-pane-android-mobile-android-tv-foss-edition-available.4791958/)
- **Reddit**: Community discussions → [r/UFManagerPro](https://www.reddit.com/r/UFManagerPro/)
- **Issues & Discussions**: Use GitHub for bugs, features, and technical questions

---

## ✨ High-Bar Quality Standards

This project targets **power users** and **developers** who expect:
- ⚡ **Performance** — Smooth scrolling, fast file operations, low memory usage
- 🔒 **Security** — No trackers, no telemetry, local-first architecture
- 🌍 **Cross-Platform** — Consistent experience on mobile, TV, and desktop
- 🎯 **Reliability** — Handles edge cases (large files, slow networks, device sleep/wake)
- 📚 **Maintainability** — Clear code, good documentation, minimal technical debt

Your contribution should uphold these standards.

---

## 📝 Key Principles

1. **Agents serve humans** — Help your human partner succeed; don't farm low-quality contributions
2. **Quality over speed** — One excellent PR beats five rejected ones
3. **Transparency builds trust** — Disclose your authorship completely
4. **Test everything** — Especially cross-platform features (Android + Windows)
5. **Search first** — Duplicate work is the #1 cause of rejection
6. **Read the docs** — Component READMEs exist for a reason
7. **Respect the bar** — This is a high-quality project; match that standard

---

**Last Updated**: September 2026  
**Repository**: https://github.com/killo431/ultimate-file-manager-pro
