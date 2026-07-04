---
layout: post
title: "A Productive C++ Development Environment"
description: "A Modern C++ Development Setup for Productive Workflows"
date: 2026-07-01
feature_image: images/cpp.png
tags: ['cpp', 'development', 'package-release', 'best-practices']
---

In this post, we will explore how to build a productive C++ development environment around `CMake`, `clangd`, `lldb`, and `AstroNvim`. The focus here is not on collecting more tools, but on making multiple tools share one consistent understanding of the same project so that build, edit, and debug behavior remain predictable.

<!--more-->

## Table of Contents

<!--toc:start-->
  - [Table of Contents](#table-of-contents)
- [Why this Setup Exists](#why-this-setup-exists)
- [System Tool Chain Layer](#system-tool-chain-layer)
  - [Compiler](#compiler)
  - [Build System](#build-system)
  - [Generators](#generators)
  - [Compilation Metadata](#compilation-metadata)
  - [Dependency Strategy](#dependency-strategy)
  - [Build Type Model](#build-type-model)
- [Project Structure Contract](#project-structure-contract)
  - [Baseline Layout](#baseline-layout)
  - [Directory Semantics](#directory-semantics)
  - [Test Directory Organization](#test-directory-organization)
  - [Constraint Model](#constraint-model)
- [Debug Stack](#debug-stack)
  - [CLI Debugging](#cli-debugging)
  - [Sanitizers](#sanitizers)
  - [IDE Debugging](#ide-debugging)
- [Integrate with AstroNvim](#integrate-with-astronvim)
  - [clangd Integration](#clangd-integration)
  - [Debugger Frontend Integration](#debugger-frontend-integration)
  - [Community Pack and Lightweight Configuration](#community-pack-and-lightweight-configuration)
  - [What AstroNvim Actually Needs](#what-astronvim-actually-needs)
<!--toc:end-->

---

# Why this Setup Exists

The problem with modern C++ development environments is not a lack of tools, but a **lack of consistent contracts** between tools. A typical C++ project involves several subsystems that are only loosely coupled with one another: compiler ([`clang`](https://clang.llvm.org/), [`gcc`](https://gcc.gnu.org/), [`MSVC`](https://visualstudio.microsoft.com/vs/features/cplusplus/)), build system ([`CMake`](https://cmake.org/), [`Make`](https://www.gnu.org/software/make/), [`Ninja`](https://ninja-build.org/)), editor ([`Neovim`](https://neovim.io/), [`VSCode`](https://code.visualstudio.com/), [`CLion`](https://www.jetbrains.com/clion/)), language server ([`clangd`](https://clangd.llvm.org/)), debugger ([`lldb`](https://lldb.llvm.org/), [`gdb`](https://www.gnu.org/software/gdb/)), and static analysis tools such as [`clang-tidy`](https://clang.llvm.org/extra/clang-tidy/) or sanitizers. The friction does not come from any single tool being weak. It comes from the fact that these tools do not naturally share the same engineering understanding of the project.

The core contradiction is that each tool often maintains its own partial model of the same codebase. The build system knows the real include paths, compile flags, macro definitions, and target graph; the language server may not. Without `compile_commands.json`, `clangd` is forced to guess, so it is no longer truly understanding the project but only approximating it. This is why code may build correctly while diagnostics, jump-to-definition, or autocomplete still fail inside the editor.

The same inconsistency appears between IDE workflows and CLI workflows. A project may compile inside an IDE but fail from the terminal, or the reverse, because one side is silently adding include paths, using a different generator, or building a different configuration such as `Debug` versus `Release`. The debugger adds yet another execution layer: after optimization, inlining, and symbol generation, the running program no longer maps perfectly to the source view, which is why breakpoints move, variables disappear, and stack frames sometimes look misleading. In other words, the real problem is not missing tools but **fragmented tool state**.

The goal of this setup is therefore not convenience in the superficial sense. It is to make the environment reproducible, decoupled, and cognitively simple. Reproducible means that on any machine the workflow should remain `git clone -> configure -> build -> debug`. Decoupled means the editor does not define the build logic, the language server does not depend on hand-written local flags, and the debugger does not require one specific UI. Simple means the developer only needs to understand how to build, run, and debug, rather than how several tools secretly synchronize with one another.

To achieve that, this article treats `CMake` plus `compile_commands.json` as the **single source of truth**. The desired end state is a CLI-first workflow: you configure the project explicitly,

```bash
cmake --preset debug
```

build it,

```bash
cmake --build --preset debug-build
```

and debug the resulting binary directly,

```bash
lldb ./path/to/your/binary
```

while the editor remains only a frontend. In this model, `AstroNvim` reads `compile_commands.json`, invokes `clangd`, and attaches to a debug adapter when requested, but it does not participate in the project's build logic. Here `compile_commands.json` is not produced by the editor either; it is generated by `CMake` during the project configuration step and then consumed by other tools as shared build metadata. That separation is the main idea behind the entire setup:

> C++ development is not IDE-centric. It is build-system-centric.

An IDE can be a very good interface, but it should remain an interpreter of the build system, not the place where the build system is secretly redefined.

---

# System Tool Chain Layer

## Compiler

If the previous section explained the motivation, this section defines the **engineering contract**. Everything above the compiler and build system, including `clangd`, the editor, and the debugger, depends on this layer behaving predictably. The invariant is simple: **the build system is the only source of compilation truth**.

At the compiler layer, the primary compiler in this setup is `clang++`, while `gcc` remains a secondary choice for compatibility or distribution-specific environments. On `macOS`, the practical default is usually `clang++`, since it is the system compiler. On `Linux`, either `clang++` or `gcc` may be available depending on the distribution, but the workflow described in this post is written with `clang++` as the preferred path. The language baseline is `C++20`, and there is no implicit fallback to older standards. If a project requires `C++20`, then that requirement should be stated explicitly in the build configuration rather than silently degraded by local machine defaults.

Direct compiler invocation is not part of the normal workflow. It may still be useful for quick debugging or for isolating a small compilation issue, for example:

```bash
clang++ -std=c++20 -Wall -Wextra -O0 main.cpp
```

However, this kind of command should be treated as a temporary debugging tool rather than the standard way to build the project. More importantly, compiler flags should not be maintained manually across shell history, editor settings, and local notes. The ownership rule here is strict: compile flags belong to the build system, and in this setup that means `CMake`.

## Build System

The build system is therefore the real center of the environment. This post uses [`CMake`](https://cmake.org/) as the single source of truth, with [`Ninja`](https://ninja-build.org/) as the preferred generator and `Make` kept only as a legacy fallback. The standard workflow is:

```bash
cmake --preset debug
cmake --build --preset debug-build
```

An out-of-source build is required. Build artifacts, cache files, generated metadata, and configuration state should live in the build directory rather than being mixed into the source tree. This rule should hold regardless of project size. Even for a small executable, keeping source and build state separate makes the workflow easier to reason about, easier to clean, and easier to connect to tools that expect a stable build directory.

`CMake` is not chosen here merely because it is popular. It is required because this setup needs a cross-platform abstraction layer, a reliable way to export compilation metadata for `clangd`, and compatibility with the surrounding editor and tooling ecosystem. Once `CMake` becomes the authority, the compiler stops being configured manually and starts executing instructions that come from one declarative source.

For a modern CMake-based project, the recommended mode is **preset-driven configuration**. This model can be understood as two layers. The first is the configuration layer, which is file-driven and described by [`CMakePresets.json`](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html). That file declares the source directory, build directory, generator, and cache variables for each named configuration. The second is the execution layer, where the command line becomes the only operational entrypoint through the workflow shown above.

This separation is important because it prevents configuration details from being scattered across shell history, editor tasks, and ad hoc scripts. Once a preset is defined, the CLI simply selects and executes it.

`CMakePresets.json` should live at the project root and be treated as versioned project configuration rather than personal machine state. In practice, that means the project can define not only configure presets, but also build presets, test presets, and workflow presets. The configure preset captures how the build tree is created, the build preset captures how the generated build tree should be built, the test preset captures how `ctest` should be run, and the workflow preset can chain multiple stages together. CMake exposes these layers directly through the CLI, for example [`cmake --build [<dir>] --preset <preset>`](https://cmake.org/cmake/help/latest/manual/cmake.1.html) for builds, [`ctest --preset <preset>`](https://cmake.org/cmake/help/latest/manual/ctest.1.html) for tests, and [`cmake --workflow --preset <preset>`](https://cmake.org/cmake/help/latest/manual/cmake.1.html) for running a configured sequence of stages.

This workflow support matters because it lets the project define not only static configuration, but also expected execution order. Instead of teaching every developer which exact configure, build, and test commands should be run in sequence, the project can encode that behavior once and expose it as a named workflow. In practice, that usually means a workflow such as `debug-full` can configure, build, and test the same build tree consistently from one command.

```bash
cmake --workflow --preset debug-full
```

For test presets specifically, the project still needs to define tests in CMake itself, typically through `enable_testing()` and `add_test(...)`. Presets standardize how those tests are executed; they do not create test definitions automatically.

The older pattern

```bash
cmake -S . -B build -G Ninja
```

is still valid CMake, but in this setup it should be treated as a legacy mode. It is acceptable for temporary experiments, CI scripts, or environments where presets are not available, but it should not be the primary developer workflow inside the project. If the project already knows its generator, build directory layout, and cache variables, those decisions should live in `CMakePresets.json` rather than being retyped manually.

A minimal preset file may look like this:

```json
{
  "version": 6,
  "cmakeMinimumRequired": {
    "major": 3,
    "minor": 25,
    "patch": 0
  },
  "configurePresets": [
    {
      "name": "debug",
      "displayName": "Debug",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/debug",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug",
        "CMAKE_CXX_STANDARD": "20",
        "CMAKE_EXPORT_COMPILE_COMMANDS": "ON"
      }
    },
    {
      "name": "release",
      "displayName": "Release",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/release",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Release",
        "CMAKE_CXX_STANDARD": "20",
        "CMAKE_EXPORT_COMPILE_COMMANDS": "ON"
      }
    }
  ],
  "buildPresets": [
    {
      "name": "debug-build",
      "configurePreset": "debug",
      "jobs": 8
    },
    {
      "name": "release-build",
      "configurePreset": "release",
      "jobs": 8
    }
  ],
  "testPresets": [
    {
      "name": "debug-test",
      "configurePreset": "debug",
      "output": {
        "outputOnFailure": true
      },
      "execution": {
        "noTestsAction": "error",
        "stopOnFailure": true
      }
    },
    {
      "name": "release-test",
      "configurePreset": "release",
      "output": {
        "outputOnFailure": true
      }
    }
  ],
  "workflowPresets": [
    {
      "name": "debug-full",
      "steps": [
        {
          "type": "configure",
          "name": "debug"
        },
        {
          "type": "build",
          "name": "debug-build"
        },
        {
          "type": "test",
          "name": "debug-test"
        }
      ]
    }
  ]
}
```

The point of this file is not verbosity for its own sake. It turns configuration into versioned project state. Instead of every developer deciding locally where the build directory lives or whether compile commands should be exported, the project records those choices once and makes them reproducible.

One useful way to think about presets is that they primarily handle **selection and orchestration**, not deep compilation policy. A preset chooses which build tree, generator, cache values, and workflow stages should be used. Lower-level compilation behavior, such as sanitizer instrumentation, is often cleaner when it is expressed through project options, target-level logic, or a dedicated toolchain file rather than by turning every preset into a bag of raw flags.

## Generators

It is worth separating the concept of a **build system** from the concept of a **generator**, because they are related but not identical. `CMake` is a meta-build system: it reads `CMakeLists.txt` and preset configuration, resolves targets and cache variables, and then emits files for a concrete backend. That backend is called a generator. Officially, CMake supports command-line generators such as `Ninja` and Unix Makefiles, as well as IDE-oriented generators such as Visual Studio and Xcode project files, all documented in the [`cmake-generators(7)` manual](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html).

The practical relationship is straightforward. The build system defines the project model, while the generator determines which concrete tool will execute that model. If the generator is `Ninja`, CMake emits `build.ninja` and related files. If the generator is Unix Makefiles, CMake emits `Makefile`-based build scripts. The project semantics should remain the same, but the execution backend changes.

In this article, `Ninja` is preferred because it is fast, explicit, and fits well with a CLI-first workflow. `Make` remains a fallback for older or more constrained environments, but it is not the primary recommendation. The key point is that developers should not think of the generator as a casual local preference. It is part of the configured build contract and therefore belongs in presets or other versioned build configuration.

## Compilation Metadata

The next part of the contract is compilation metadata. The key artifact is `compile_commands.json`, which must be generated explicitly:

```bash
cmake --preset debug
```

assuming that the preset enables `CMAKE_EXPORT_COMPILE_COMMANDS=ON`. By default, `compile_commands.json` is emitted into the corresponding build directory rather than the source root. In the preset example above, that means it would appear at `build/debug/compile_commands.json` or `build/release/compile_commands.json`. This matters because many editor integrations expect the file to exist either in the active build directory or to be symlinked from the project root. The important rule is that the file belongs to build state, not source state.

This file is not a convenience feature for one editor plugin. It is the machine-readable description of how each translation unit is compiled. Each entry records the source file, include paths, compiler flags, macro definitions, and language standard associated with a concrete compile command. For `clangd`, this database is foundational because `clangd` operates by reading compilation metadata and constructing an AST from it. It does not reliably infer the real build configuration from the source tree alone.

That is why missing or stale compilation metadata produces very specific failures: incorrect header resolution, broken go-to-definition, false diagnostics, or symbols that appear to vanish in the editor even though the project still builds. In many teams this is misdiagnosed as an editor problem, but in practice it usually means the compilation database is absent, outdated, or inconsistent with the actual build directory.

## Dependency Strategy

Dependency strategy is intentionally kept conservative in the baseline setup. No global dependency manager is required. System package managers such as `brew` or `apt` are acceptable for installing the toolchain itself. `vcpkg` and `Conan` are both valid options when a project needs stronger dependency management, but they are treated as optional extensions rather than part of the core environment. `vcpkg` brings a relatively heavy integration footprint, while `Conan` is better suited to full dependency graph management.

The current decision is to exclude dependency management from the baseline configuration on purpose. The reason is not that these tools are bad, but that the minimal setup should reduce external state and keep reproducibility centered on the build system itself. Regardless of which package strategy a real project eventually adopts, dependencies should not alter the semantics of the build system, silently rewrite compiler invocation rules, or break LSP correctness.

## Build Type Model

Finally, the build type model must also be explicit. A `Debug` configuration should keep optimization disabled, symbols enabled, and optionally allow sanitizers such as `ASAN` or `UBSAN`. A `Release` configuration should enable optimization such as `O2` or `O3`, minimize or strip symbols when appropriate, and typically disable runtime assertions. These two modes should live in distinct build directories rather than being switched in place. In other words, `Debug` and `Release` are not just flags; they are separate build states with different semantics for compilation, execution, and debugging.

A practical baseline looks like this:

| Build Type | Optimization | Debug Symbols | Sanitizers | Assertions | Build Directory |
| ---------- | ------------ | ------------- | ---------- | ---------- | --------------- |
| `Debug`    | `O0`         | enabled       | optional   | enabled    | `build/debug`   |
| `Release`  | `O2` / `O3`  | minimal       | disabled   | typically disabled | `build/release` |

Taken together, this layer defines a strict division of responsibilities: the build system defines compilation truth, the compiler executes that truth, tooling consumes the exported metadata, and the IDE does not define any build semantics on its own.

---

# Project Structure Contract

For a C++ project built around `CMake`, `clangd`, and `CTest`, directory layout is not just an aesthetic choice. It affects how build metadata is generated, how headers are interpreted, how tests are registered, and how debugging artifacts remain separated from source state. The principle is simple: **filesystem layout should follow toolchain assumptions, not personal preference**.

This matters because the build system, language server, debugger, and test runner all touch the same project tree, but they do not interpret every directory in the same way. `CMake` needs a stable source root and predictable generated directories. `clangd` relies on build metadata that points back into source and header trees. The debugger expects binaries and symbols to come from a known build output. `CTest` needs test registration to be explicit rather than inferred from ad hoc file placement. A layout that looks tidy to one developer but violates these assumptions usually creates friction later.

## Baseline Layout

For this setup, a practical baseline looks like this:

```text
project/
├── CMakeLists.txt
├── CMakePresets.json
├── src/
├── include/
├── tests/
├── cmake/
└── build/
```

This structure is intentionally modest. It is large enough to separate public headers, implementation files, helper CMake modules, tests, and generated artifacts, but small enough that the project does not become over-architected before it has real complexity.

## Directory Semantics

Before going further, it is useful to define what each directory is allowed to mean.

The `src/` directory holds implementation files, typically `.cpp` translation units and internal implementation headers when needed. It is not the place where the project's external contract is defined. In other words, `src/` describes how the software is implemented, not what it promises to other targets.

The `include/` directory holds public interface headers. These headers define the external contract of the project and should not depend on private build tree artifacts or fragile internal layout assumptions. If a header under `include/` only works because some local build detail leaks into it, then the boundary between public API and implementation has already become unclear. In practice, many projects go one step further and place public headers under a namespaced path such as `include/<project-name>/...` to reduce header name collisions and make the installation boundary more explicit.

The `tests/` directory holds test code. Test sources should live there rather than being mixed into production directories, and they should be added to the build graph explicitly through `CMake`. Just as importantly, test layout should not distort the production build graph. Tests exist to validate production code, not to redefine how that code is organized.

The `build/` directory is fully generated state. It should never be committed, and it should never contain authoritative source code. Build outputs, cache files, generated metadata such as `compile_commands.json`, object files, binaries, and test artifacts belong there precisely because they are derived state rather than maintained source state. The same rule applies to generated headers: if the build system creates headers during configuration or build, those files belong to the build tree rather than being written back into `include/` or `src/`.

The `cmake/` directory is optional but useful once the project grows beyond a single `CMakeLists.txt`. It is the right place for helper modules, reusable configuration fragments, and toolchain abstractions that should not clutter the project root. What it should not become is a second source tree with unclear ownership.

## Test Directory Organization

One part of project structure deserves separate discussion: test layout. In practice, C++ projects do **not** have one universal standard for how the `tests/` directory should be organized. Several patterns are common, and each solves a different scaling problem.

The first pattern is a flat structure:

```text
tests/
├── test_a.cpp
└── test_b.cpp
```

This is the simplest model. Each file may become its own test target, or several files may be grouped into a single test binary. It works well for small utilities, compact libraries, and CLI-oriented projects where the test suite is still narrow enough that extra hierarchy would add more noise than value.

The second pattern is a category-based structure:

```text
tests/
├── unit/
└── integration/
```

This structure separates tests by intent rather than by source file location. `unit/` usually contains isolated logic tests with minimal external dependency, while `integration/` contains broader tests that cross module boundaries, perform I/O, or rely on heavier runtime setup. This model aligns naturally with CI pipelines because unit and integration stages often have different runtime and failure expectations.

The third pattern is a source-mirrored structure:

```text
src/
├── engine/
└── net/

tests/
├── engine/
└── net/
```

Here the test tree mirrors the production tree. This is common in large or long-lived systems because it creates clearer ownership boundaries and makes it easier to map tests back to the corresponding production modules. It is especially useful when teams are organized around module boundaries rather than around one shared code pool.

These patterns can be summarized briefly:

| Pattern | Shape | Strength | Typical Usage |
| ------- | ----- | -------- | ------------- |
| Flat | `tests/test_x.cpp` | minimal overhead | small tools, small libraries |
| Category-based | `tests/unit`, `tests/integration` | explicit test intent and CI alignment | modular applications, actively maintained repos |
| Source-mirrored | `tests/<module>` matching `src/<module>` | ownership and traceability | large teams, long-lived systems |

An important engineering clarification is that **directory structure does not define test execution behavior**. The filesystem can help humans organize test code, but it does not decide how tests are built, grouped, filtered, or scheduled.

Execution semantics come from the build and test layers instead. `CMake` defines test targets and registration through mechanisms such as `add_test(...)`. `CTest` classifies and filters tests through names, labels, and preset configuration. CI systems then decide which subsets of those registered tests run in which pipeline stage. This is why two projects can have almost identical `tests/` directories but completely different execution behavior.

That distinction is worth making explicit because developers often overestimate what a folder name means. A directory named `integration/` may suggest slower or broader tests, but that meaning only becomes operational when the build system and test runner encode it. The folder alone is only an organizational signal.

For this article, the recommended default is the **category-based** structure:

```text
tests/
├── unit/
└── integration/
```

This is not because it is the only correct layout, but because it gives a good balance between scalability and simplicity. It scales better than a flat directory once the test suite grows, it avoids coupling test structure too tightly to the internal `src/` layout, and it maps naturally to CI-level separation when unit and integration tests need different execution policies. At the same time, it stays simple enough for local iteration and does not force a large-system ownership model onto a small or medium project.

## Constraint Model

Regardless of which test layout a project chooses, some constraints should remain invariant.

Tests should be independent of the production include layout in the sense that they must not depend on accidental header placement or private build-directory leakage. Tests must be registered through the build system rather than discovered through ad hoc filesystem conventions alone. And tests should not rely on directory shape itself as the source of execution semantics. Layout helps organization; `CMake` and `CTest` define behavior.

Taken together, this gives a clean separation of concerns:

- filesystem layout is the organizational layer
- `CMake` is the build graph definition layer
- `CTest` is the execution classification layer

These layers are intentionally decoupled. That decoupling leads to the principle that matters most for day-to-day engineering:

> Test directory structure is an organizational convention; execution semantics are defined by CMake and CTest, not the filesystem.

---

# Debug Stack

## CLI Debugging

In practice, the debugging stack in this setup has three layers: the command-line debugger, runtime instrumentation, and the editor or IDE frontend. They are related, but they are not interchangeable. The command-line debugger controls program execution, sanitizers instrument runtime behavior, and the IDE only orchestrates those lower layers through a user interface.

At the CLI layer, the primary debugger depends on platform conventions. On `macOS`, the practical default is `lldb`. On `Linux`, `gdb` remains the most common baseline, although `lldb` is also a viable option in many environments. The important point is not which debugger binary is more fashionable, but that the debugger operates directly on the compiled program and its symbols rather than on source files alone.

Before going further, the most important requirement is that debugging needs a real debug build. At minimum, that means debug symbols must be enabled and aggressive optimization should be avoided. In a CMake-driven workflow, this typically means using a `Debug` preset or setting:

```bash
-DCMAKE_BUILD_TYPE=Debug
```

At the raw compiler level, the equivalent intent is usually expressed with flags such as:

```bash
-g -O0
```

Without symbols, backtraces lose fidelity and variable inspection becomes unreliable. With too much optimization, the debugger may still run, but source-level stepping, local variable visibility, and breakpoint behavior become much harder to interpret.

A typical `lldb` workflow looks like this:

```bash
lldb ./build/debug/app
```

From there, only a small core command set is needed for day-to-day work:

| Command | Purpose |
| ------- | ------- |
| `b main` | set a breakpoint |
| `run` | start execution |
| `bt` | show a stack trace |
| `frame variable` | inspect local variables |
| `next` | step over |
| `step` | step into |
| `continue` | resume execution |

The key constraint is that the CLI debugger operates on the **compiled binary plus debug symbols**. It does not depend on IDE state, editor plugins, or active CMake runtime context after the build has already happened. In that sense, the debugger is downstream of the build system, not embedded inside it. However, debugger correctness still depends heavily on build-system decisions: symbols must actually be generated, `Debug` builds must avoid stripping them, and the build directory used for debugging must be the same one that produced the binary being inspected.

## Sanitizers

The second layer is runtime instrumentation. Sanitizers are not traditional debuggers; they are compiler-assisted runtime checks that modify the generated binary so that invalid behavior is detected at execution time. Their purpose is to catch classes of bugs that can be difficult to inspect manually in a debugger, especially memory misuse and undefined behavior.

Two sanitizers are especially common in modern C++ workflows. `AddressSanitizer` (`ASAN`) is used to detect heap and stack memory errors such as use-after-free, out-of-bounds access, and similar corruption patterns. `UndefinedBehaviorSanitizer` (`UBSAN`) is used to detect invalid or undefined language semantics such as problematic integer overflow, invalid casts, or alignment violations. In practice, these two are often enabled together:

```bash
-fsanitize=address,undefined
```

There are several ways to express this in a CMake-based workflow. The most direct method is to inject the sanitizer flags through preset cache variables such as `CMAKE_C_FLAGS`, `CMAKE_CXX_FLAGS`, and `CMAKE_EXE_LINKER_FLAGS` in a preset like `debug-asan`. This works, and it is useful as an introductory setup because the preset becomes self-contained and no `CMakeLists.txt` changes are required. The downside is that raw flags become scattered through cache state, are harder to compose cleanly, and can end up polluting broader build configuration.

A more project-oriented approach is to expose an explicit option such as `ENABLE_ASAN` in `CMakeLists.txt`, and then let a preset turn that option on. That model is closer to common engineering practice because the project declares sanitizer support intentionally rather than smuggling it in through ad hoc cache strings. It also composes better with multiple build profiles.

A minimal example looks like this:

```cmake
option(ENABLE_ASAN "Enable AddressSanitizer" OFF)

if(ENABLE_ASAN)
  add_compile_options(-fsanitize=address,undefined)
  add_link_options(-fsanitize=address,undefined)
endif()
```

And the preset only needs to activate the project-level option:

```json
{
  "name": "debug-asan",
  "inherits": "debug",
  "cacheVariables": {
    "ENABLE_ASAN": "ON"
  }
}
```

Another structured approach is to isolate sanitizer behavior in a dedicated file such as `cmake/toolchains/asan.cmake`, and let the preset select it. This can work well in a reusable template because the preset stays focused on configuration selection while the extra file holds the compilation policy. In that model, responsibilities are separated as follows:

- the preset selects configuration
- the toolchain defines compilation behavior
- `CMakeLists.txt` defines the build graph

That can be expressed with a very small dedicated file such as `cmake/toolchains/asan.cmake`:

```cmake
# Minimal example: real projects may also need to cover shared/module linker flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fsanitize=address,undefined")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address,undefined")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address,undefined")
```

and a preset that selects it:

```json
{
  "name": "debug-asan",
  "generator": "Ninja",
  "binaryDir": "${sourceDir}/build/debug-asan",
  "toolchainFile": "cmake/toolchains/asan.cmake",
  "cacheVariables": {
    "CMAKE_BUILD_TYPE": "Debug"
  }
}
```

For a reusable project template or a blog post that is trying to teach a disciplined setup, this model can be a useful advanced option. It scales naturally when additional variants such as `UBSAN`, `TSAN`, or `LSAN` need to be introduced later, and it keeps the preset readable instead of turning it into a raw flag container. However, for many ordinary projects, a project-level option plus target-based compile and link settings is still the more common default.

Sanitizers work best when debug symbols are enabled and optimization is kept low, usually at `O0` or `O1`. They can still be used in other configurations, but highly optimized binaries are generally less pleasant to diagnose because the resulting runtime and stack traces become harder to interpret.

Their execution model is also different from normal observation-based debugging. When a sanitizer detects a violation, the program typically aborts immediately and prints a diagnostic stack trace. This is not a recovery-oriented workflow. The binary is being instrumented so that illegal behavior becomes explicit as soon as it occurs.

That leads to the main constraint of this layer: sanitizers are **not observational tools**. They change program behavior by injecting checks into the compiled binary. That is exactly why they are effective, but it is also why they should be understood as a distinct debugging layer rather than as a prettier form of logging or tracing.

The most useful summary is therefore this: sanitizers should be treated primarily as a **toolchain-level concern**, not merely as a build preset concern. Presets select and orchestrate configurations; toolchains and project logic define how the binary is actually built.

## IDE Debugging

The third layer is IDE integration. In this setup, IDE debugging should be understood as a frontend for the CLI debugger, not as an independent execution system. Whether the interface is `nvim-dap`, a graphical IDE, or another DAP-compatible frontend, the underlying execution still flows through a real debugger such as `lldb` or `gdb`.

For a Neovim-based workflow, the typical components are `nvim-dap`, a debug adapter such as `lldb-vscode`, and the Debug Adapter Protocol itself. The execution chain can be summarized as:

<div style="text-align: center;" class="mermaid">
flowchart LR
    A[IDE / Editor Frontend]
    B[DAP]
    C[lldb / gdb]
    D[Binary]

    A --> B --> C --> D
</div>

This separation is important because it keeps responsibility boundaries clear. The IDE does not compile the code, does not define build flags, and does not replace the debugger. It only launches and orchestrates the debugger with a structured configuration.

In practice, the debugger frontend still needs a few concrete facts: the executable path, the working directory, and consistent source paths that match the debug symbols embedded in the binary. Editor-side project metadata such as `compile_commands.json` can help the surrounding development experience feel coherent, but actual source-level debug mapping depends primarily on the binary, its symbols, and path consistency between build and source trees.

---

# Integrate with AstroNvim

At this point, integrating the project with `AstroNvim` should not require redefining the toolchain. If the project already exports `compile_commands.json`, separates build directories cleanly, and exposes a predictable debug binary, then the editor only needs to consume that information. This is the reason I describe AstroNvim here as a frontend rather than as a build environment: it becomes useful only after the build system has already expressed the project correctly.

## clangd Integration

For C++ editing, the most important integration path is still `clangd`. AstroNvim does not need to understand `CMake` targets directly if `clangd` can already see the correct compilation database. In the [`async_logger`](https://github.com/HuRuilizhen/async_logger) example project, that contract is made explicit in two places.

First, the project exports compile commands through `CMakePresets.json`. Second, the project includes a local `.clangd` file that points `clangd` to the active build directory:

```yaml
CompileFlags:
  CompilationDatabase: build/debug
```

This small file matters because it removes ambiguity about which build tree the editor should treat as authoritative. In a project with multiple build directories, `clangd` should not be left to guess. The editor experience becomes much more stable when the project tells `clangd` exactly where the relevant compilation database lives.

Once that is in place, AstroNvim can delegate normal code intelligence work to `clangd`: go-to-definition, completion, diagnostics, symbol navigation, and refactoring assistance. None of that requires the editor to define include paths or compile flags manually. The editor is only surfacing the model already produced by the build system.

## Debugger Frontend Integration

The same thin-frontend principle applies to debugging. AstroNvim does not create a separate debugging model for the project. It simply provides a UI over the debugger stack described in the previous section. As long as the project already has a valid debug build and a predictable executable path, the editor can launch the debugger without redefining anything fundamental.

In the `async_logger` project, the build system already defines runtime output under `${CMAKE_BINARY_DIR}/bin`, and the presets define a deterministic build root such as `build/debug`. That means the editor-side debug configuration can stay simple: it only needs to know which binary to launch, which working directory to use, and whether stop-on-entry is desired. This is exactly the kind of integration that stays maintainable over time because the editor is consuming project state, not inventing it.

## Community Pack and Lightweight Configuration

One useful detail from the current setup is that the AstroNvim side is intentionally light. The active Neovim community imports include the C++ pack directly. In my current configuration, this lives in [`lua/community.lua`](https://github.com/HuRuilizhen/astronvim-configuration/blob/main/lua/community.lua), and the imported community modules come from the [AstroCommunity catalog](https://astronvim.github.io/astrocommunity/):

```lua
-- lua/community.lua
return {
  "AstroNvim/astrocommunity",
  { import = "astrocommunity.pack.lua" },
  { import = "astrocommunity.pack.cpp" }
}
```

That is enough to bring in a reasonable baseline for the language ecosystem without turning the editor configuration into the place where project semantics are defined. In other words, the community pack helps with editor capability, but it does not replace the role of `CMake`, `clangd`, or the debugger itself.

This distinction is worth preserving. Tool installation and editor conveniences belong to the editor layer. Compilation truth, test registration, and debug correctness belong to the project layer. The more these responsibilities remain separated, the easier the setup is to reason about and reproduce.

## What AstroNvim Actually Needs

Looking at the `async_logger` project, the editor integration works well because the project already satisfies a short list of requirements:

- a valid `CMakePresets.json`
- an exported `compile_commands.json`
- a `.clangd` file that points to the intended compilation database
- a deterministic build directory such as `build/debug`
- a predictable binary output path for debugging

After these conditions are met, AstroNvim has very little left to invent. It can attach `clangd`, surface diagnostics and navigation, and act as a DAP frontend for the debugger. That is the real integration story here: a good AstroNvim workflow for C++ is mostly a good CMake project with a thin editor frontend.

---

Related Posts / Websites 👇

📑 [GitHub - HuRuilizhen/async_logger](https://github.com/HuRuilizhen/async_logger)

📑 [GitHub - HuRuilizhen/astronvim-configuration](https://github.com/HuRuilizhen/astronvim-configuration)

📑 [AstroNvim - AstroCommunity](https://astronvim.github.io/astrocommunity/)

📑 [GitHub - AstroCommunity C++ Pack](https://github.com/AstroNvim/astrocommunity/tree/main/lua/astrocommunity/pack/cpp)

📑 [clangd - Configuration](https://clangd.llvm.org/config)

📑 [CMake - cmake-presets(7)](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html)

📑 [CMake - cmake-generators(7)](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html)

📑 [CMake - ctest(1)](https://cmake.org/cmake/help/latest/manual/ctest.1.html)

📑 [LLVM - LLDB Documentation](https://lldb.llvm.org/)

📑 [GitHub - mfussenegger/nvim-dap](https://github.com/mfussenegger/nvim-dap)
