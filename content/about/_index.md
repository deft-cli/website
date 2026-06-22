---
title: About
---

C and C++ tooling is fragmented. `deft` brings a familiar, manifest-driven
package-manager workflow while staying dependency-free itself — it shells out
to tools your system already has (`clang`, `git`, `curl`/`wget`/PowerShell,
`ar`/`llvm-ar`/`lib.exe`) instead of bundling an HTTP client, VCS library, or
archiver crate. See [docs/architecture.md](../docs/guides/architecture) for the full
rationale.
