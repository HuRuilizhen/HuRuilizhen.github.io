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

Personally, I like to set Windows Terminal with transparent background and Quake Mode (slide down from the top of the screen with a hotkey). Go to the Windows Terminal Settings, `Settings > General > Appearance > Transparency`. Enable Quake Mode by following the instructions on the [Microsoft Quake Mode Page](https://learn.microsoft.com/en-us/windows/terminal/tips-and-tricks?WT.mc_id=DT-MVP-5004452#quake-mode). You can also set the background image, font, and other settings. Here is my current profile settings in json format:

```json
{
  "startup": {
    "defaultProfile": "PowerShell",
    "defaultTerminalApplication": "Windows Terminal",
    "language": "English (United States)",
    "defaultImeInputMode": "Default",
    "launchOnMachineStartup": true,
    "firstWindowPreference": "openTabWithDefaultProfile",
    "newInstanceBehavior": "createNewWindow",
    "launchSize": {
      "columns": 120,
      "rows": 30
    },
    "launchParameters": "default"
  }
}
```

My appearance settings for PowerShell:

```json
{
  "profiles": {
    "list": [
      {
        "name": "PowerShell",
        "colorScheme": "Campbell",
        "font": {
          "face": "MesloLGM Nerd Font",
          "size": 12,
          "weight": "normal"
        },
        "lineHeight": 1.2,
        "cellWidth": 0.6,
        "useAcrylic": false,
        "opacity": 60,
        "cursorShape": "bar",
        "cursorColor": "#FFFFFF",
        "padding": "8, 8, 8, 8",
        "scrollbarState": "visible",
        "intenseTextStyle": "bright",
        "backgroundImage": null,
        "backgroundImageStretchMode": "none",
        "experimental.retroTerminalEffect": false,
        "antialiasingMode": "cleartype",
        "adjustIndistinguishableTextLightness": "never",
        "useAtlasEngine": true,
        "useAcrylicMaterial": false,
        "backgroundImageOpacity": 1.0,
        "backgroundImageAlignment": "center",
        "cursorHeight": 100,
        "fontFeatures": [],
        "builtinGlyphs": true,
        "experimental.retroTerminalEffect": false,
        "useAcrylic": false,
        "acrylicOpacity": 0.6,
        "experimental.pixelShaderPath": null,
        "intenseTextStyle": "bright",
        "backgroundImageOpacity": 1.0,
        "backgroundImageStretchMode": "none",
        "backgroundImageAlignment": "center",
        "useAtlasEngine": true,
        "experimental.retroTerminalEffect": false,
        "backgroundImageOpacity": 1.0,
        "useAcrylic": false,
        "backgroundOpacity": 60,
        "experimental.pixelShaderPath": null,
        "intenseTextStyle": "bright",
        "backgroundImage": null,
        "experimental.retroTerminalEffect": false,
        "builtinGlyphs": true,
        "experimental.useColorEmoji": true
      }
    ]
  }
}
```

Also, if you want to use Windows Terminal effectively, shortcut keys and hotkeys can be very useful. 
- `ctrl+shift+c` to close the current tab
- `ctrl+shift+t` to open a new tab
- `ctrl+shift+n` to open a new window
- `ctrl+shift+m` to open marker mode
- `ctrl+tab` to switch tabs forward
- `ctrl+shift+tab` to switch tabs backwards
- `f11` to toggle fullscreen mode
- `ctrl+f12` to toggle focus mode
- `ctrl+shift+p` to open the command palette

You can find more in `Settings > Actions`.

## Basic PowerShell Instructions

Commands in PowerShell can be very different from those in traditional Unix-like shells. Tutorials for beginners can be found in the [PowerShell Documentation](https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/01-getting-started?view=powershell-7.5). There is a principal called "Verb-Noun" for cmdlet naming conventions. For example, `Get-Help`, `Set-Location`, `Get-Process`, etc. You can use `Get-Help <cmdlet-name>` to get help on any cmdlet. For example:

```powershell
# Get help for the Get-Process cmdlet
Get-Help Get-Process

# Get commands related to a specific noun
Get-Command -Noun <noun>

# Get processes running on the system (ps equivalent)
Get-Process

# Get current directory (pwd equivalent)
Get-Location

# Get Items in the current directory (ls equivalent)
Get-ChildItem
```

Not that hard, right? Once you get used to it, PowerShell can be very powerful and flexible. But there is still a huge difference between PowerShell and traditional shells, so be sure to check out the documentation for more details. Especially, parameters and piping. Unlike traditional shells (such as CMD and Bash) that pass **text**, PowerShell passes rich ·.NET· **objects** that contain **properties** and **methods**. Here are some examples for reference:

```powershell
# Piping example: Get processes and sort by CPU usage
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5

# Parameter example: Get processes with a name containing "chrome" and stop them
Get-Process -Name *chrome* | Stop-Process
```

## Basic PowerShell Profile Setup

Powershell profile is a file that contains commands and settings that are executed when you open a new PowerShell session. Consider it like a startup script for your PowerShell (.bashrc for Unix-like shells). You can add some basic commands and settings to your profile to make your life easier. Later, I will share my PowerShell profile setup in this post function by function. To open your profile, type `<editor> $PROFILE` in PowerShell. Again, note that `$PROFILE` is different for different PowerShell versions. So, make sure to replace it with the correct path.

---

# Package Management Tools

Like apt for Ubuntu, yum for CentOS, brew for macOS, etc. Windows Terminal comes with a package manager called **winget** and a package manager called **chocolatey**. They are both very useful and easy to use. But focus on different use cases. I both of them for different purposes.

## winget

Officially produced by Microsoft, it is lightweight, modern, and tightly integrated with the system. It is more like "App Store CLI" for Windows, focusing on installing and managing free open source or store applications. Features:
- **Official Installation**: In most cases, the official installer (e.g., .msi, .exe) is called and then installed using its silent parameters. The more modern MSIX is also supported.
- **Secure Source**: Microsoft will perform some scanning and verification, and the source is relatively trustworthy.
- **Simple Usage**: The command line syntax is more modern and concise.

You can find more information on the [Winget GitHub page](https://github.com/microsoft/winget-cli) or check the official website at [Windows Package Manager](https://learn.microsoft.com/en-us/windows/package-manager/). It is available for Windows 11 by default most of the time. You can also install it via the Windows Store by searching for "Winget". For some useful commands, you can refer to the [Winget documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/install?source=recommendations). 

## chocolatey

Chocolatey is an "automated software deployment platform" for Windows that manages software as code. Features:
- **Community Driven** : Community projects, commercial company support. "Trust lies in the community." The packages in the public repository are provided by different maintainers, and users need to make their own judgments.
- **Powerful Installation**: It completely relies on automated scripts (PowerShell) to download and install silently, and can handle very complex installation processes (such as entering serial numbers, copying files, etc.).
- **Classic Syntax**: The command line syntax is classic and powerful, and some commands are more complex.

Download and install Chocolatey via the [Chocolatey GitHub page](https://github.com/chocolatey/chocolatey) and official website at [Chocolatey Installation](https://chocolatey.org/install).  Run the following command:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

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

In another post of mine, I also talked about how to set up `windsurf` (powerful but free AI tool) with `Neovim`. please check it out [here](/Linux-Terminal-Beautify#integrate-with-ai-tools).

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
