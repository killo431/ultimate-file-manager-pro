<!-- Based on: https://github.com/github/awesome-copilot/blob/main/instructions/rust.instructions.md -->
---
applyTo: "UFM-Windows/src-tauri/**/*.rs"
description: "Rust/Tauri development standards for the Windows companion backend"
---
# Rust & Tauri coding standards

Apply the repository-wide guidance from `../copilot-instructions.md` to all code.

## General Guidelines
- Always prioritize readability, safety, and maintainability; leverage Rust's ownership system for memory safety.
- Write idiomatic, safe Rust that follows the borrow checker's rules; avoid `unsafe` unless required and fully documented.
- Prefer borrowing (`&T`) over cloning unless ownership transfer is necessary; avoid unnecessary allocations.
- Split code into focused modules (`mod`) with clear public interfaces (`pub`).
- Ensure code compiles without warnings and passes `cargo clippy`.

## Tauri-Specific Guidance
- Keep Tauri IPC commands thin: validate input, delegate to a well-tested internal function, and map errors to a serializable error type returned to the frontend.
- Never trust paths, hostnames, or credentials received from the frontend or the network (LAN discovery, PIN pairing) without validation.
- Keep TLS/PIN pairing and certificate-pinning logic isolated and well-documented, since it is security-critical.

## Error Handling
- Use `Result<T, E>` for recoverable errors and `panic!` only for truly unrecoverable states.
- Prefer the `?` operator over `unwrap()`/`expect()` for error propagation; do not use them in production code paths.
- Create custom error types with `thiserror` (or implement `std::error::Error`) and provide meaningful, non-leaky error messages back across the Tauri IPC boundary.

## Code Style and Formatting
- Follow the Rust Style Guide and format with `rustfmt`.
- Keep lines under 100 characters when practical.
- Document public items with `///` doc comments.
