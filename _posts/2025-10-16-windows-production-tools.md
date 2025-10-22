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

Powershell profile is a file that contains commands and settings that are executed when you open a new PowerShell session. Consider it like a startup script for your PowerShell (.bashrc for Unix-like shells). You can add some basic commands and settings to your profile to make your life easier. Later, I will share my PowerShell profile setup in this post function by function. To open your profile, type `<editor> $PROFILE` in PowerShell. Again, note that `$PROFILE` is different for different PowerShell versions. So, make sure to replace it with the correct path.

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

This is my favorite Neovim distribution. It comes with a lot of pre-configured plugins and settings that make it easy to use right out of the box. You can find more information and installation instructions on the [Astronvim GitHub page](https://github.com/AstroNvim/AstroNvim), or check the official website at [https://astronvim.com/](https://astronvim.com/). Before configuring Astronvim, make sure you have Neovim installed. You can install Neovim via Chocolatey with the command:

```powershell
choco install neovim
```

And also we have some dependencies for better coding experience:
- Nerd Fonts: done at [font](#font). Necessary for proper display of icons in the status line and file explorer.
- ripgrep: A fast search tool that is used by many Neovim plugins for searching files. Install via Chocolatey: `choco install ripgrep`. `<Leader>fw` to search files.
- lazygit: done at [lazygit](#lazygit). Default git client for Astronvim. Install via Chocolatey: `choco install lazygit`. `<Leader>gg` to open lazygit.
- gdu: Short for "go disk usage", A fast disk usage analyzer that can be used within Neovim. Install via Chocolatey: `choco install gdu`. `<Leader>gs` to open gdu.
- bottom: done at [bottom](#bottom). A better performance monitor. Install via Chocolatey: `choco install bottom`. `<Leader>tt` to open bottom.
- Python: Install via Chocolatey: `choco install python`. `<Leader>tp` to open Python REPL.
- Node.js: Node is needed for a lot of the LSPs. Install via Chocolatey: `choco install nodejs`. `<Leader>tn` to open Node.js REPL.

Next, just follow the [Astronvim Installation Guide](https://docs.astronvim.com/#-installation) to set it up. 

- Make a backup of your current nvim config (if exists)
    ```powershell
    Move-Item $env:LOCALAPPDATA\nvim $env:LOCALAPPDATA\nvim.bak
    ```
- Clean neovim folders (Optional but recommended)
    ```powershell
    Move-Item $env:LOCALAPPDATA\nvim-data $env:LOCALAPPDATA\nvim-data.bak
    ```
- Clone the repository
    ```powershell
    git clone --depth 1 https://github.com/AstroNvim/template $env:LOCALAPPDATA\nvim
    # remove template's git connection to set up your own later
    Remove-Item $env:LOCALAPPDATA\nvim\.git -Recurse -Force
    ``` 

In another post of mine, I also talked about how to set up `windsurf` (powerful and free AI tool) with `Neovim`. please check it out [here](/Linux-Terminal-Beautify#integrate-with-ai-tools).

---

# Conclusion

Customize your terminal to fit your workflow and enjoy a more efficient coding experience on Windows! And here is my final checklist of tools and settings (I deleted some less relevant tools for brevity):

```powershell
# Chocolatey Packages
> choco list
Chocolatey v2.5.1
bat 0.25.0
bottom 0.11.2
chocolatey 2.5.1
dust 1.2.3
fastfetch 2.53.0
fzf 0.59.0
git 2.51.0.2
git.install 2.51.0.2
Lazydocker 0.23.3
lazygit 0.55.1
less 679.0.0
neovim 0.11.4
nodejs 22.20.0
nodejs.install 22.20.0
oh-my-posh 27.1.2
ripgrep 14.1.0
tldr 1.0.0
tldr-plusplus 0.6.1
winfetch 2.5.1
zoxide 0.9.2
```

And here is my current profile settings:

```powershell
# ---- Oh My Posh ----
oh-my-posh init pwsh --config "${env:POSH_THEMES_PATH}\powerlevel10k_rainbow.omp.json" | Invoke-Expression

# ---- Terminal Icons ----
Import-Module -Name Terminal-Icons
  
# ---- PSReadLine ----
Set-PSReadLineOption -PredictionViewStyle ListView

# ---- Zoxide ----
# Just copy and pasted from tldr zoxide init instructions. It works! :)
```

For Powershell Modules and PSSusbsytems, refer to the [Powershell Modules and PSSubsystems Checklist](#powershell-modules-and-pssubsystems).

---

Related Posts / Websites 👇

📑 [Tim Deschryver - Creating A Good Looking Windows Terminal](https://timdeschryver.dev/bits/creating-a-good-looking-windows-terminal)

📑 [Tim Deschryver - How I Have Set Up My New Windows Development Environment in 2022](https://timdeschryver.dev/blog/how-i-have-set-up-my-new-windows-development-environment-in-2022)

📑 [Microsoft - Quake Mode](https://learn.microsoft.com/en-us/windows/terminal/tips-and-tricks?WT.mc_id=DT-MVP-5004452#quake-mode)

📑 [Microsoft - Installing Powershell on Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5)

📑 [Microsoft - Get PSSsubsystem](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-pssubsystem?view=powershell-7.5)
