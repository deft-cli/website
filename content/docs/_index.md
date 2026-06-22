---
title: "Documentation"
---

## Getting Started with Deft

This guide will help you set up and build your first C/C++ project using Deft in less than a minute. Before proceeding, ensure that `clang` is installed on your system and the `deft` binary is accessible in your system's `PATH`. You can verify your environment at any time by running `deft doctor`.

### 1. Initialize a New Project

Deft eliminates the need for complex, handwritten build scripts. To scaffold a new project, create an empty directory and initialize it with a single command:

```bash
mkdir my_project
cd my_project

# Initializes a standard C++ executable project by default
deft init

```

*To initialize a pure C project or a static library instead, use the explicit flags: `deft init --c` or `deft init --lib`.*

### 2. Project Layout Enforcements

Deft enforces a strict, predictable directory structure inspired by Cargo. This design eliminates configuration sprawl. Your initialized project will look like this:

```text
my_project/
├── src/
│   └── main.cpp  # The canonical entry point for a C++ executable
├── .gitignore    # Automatically generated to exclude the target/ directory
└── deft.toml     # The minimalist package manifest

```

The generated `deft.toml` contains basic configuration metadata and strict language profiles:

```toml
[package]
name = "my_project"
version = "0.2.0"

[profile.cpp]
standard = "c++20"
warnings = ["all", "extra"]
optimization = "0"

```

> **Strict Layout Rule:** Deft packages are single-language only. If the compilation engine detects a foreign source file (such as a `.c` file inside a C++ package) during `Layout::collect_sources`, the build will immediately abort with a `LayoutViolation` error.

### 3. Build the Project

To compile your application, run the build command from the project root:

```bash
deft build

```

Deft automatically discovers all translation units inside `src/`, schedules them across an optimized internal thread pool, and invokes the compiler driver. Thanks to the *Trusted Hot-Path* execution model, subsequent incremental builds take **under 0.02 seconds** because environment checking is completely decoupled from the build pipeline.

All compiled binaries and objects are isolated within the `target/` directory:

* **Unix-like systems:** `target/debug/my_project`
* **Windows systems:** `target/debug/my_project.exe`

To compile a production-ready binary with aggressive `-O3` optimizations, disabled debug symbols, and the `-DNDEBUG` macro active, pass the release flag:

```bash
deft build --release

```

### 4. Run the Executable

You can compile and immediately execute your project's binary using a single command:

```bash
deft run

```

If you need to pass arguments directly to your underlying compiled executable, use the standard double-dash `--` separator:

```bash
deft run -- --port 8080 --verbose

```

*All arguments following the `--` token are captured greedily and forwarded verbatim to the launched child process.*

### 5. Add Dependencies

Deft manages external dependencies securely without tracking heavy git submodules or vendoring raw header files. To declare a dependency, add its GitHub shorthand to the `[dependencies]` table inside your `deft.toml`:

```toml
[dependencies]
"gh:user/network_lib" = "1.5"

```

To pull the dependency and freeze its state, execute:

```bash
deft update

```

Deft invokes the host OS utilities atomically to clone the repository into the global cache directory (`~/.deft/cache/`), resolves the specific tag, and pins the exact commit SHA inside `deft.lock` to guarantee reproducible builds across different development environments. The dependency is automatically compiled as a static library (`.a` / `.lib`) and linked into your artifact during the next `deft build`.