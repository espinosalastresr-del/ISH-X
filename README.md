# iSH-X

ARM64 Linux userspace runtime for iOS, derived from the iSH/iSH-AOK architecture.

## Goals
- ARM64 (aarch64) guest only.
- Alpine Linux AArch64 root filesystem managed separately from the IPA.
- Gadget JIT is the mandatory functional baseline.
- StikDebug/StikJIT is optional and must never be required for startup.
- Linux UID 0 is guest/fake root, never iOS/XNU root.
- iOS device capabilities are exposed through explicit public iOS APIs via NativeBridge.
- No direct hardware, private API, kernel-driver, or XNU access.

See docs/ARCHITECTURE.md and docs/DEVELOPMENT.md for the project contract.

## Status
Bootstrap repository created. Runtime source import and ARM64 bring-up are next.
