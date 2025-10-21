---
layout: post
title: "Windows Terminal Configuration"
description: "Configuration of Windows Terminal with PowerShell and related tools"
date: 2025-10-16
feature_image: images/windows-terminal-configuration.png
tags: ['windows', 'command-line-interface']
---

The most exciting part after getting a new machine is configuration! In this post, I will share my configuration of Windows Terminal with PowerShell, focusing on the appearance settings, package management tools, command-line tools, and editor settings. I will walk you through the process of setting up a beautiful and functional terminal experience on Windows, which will greatly enhance your productivity and coding experience. 

<!--more-->

## Table of Contents
- [Installation and Startup](#installation-and-startup)
  - [Intalling Windows Terminal and PowerShell 7.x](#intalling-windows-terminal-and-powershell-7x)
  - [Windows Terminal Startup](#windows-terminal-startup)
  - [Basic PowerShell Instructions](#basic-powershell-instructions)
  - [Basic PowerShell Profile Setup](#basic-powershell-profile-setup)
- [Package Management Tools](#package-management-tools)
  - [winget](#winget)
  - [chocolatey](#chocolatey)
- [Appearance Settings](#appearance-settings)
  - [font](#font)
  - [posh](#posh)
  - [terminal icons](#terminal-icons)
- [Command-Line Tools](#command-line-tools)
  - [tldr - Community-driven man](#tldr---community-driven-man)
  - [bat - Better cat](#bat---better-cat)
  - [winfetch - System Info](#winfetch---system-info)
  - [dust - Quick du](#dust---quick-du)
  - [bottom - Better top](#bottom---better-top)
  - [fzf - Fuzzy finder](#fzf---fuzzy-finder)
  - [zoxide - Smart cd](#zoxide---smart-cd)
- [Development Settings](#development-settings)
  - [lazygit - TUI for git commands](#lazygit---tui-for-git-commands)
  - [lazydocker - TUI for docker commands](#lazydocker---tui-for-docker-commands)
  - [astronvim - Distr of Neovim](#astronvim---distr-of-neovim)
- [Conclusion](#conclusion)
---

# Installation and Startup

## Intalling Windows Terminal and PowerShell 7.x

To get started, ensure you have Windows Terminal. If not, you can download it from the [Microsoft Terminal Install Page](https://learn.microsoft.com/en-us/windows/terminal/install). The default version of PowerShell that comes with Windows is PowerShell 5.1, but I recommend installing PowerShell 7.x for a better experience. For example, PowerShell 7.x provides access to the `Get-PSSubsystem` cmdlet, which is not available in PowerShell 5.1. The most useful function for me is `PSReadLine`, which can give you completion suggestions as you type (based on your history).

Please refer to the [Installing PowerShell 7 on Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5) guide for installation instructions. Usually, I install PowerShell via [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/) using the command:

```powershell
winget install --id Microsoft.Powershell --source winget
```

One thing to note is that the PROFILE variable in PowerShell 7.x points to a different location than in PowerShell 5.1. You can check your profile path by running:

```powershell
$PROFILE
```

Just for reference, you can check current PowerShell Modules and PSSusbsytems with the following commands. And here are my current outputs:

<a name="powershell-modules-and-pssubsystems"></a>
```powershell
# PowerShell Modules
> Get-Module

ModuleType Version    PreRelease Name                                ExportedCommands
---------- -------    ---------- ----                                ----------------
Manifest   7.0.0.0               Microsoft.PowerShell.Management     {Add-Content, Clear-Content, Clear-Item, Clear-ItemProperty…}
Manifest   7.0.0.0               Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object…}
Script     0.0                   oh-my-posh-core                     {Enable-PoshLineError, Enable-PoshTooltips, Enable-PoshTransientPrompt, Set-PoshContext…}
Script     2.3.6                 PSReadLine                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PSReadLineKeyHandler, Set-PSReadLineKeyHandler…}
Script     0.11.0                Terminal-Icons                      {Add-TerminalIconsColorTheme, Add-TerminalIconsIconTheme, Format-TerminalIcons, Get-TerminalIconsColorTheme…}

# PSSusbsytems
> Get-PSSubsystem

Kind              SubsystemType      IsRegistered Implementations
----              -------------      ------------ ---------------
CommandPredictor  ICommandPredictor         False {}
CrossPlatformDsc  ICrossPlatformDsc         False {}
FeedbackProvider  IFeedbackProvider          True {General Feedback}
```

We will discuss some of these modules later in the post. Some modules may require additional installation steps, but they provide great functionality.

## Windows Terminal Startup

## Basic PowerShell Instructions

## Basic PowerShell Profile Setup

---

# Package Management Tools

## winget

## chocolatey

---

# Appearance Settings

## font

## posh

## terminal icons

---

# Command-Line Tools

## tldr - Community-driven man

## bat - Better cat

## winfetch - System Info

## dust - Quick du

## bottom - Better top

## fzf - Fuzzy finder

## zoxide - Smart cd

---

# Development Settings

## lazygit - TUI for git commands

## lazydocker - TUI for docker commands

## astronvim - Distr of Neovim

---

# Conclusion

---

Related Posts / Websites 👇

📑 [Tim Deschryver - Creating A Good Looking Windows Terminal](https://timdeschryver.dev/bits/creating-a-good-looking-windows-terminal)

📑 [Tim Deschryver - How I Have Set Up My New Windows Development Environment in 2022](https://timdeschryver.dev/blog/how-i-have-set-up-my-new-windows-development-environment-in-2022)

📑 [Microsoft - Quake Mode](https://learn.microsoft.com/en-us/windows/terminal/tips-and-tricks?WT.mc_id=DT-MVP-5004452#quake-mode)

📑 [Microsoft - Installing Powershell on Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5)

📑 [Microsoft - Get PSSsubsystem](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-pssubsystem?view=powershell-7.5)
