<!-- Inspired by: https://github.com/github/awesome-copilot/blob/main/instructions/performance-optimization.instructions.md -->
---
applyTo: "**"
description: "Performance optimization guidelines for the UFM Android app and Windows companion"
---
# Performance standards

Apply the repository-wide guidance from `../copilot-instructions.md`.

## General Guidelines
- Profile before optimizing; avoid speculative micro-optimizations that harm readability without measured benefit.
- Keep large file operations (copy, move, archive extraction, video thumbnail generation) off the main/UI thread using coroutines (Kotlin) or async tasks (Rust/Tauri) and stream data instead of loading entire files into memory.
- Avoid unnecessary allocations and cloning in hot paths, especially in Rust file-transfer and network code.

## Android
- Avoid overdraw and unnecessary recompositions/layout passes in dual-pane views; use lazy lists for large directory listings.
- Generate video/image thumbnails asynchronously and cache results instead of regenerating them on every scroll.

## Windows Companion
- Keep the Tauri app's resource footprint low (its selling point is low overhead vs. Electron alternatives); avoid bundling unnecessary dependencies.
- Stream file transfer progress rather than polling or blocking the UI thread; batch small file operations where possible.

## React/TypeScript
- Memoize expensive renders (large directory trees) and avoid unnecessary re-renders during active file transfers.
