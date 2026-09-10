---
applyTo: "UFM-Windows/src/**/*.ts,UFM-Windows/src/**/*.tsx"
description: "React 19 + TypeScript development standards for the Windows companion UI"
---
# React & TypeScript coding standards

Apply the repository-wide guidance from `../copilot-instructions.md` to all code.

## General Guidelines
- Use TypeScript strict mode; avoid `any` in favor of precise types or generics.
- Prefer function components with hooks over class components.
- Keep components small and focused; extract shared logic into custom hooks under a dedicated hooks directory.
- Name components in `PascalCase` and hooks with a `use` prefix in `camelCase`.
- Use `lucide-react` icons and existing UI patterns rather than introducing new UI libraries without discussion.

## State & Data Flow
- Keep Tauri `invoke` calls (IPC to the Rust backend) in a thin, typed API layer rather than scattered throughout components.
- Handle IPC/network failures explicitly in the UI (loading, error, empty states) rather than assuming success.
- Prefer local component state and React context over introducing new global state libraries unless the existing codebase already uses one.

## Error Handling
- Surface errors from Tauri commands and network operations (pairing, LAN discovery, file transfer) to the user with actionable messaging.
- Do not swallow promise rejections; always handle or propagate them.

<!-- No exact React/Tauri-frontend match was found in github/awesome-copilot/instructions at the time of writing; this file follows general React + TypeScript community conventions. -->
