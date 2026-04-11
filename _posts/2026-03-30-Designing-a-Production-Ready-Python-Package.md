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

## Package Organization

## tests

---

# Build System

## setup.py → pyproject

## Backends

## Recommendation

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
