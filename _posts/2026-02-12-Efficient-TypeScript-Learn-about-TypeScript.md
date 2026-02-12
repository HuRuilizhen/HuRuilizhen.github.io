---
layout: post
title: "Efficient TypeScript - Learn about TypeScript"
description: "Terms Regarding TypeScript Basic Best Practices"
date: 2026-02-12
feature_image: images/typescript.png
tags: ['typescript']
---

This article starts a reading-notes series on ***Efficient TypeScript***. Many developers learn TypeScript through scattered features — generics, utility types, strict flags — yet still misunderstand how the language actually works. Before discussing specific techniques, we will step back and examine the overall model of TypeScript: what it guarantees, what it does not, and why its type system behaves the way it does. Establishing this mental framework is essential for writing predictable and maintainable TypeScript code.

<!--more-->

## Table of Contents

<!--toc:start-->
- [Term 01: Understand the Relations between TypeScript and JavaScript](#term-01-understand-the-relations-between-typescript-and-javascript)
- [Term 02: Learn about TypeScript Options](#term-02-learn-about-typescript-options)
  - [Code with Incorrect Type will also Produce Output](#code-with-incorrect-type-will-also-produce-output)
  - [Unable to Check TypeScript Type at Runtime](#unable-to-check-typescript-type-at-runtime)
  - [Type Operations cannot Affect Runtime Values](#type-operations-cannot-affect-runtime-values)
  - [the Runtime Type may be Different from the Declared Type](#the-runtime-type-may-be-different-from-the-declared-type)
  - [cannot Overload a Function Based on a TypeScript Type](#cannot-overload-a-function-based-on-a-typescript-type)
  - [TypeScript Types Have No Impact on Runtime Performance](#typescript-types-have-no-impact-on-runtime-performance)
- [Term 03: Code Generation is Independent of the Type System](#term-03-code-generation-is-independent-of-the-type-system)
- [Term 04: Accustomed to Structural Typing](#term-04-accustomed-to-structural-typing)
- [Term 05: Restrict the Use of Any Type](#term-05-restrict-the-use-of-any-type)
  - [any Type do not Have Type Safety](#any-type-do-not-have-type-safety)
  - [any Type will Break Agreement](#any-type-will-break-agreement)
  - [any Type do not Have Language Service](#any-type-do-not-have-language-service)
  - [any Type will Cover up Errors during Code Refactoring](#any-type-will-cover-up-errors-during-code-refactoring)
  - [any Type will Undermine Confidence in Type Systems](#any-type-will-undermine-confidence-in-type-systems)
<!--toc:end-->

---

# Term 01: Understand the Relations between TypeScript and JavaScript

---

# Term 02: Learn about TypeScript Options

## Code with Incorrect Type will also Produce Output

## Unable to Check TypeScript Type at Runtime

## Type Operations cannot Affect Runtime Values

## the Runtime Type may be Different from the Declared Type

## cannot Overload a Function Based on a TypeScript Type

## TypeScript Types Have No Impact on Runtime Performance

---

# Term 03: Code Generation is Independent of the Type System

---

# Term 04: Accustomed to Structural Typing

---

# Term 05: Restrict the Use of Any Type

## any Type do not Have Type Safety

## any Type will Break Agreement

## any Type do not Have Language Service

## any Type will Cover up Errors during Code Refactoring

## any Type will Undermine Confidence in Type Systems

---
