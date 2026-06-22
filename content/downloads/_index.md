---
title: "Downloads"
---

### Since Deft is currently in its early deployment phase, we distribute it directly via source compilation to ensure complete transparency and zero environmental friction.

```sh
cargo build --release
# binary at target/release/deft
```

Requires `clang`/`clang++` and `git` on `PATH`, plus an archiver (`ar` on
Unix; `llvm-ar` or `lib.exe` on Windows) and a fetch tool (`curl`/`wget` on
Unix, PowerShell on Windows). Run `deft doctor` after building to verify your
environment end-to-end, including a real probe compile against `<stdio.h>`.
