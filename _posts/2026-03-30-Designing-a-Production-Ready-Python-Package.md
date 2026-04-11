---
layout: post
title: "Designing a Production-Ready Python Package: Structure, Tooling, and Automation"
description: "Configuration of Windows Terminal with PowerShell and related tools"
date: 2026-03-30
feature_image: https://pypi.org/static/images/logo-large.516e776d.svg 
tags: ['python', 'package', 'best-practices']
---

Python packaging has long been shaped by a fragmented ecosystem of loosely connected tools—linters, formatters, build systems, and release workflows—each solving a piece of the problem but rarely forming a coherent whole. Recent developments, particularly the rise of tools like Ruff and the standardization around `pyproject.toml`, are beginning to change this landscape. This article presents a practical, end-to-end approach to designing a production-ready Python package, covering project structure, modern tooling choices, and CI/CD automation, with an emphasis on clarity, consistency, and maintainability.

<!--more-->

## Table of Content

<!--toc:start-->
  - [Table of Content](#table-of-content)
- [Introduction](#introduction)
  - [The Problem](#the-problem)
  - [Goals](#goals)
- [The Evolution of Python Tooling](#the-evolution-of-python-tooling)
  - [Fragmented Era](#fragmented-era)
  - [Early Attempts at Standardization: Black](#early-attempts-at-standardization-black)
  - [Toward Unification: Ruff](#toward-unification-ruff)
- [Project Structure](#project-structure)
  - [src vs flat](#src-vs-flat)
  - [Package Organization](#package-organization)
  - [tests](#tests)
- [Build System](#build-system)
  - [setup.py → pyproject](#setuppy-pyproject)
  - [Backends](#backends)
  - [Recommendation](#recommendation)
- [Code Quality](#code-quality)
  - [Traditional Issues](#traditional-issues)
  - [Ruff replaces](#ruff-replaces)
  - [Integration](#integration)
  - [Ruff vs Black](#ruff-vs-black)
- [Testing](#testing)
  - [pytest](#pytest)
  - [Structure](#structure)
  - [CLI testing](#cli-testing)
- [Packaging & Publishing](#packaging-publishing)
  - [Artifacts](#artifacts)
  - [PyPI](#pypi)
  - [Versioning](#versioning)
  - [Pitfalls](#pitfalls)
- [CI/CD](#cicd)
  - [Why](#why)
  - [Pipeline](#pipeline)
  - [Release](#release)
  - [Security](#security)
- [Putting It All Together](#putting-it-all-together)
  - [Template](#template)
  - [Workflow](#workflow)
  - [Toolchain Summary](#toolchain-summary)
- [Conclusion](#conclusion)
  - [Takeaways](#takeaways)
  - [Future](#future)
<!--toc:end-->

---

# Introduction

## The Problem

Python package development has historically relied on a collection of loosely connected tools, each addressing a specific concern—linting, formatting, dependency management, building, and publishing. While this modularity provides flexibility, it often results in fragmented workflows, duplicated configuration, and inconsistent project conventions. Developers are left to manually assemble their own toolchains, making even simple tasks—such as maintaining code quality or releasing a package—more complex than necessary. The difficulty lies not in any single tool, but in the lack of a cohesive system that ties them together into a predictable and maintainable workflow.

## Goals

This article aims to present a modern, production-oriented approach to Python packaging by establishing a coherent workflow that spans the entire lifecycle of a package—from project structure and build configuration to code quality, testing, and automated releases. Rather than enumerating all available tools, the focus is on practical decisions and trade-offs, highlighting a streamlined toolchain centered around pyproject.toml and Ruff. The goal is to reduce complexity, improve consistency, and provide a clear, reproducible foundation for building and maintaining Python packages in real-world projects.

---

# The Evolution of Python Tooling

## Fragmented Era

Before the recent consolidation of Python tooling, package development typically involved assembling a collection of independent tools, each responsible for a narrow aspect of the workflow. Linting (e.g., [flake8](https://flake8.pycqa.org/en/latest/),  [pylint](https://pylint.readthedocs.io/en/stable/)), formatting (e.g., [Black](https://black.readthedocs.io/en/stable/)), import organization (e.g., [isort](https://here-be-pythons.readthedocs.io/en/latest/python/isort.html)), dependency management, and packaging (e.g., [setuptools](https://setuptools.pypa.io/en/latest/)) were handled by separate utilities, often with overlapping responsibilities and inconsistent conventions.

> Term LSP: The **L**anguage **S**erver **P**rotocol defines the protocol used between an editor or IDE and a language server that provides language features like auto complete, go to definition, find all references etc. For more infomation, visit: [https://microsoft.github.io/language-server-protocol/](https://microsoft.github.io/language-server-protocol/)

> Term Linter: A **linter** is a static code analysis tool used to provide diagnostics around programming errors, bugs, stylistic errors and suspicious constructs. Linters can be executed as a standalone program in a terminal, where it usually expects one or more input files to lint. 

This modular ecosystem provided flexibility, but at the cost of cohesion. Configuration was scattered across multiple files—such as setup.py (packaging), setup.cfg (tool configuration), tox.ini (testing / environment), and requirements.txt (dependencies) —making it difficult to maintain a clear and unified project setup. Tool interactions were not always predictable, and developers frequently had to resolve conflicts between formatting rules or manually coordinate execution order across tools.

As a result, the complexity of Python packaging did not stem from any individual tool, but from the need to compose many tools into a working system. The lack of a cohesive workflow meant that even routine tasks—such as enforcing code quality or preparing a release—required non-trivial effort and careful orchestration.

## Early Attempts at Standardization: Black

The introduction of [Black](https://black.readthedocs.io/en/stable/) marked an important step toward reducing this fragmentation by standardizing one aspect of the development process: code formatting. By adopting a strictly opinionated approach with minimal configuration, Black eliminated many of the debates and inconsistencies surrounding code style, allowing teams to converge on a single, deterministic format.

This shift demonstrated that developers were willing to trade configurability for consistency, especially when it simplified collaboration and reduced cognitive overhead. However, Black addressed only a single dimension of the broader problem. Linting, import management, packaging, and workflow orchestration remained distributed across separate tools, each with its own configuration and execution model.

In this sense, Black can be seen as an early attempt at standardization—one that proved the value of unification, but also highlighted the limitations of solving fragmentation at only one layer of the toolchain.

## Toward Unification: Ruff

More recent developments, particularly the emergence of [Ruff](https://docs.astral.sh/ruff/), signal a broader shift from tool composition toward tool consolidation. Rather than focusing on a single concern, Ruff integrates multiple responsibilities—such as linting, import sorting, and code modernization—into a single, high-performance tool built on a unified internal architecture.

By analyzing source code through a single parsing pipeline and applying a wide range of rules within the same execution context, Ruff significantly reduces both runtime overhead and configuration complexity. It also enables automatic fixes for many classes of issues, further streamlining the development workflow.

More importantly, Ruff changes the structure of the toolchain itself. Instead of coordinating multiple tools with overlapping responsibilities, developers can rely on a more centralized and consistent system. When combined with the standardization of project configuration through pyproject.toml, this shift enables a more cohesive and maintainable packaging workflow.

This evolution—from fragmented tools, to partial standardization, to broader unification—reflects a gradual movement toward simplicity at the system level, rather than improvements in isolated components.

---

# Project Structure

## src vs flat

A fundamental decision in structuring a Python package is whether to use a flat layout or a src-based layout. While both can work, they differ significantly in how imports are resolved during development and testing.

```bash
# flat layout package example
demo/
  foo/
    __init__.py
  run.py

# src-based layout package example
demo/
  src/
    foo/
      __init__.py
  run.py
```

In a flat layout, the package directory resides at the project root, making it directly discoverable by Python’s import system. Since the current working directory is included in `sys.path`, imports such as `import mypkg` succeed by resolving modules from the source tree itself. This behavior is convenient, but it can mask issues: code that works locally may fail after installation, as the import no longer relies on the source directory but on the installed package.

```bash
# import path example
❯ python3.11
Python 3.11.15 (main, Mar  3 2026, 00:52:57) [Clang 17.0.0 (clang-1700.6.3.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
[
  '', # current working directory, where you run python script
  '/opt/homebrew/Cellar/python@3.11/3.11.15/Frameworks/Python.framework/Versions/3.11/lib/python311.zip',
  '/opt/homebrew/Cellar/python@3.11/3.11.15/Frameworks/Python.framework/Versions/3.11/lib/python3.11',
  '/opt/homebrew/Cellar/python@3.11/3.11.15/Frameworks/Python.framework/Versions/3.11/lib/python3.11/lib-dynload',
  '/opt/homebrew/lib/python3.11/site-packages'
]
```

The `src/` layout addresses this by placing the package under a dedicated source directory (e.g., `src/mypkg`). In this setup, the project root is no longer sufficient for resolving imports, and the package must be installed (or explicitly added to the import path) before it can be used. This enforces a stricter and more realistic workflow, ensuring that imports behave consistently between local development and deployed environments.

Beyond consistency, the `src/` layout also helps prevent import shadowing, where modules in the project unintentionally override standard library or third-party packages due to their presence in the working directory. By separating the source tree from Python’s default import path, this class of subtle and environment-dependent bugs is effectively eliminated.

> Tips: Flat layouts test whether code can be imported from the source tree; `src/` layouts test whether it works as an installed package. For production-oriented packages, the `src/` layout provides stronger guarantees around correctness and reproducibility, and is therefore generally preferred over the flat layout.

## Package Organization

Once the project layout is established, the next concern is how to organize code within the package itself. A well-structured package should reflect logical boundaries in the system, rather than incidental implementation details.

At a minimum, modules should be grouped by functionality, with each subpackage representing a coherent responsibility. This helps maintain a clear separation of concerns and avoids the accumulation of large, monolithic modules that are difficult to reason about. Deep and overly nested hierarchies should also be avoided, as they tend to increase cognitive overhead without providing meaningful structure.

```bash
# anti-pattern example: layer-based / technical role-based
mypkg/
  models/
  services/
  utils/
  helpers/

# anti-pattern example: over-engineered / over-nested
mypkg/
  core/
    domain/
      entities/
      services/
    infrastructure/
      adapters/
      repositories/
```

A common anti-pattern is organizing code by technical roles rather than functional boundaries. For example, separating modules into `models/`, `services/`, and `utils/`. While this may appear structured, it often leads to fragmented logic, where a single feature is spread across multiple directories, increasing coupling and making changes harder to implement. Another frequent issue is the overuse of generic `utils` or `helpers` modules, which tend to accumulate unrelated functionality and become implicit dependency hubs. In contrast, organizing code by functionality keeps related components together, making the system easier to understand, maintain, and evolve.

> Tips: Functional organization keeps related logic together, reducing the need to navigate across multiple layers to understand or modify a feature.

Equally important is defining a stable public interface. The top-level package (typically via `__init__.py`) should expose only the components intended for external use, while internal modules remain encapsulated. This distinction between public and private APIs becomes especially valuable as the package evolves, allowing internal refactoring without breaking downstream users.

For projects that include command-line interfaces, it is often useful to isolate CLI-related code into a dedicated module (e.g., `cli/` or `__main__.py`), keeping it separate from core application logic. This separation ensures that the core package remains reusable as a library, independent of how it is invoked.

```bash
# functionality-based package structure example 
src/
  mypkg/
    __init__.py

    user/
      __init__.py
      models.py
      service.py

    auth/
      __init__.py
      service.py
      tokens.py

    cli/
      __init__.py
      main.py

    _internal/
      config.py
      logging.py
```

In the above structure, related components, such as models, services, and helpers—are colocated within the same feature directory, rather than being split across technical layers. This makes the codebase easier to navigate and reduces the need to traverse multiple modules to understand or modify a feature. Separating CLI logic into its own module further ensures that the core package remains reusable as a library, while internal utilities can be grouped under a clearly marked private namespace.

In practice, an effective package structure is one that makes the codebase easy to navigate, minimizes coupling between components, and provides a clear entry point for both users and maintainers.

## tests

Testing is an integral part of a production-ready package, and its structure should reinforce clarity, isolation, and ease of execution. In practice, pytest has become the de facto standard for Python testing due to its simplicity and flexibility, and is a natural choice for most projects.

A common and effective approach is to place all tests in a dedicated `tests/` directory at the project root, rather than embedding them within the package itself. This separation keeps production code and verification logic distinct, avoids unintentionally packaging test code during distribution, and makes the project layout easier to reason about. It also aligns well with pytest’s default discovery mechanisms, requiring minimal configuration.

Within the `tests/` directory, test modules are typically organized to mirror the package structure, though this does not need to be strictly enforced. A loose correspondence is often sufficient to maintain navigability without introducing unnecessary rigidity. The goal is to make it straightforward to locate the tests associated with a given feature, while preserving flexibility as the codebase evolves.

For projects that expose a command-line interface, it is important to treat the CLI as an external interface and test it accordingly. Rather than relying solely on internal function calls, tests should validate behavior through invocation mechanisms such as subprocess execution or CLI runners. This ensures that the interface behaves correctly from a user’s perspective, independent of its internal implementation.

Overall, a well-structured testing setup emphasizes isolation, discoverability, and alignment with real usage patterns, helping to ensure that the package behaves consistently across development and deployment environments.

---

# Build System

## setup.py → pyproject

Historically, Python packaging relied on `setup.py` as both a configuration file and an executable script. While flexible, this approach blurred the boundary between definition and execution, allowing arbitrary code to run during the build process. As a result, builds were often difficult to reason about, inconsistent across environments, and tightly coupled to specific tooling conventions.

The introduction of `pyproject.toml` marked a shift toward a more declarative and standardized packaging model. Instead of embedding logic in Python code, project metadata and build configuration are expressed in a structured format that can be interpreted uniformly by different tools. This separation enables a clearer contract between the project and the build system, reducing implicit behavior and improving reproducibility.

Equally important, `pyproject.toml` serves as a unified configuration entry point for the broader Python tooling ecosystem. Beyond packaging, tools for linting, formatting, and dependency management increasingly rely on the same file, reducing configuration sprawl and promoting consistency across the project.

With setup.py, defining a package requires executing Python code. With pyproject.toml, the package is described as data, allowing build tools to operate without arbitrary code execution. In effect, this transition reframes packaging from an execution-driven process to a configuration-driven one. By standardizing how projects declare their structure and requirements, `pyproject.toml` provides a more predictable and maintainable foundation for modern Python development.

> Tips: `setup.py` executes code to define a package; `pyproject.toml` describes a package without executing code.

## Backends

While `pyproject.toml` defines the structure and metadata of a project, it does not perform the build itself. This responsibility is delegated to a build backend, which implements the logic required to transform source code into distributable artifacts such as wheels or source distributions. Common backends include [setuptools](https://setuptools.pypa.io/en/latest/), [hatchling](https://github.com/pypa/hatch), and [poetry-core](https://github.com/python-poetry/poetry-core).

A build backend can be understood as the execution layer of the packaging system. It interprets the project definition provided in `pyproject.toml` and carries out the necessary steps to build the package. Different backends may offer varying levels of abstraction and additional features, but they all conform to a common interface, allowing tools like pip to interact with them in a standardized way.

This separation between definition and execution is a key aspect of modern Python packaging. By decoupling project configuration from build logic, the ecosystem allows developers to choose a backend that fits their needs without changing how the project is described. In other words, `pyproject.toml` specifies what the project is, while the build backend determines how it is built.

In practice, this model enables a more modular and extensible tooling landscape, where improvements in build systems can be adopted independently of project configuration, and where projects remain interoperable across different tools and environments.

## Recommendation

Choosing a build system is less about individual tools and more about understanding the trade-offs between different design approaches. In practice, a good build setup should prioritize simplicity, predictability, and compatibility with the broader Python ecosystem. These criteria help ensure that the project remains easy to maintain, behaves consistently across environments, and integrates smoothly with standard tooling.

From this perspective, build systems can be broadly divided into two categories. Minimal backends, such as setuptools or hatchling, focus on implementing the standard packaging interface with minimal abstraction. They align closely with the `pyproject.toml` specification, offer greater transparency, and reduce the risk of tool-specific lock-in. This makes them well-suited for libraries and projects that prioritize stability and long-term maintainability.

In contrast, integrated toolchains, such as poetry, provide a more opinionated and feature-rich experience by combining dependency management, packaging, and environment handling into a single workflow. While this can improve developer ergonomics, it also introduces additional abstraction and tighter coupling to the tool itself, which may increase complexity over time.

In most cases, a minimal, standard-aligned backend is the preferable choice. It keeps the configuration surface small, avoids unnecessary indirection, and adheres closely to the evolving Python packaging standards. 

> Tips: Integrated toolchains remain a reasonable option when their additional features are explicitly needed, but they should be adopted with an understanding of the trade-offs involved.

---

# Code Quality

## Traditional Issues

## Ruff replaces

## Integration

## Ruff vs Black

---

# Testing

## pytest

## Structure

## CLI testing

---

# Packaging & Publishing

## Artifacts

## PyPI

## Versioning

## Pitfalls

---

# CI/CD

## Why

## Pipeline

## Release

## Security

---

# Putting It All Together

## Template

## Workflow

## Toolchain Summary

---

# Conclusion

## Takeaways

## Future
