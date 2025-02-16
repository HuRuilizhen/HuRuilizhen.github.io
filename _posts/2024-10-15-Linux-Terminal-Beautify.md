---
layout: post
title: "Linux Terminal Beautify"
description: "A quick start on how to beautify the terminal in Linux"
date: 2024-10-15
feature_image: images/terminal.png
tags: ['command-line-interface', 'linux']
---

Ever since I mistakenly refreshed my desktop to `Ubuntu 24.04` system and left it idle for a long time, I now try to turn it into a productivity tool. The first thing needs to be done is to beautify the terminal just like the one in my `Macbook Air`. Here is how I did it: 💻

<!--more-->

---

# Shell Setup

- **ZSH**

    A powerful and user-friendly Unix shell that combines the features of both the Bourne shell (sh) and C shell (csh), while also adding many new and innovative features. It aims to be a highly functional and interactive shell. Some key features of ZSH include: `Command Line Completion`, `Spelling Correction`, `History Management`, `Plugin Architecture`, and `Theme Support`. Since I use ubuntu environment, I prefer using `apt` to install `zsh`.

    ```bash
    sudo apt install zsh
    ```

    >If you meet some problems in setting `zsh` as default shell, you can use `chsh -s /bin/zsh` to set. I think if you finish setting up `Oh My ZSH` successfully, you actually already make `zsh` as your default shell. Check it with `echo $SHELL`. But if still meet problems, just restart your Ubuntu Desktop.

- **Oh My ZSH**

    This third-party plugin manager is a collection of plugins for ZSH that I use. It provides a bunch of useful features and plugins. Here is how I installed it:

    ```bash
    sh -c “$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)”
    ```

    And here is a screenshot of what we have done:

{% include image_caption.html imageurl="/images/terminal-with-oh-my-zsh.png" title="Terminal with Oh My ZSH" caption="Terminal with Oh My ZSH"%}

- **Nerd Font**

    With `Nerd Font` you can get a beautiful terminal with awesome icons. If you don't have `Nerd Font` installed, you can choose one [here](https://www.nerdfonts.com/font-downloads)
    
    If you have `Nerd Font` installed, you can set it in `Terminal>Preferences>Profile>Custom Fonts` like below:
{% include image_caption.html imageurl="/images/terminal-with-nerd-font.png" title="Terminal with Nerd Font" caption="Terminal with Nerd Font"%}

- **Powerlevel10k**
  
    Apowerfull theme I use for my terminal. It is a good choice for a good productivity. You can find it at [Powerlevel10k GitHub repo](https://github.com/romkatv/powerlevel10k). Using git to clone it is easy:

    ```bash
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    ```

    - Then set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`
    - Restart your terminal or type `source ~/.zshrc`
    - Set the same terminal font in VSCode `"terminal.integrated.fontFamily": "'0xProto Nerd Font'"` (optional)
    
    With a screenshot, you can see terminal with `Powerlevel10k` theme:

{% include image_caption.html imageurl="/images/terminal-with-powerlevel10k.png" title="Terminal with Powerlevel10k" caption="Terminal with Powerlevel10k"%}

- **zsh-autosuggestions**
  
    A plugin that suggests commands based on what you have typed. 

    ```bash
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    ```

    - Then set `plugins=(... zsh-autosuggestions)` in `~/.zshrc`
    - Restart your terminal or type `source ~/.zshrc`

- **zsh-syntax-highlighting**
  
    A plugin that highlights your command syntax. It is a must-have plugin.

    ```bash
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    ```

    - Set `plugins=(... zsh-syntax-highlighting)` in `~/.zshrc`
    - Restart your terminal or type `source ~/.zshrc`
  
---

# Command Line Tool Setup

- **bat**
  
    Which is short for better cat. Basically, it is a cat with syntax highlighting, line numbers, and pages.

    ```bash
    sudo apt install bat
    ```

    Here is a screenshot of using `bat` to show `README.md` file:

{% include image_caption.html imageurl="/images/terminal-with-bat.png" title="Terminal with bat" caption="Terminal with bat"%}

- **fzf**
  
    A command-line fuzzy finder that helps you select files and commands. I don't recommend using apt to install `fzf`, since you must do key-binding for it manually. Therefore, git clone it from [GitHub](https://github.com/junegunn/fzf).

    ```bash
    git clone –depth 1 https://github.com/junegunn/fzf.git ~/.fzf
    ```

    - Then install `fzf` using `~/.fzf/install`
    - Restart your terminal or type `source ~/.zshrc`

    | Key Bindings               | Actions                         |
    | -------------------------- | ------------------------------- |
    | `<C-t>`                    | Find files                      |
    | `<C-r>`                    | Find commands                   |
    | `<command> <file path>/**` | Combine fzf with other commands |

    - Additionally, if you want to make `fzf` work with `editor`, you can add following lines in `~/.zshrc`:
    ```bash
    function open-editor-with-fzf() {
        local file=$(find . -type f | fzf --preview="cat {}")
        if [[ -n $file ]]; then
            ${EDITOR:-nvim} "$file"
        fi
    }
    zle -N open-editor-with-fzf
    bindkey "^E" open-editor-with-fzf
    ```

- **eza**
  
    Better ls command with colors and icons. Make your terminal look more interesting.

    ```bash
    sudo apt install eza
    ```

    And setting alias `ls` to `eza` in `~/.zshrc`

    ```bash
    # --- eza ---
    alias ls="eza --color=always --long --git --icons=always --no-time --no-user --no-permissions"
    alias ll="eza --color=always --long --git --icons=always --header"
    ```
    Then restart your terminal or type `source ~/.zshrc`, and try `ls` and `ll`.

{% include image_caption.html imageurl="/images/terminal-with-eza-ls.png" title="Terminal with eza ls" caption="Terminal with eza ls"%}

{% include image_caption.html imageurl="/images/terminal-with-eza-ll.png" title="Terminal with eza ll" caption="Terminal with eza ll"%}

- **tldr**
  
    A command-line man page reader but more user-friendly.

    ```bash
    sudo apt install tldr
    ```
    
    See different commands with `tldr` using `tldr <command>`, here is an example comparing `tldr` and `man`:

{% include image_caption.html imageurl="/images/terminal-with-tldr.png" title="Terminal with tldr" caption="Terminal with tldr"%}

{% include image_caption.html imageurl="/images/terminal-with-man.png" title="Terminal with man" caption="Terminal with man"%}    

- **fastfetch**

    A command-line system information tool that is easy to use and fast. The install process is a bit different from others. We need to add `ppa:zhangsongcui3371/fastfetch` to our sources, and then install it.

    ```bash
    sudo add-apt-repository ppa:zhangsongcui3371/fastfetch
    sudo apt update
    sudo apt install fastfetch
    ```

    By the way, we can directly install `fastfetch` by typing `brew install fastfetch` in MacOS. Here are some screenshots of using `fastfetch` to show system information (Apple Silicon and Linux):

{% include image_caption.html imageurl="/images/fastfetch-apple-silicon.jpg" title="Apple Silicon System Info" caption="Apple Silicon System Info"%}

{% include image_caption.html imageurl="/images/fastfetch-linux.png" title="Linux System Info" caption="Linux System Info"%}

---

# Editor Setup

- **Neovim**
    AstroNvim is an aesthetically pleasing and feature-rich neovim config that is extensible and easy to use with a great set of plugins. To install it, you need to install `neovim` first.
    ```bash
    sudo apt install neovim
    ```
    - Then clean up cache and state files:
    ```bash
    mv ~/.local/share/nvim ~/.local/share/nvim.bak
    mv ~/.local/state/nvim ~/.local/state/nvim.bak
    mv ~/.cache/nvim ~/.cache/nvim.bak
    ```
    - Clone [AstroNvim](https://github.com/AstroNvim/AstroNvim) repository
    ```bash
    git clone --depth 1 https://github.com/AstroNvim/template ~/.config/nvim
    # remove template's git connection to set up your own later
    rm -rf ~/.config/nvim/.git
    nvim
    ```

- **Integrate with AI Tools** 

    Codeium is a powerful code assistant that provides an AI-powered code completion, code refactoring, and code formatting. I usually use it in my daily work before with `VSCode`. Let's do it in `Neovim`, add a new file named `codeium.lua` in `~/.config/nvim/lua/plugins/` and add the following lines:

    ```lua
    return {
      'Exafunction/codeium.vim',
      event = 'BufEnter'
    }
    ```

    - Then install the plugin, by typing `SPC + p + i` in `Neovim` (`normal` mode)
    - Before you start using codeium you need to auth your account type `:Codeium Auth` in `Neovim` (`command` mode)
    - Look for more help info you can use `:help codeium`. Here list some useful commands:

    | Action                      | Function                       | Default Binding |
    | --------------------------- | ------------------------------ | --------------- |
    | Clear current suggestion    | `codeium#Clear()`              | `<C-]>`         |
    | Next suggestion             | `codeium#CycleCompletions(1)`  | `<M-]>`         |
    | Previous suggestion         | `codeium#CycleCompletions(-1)` | `<M-[>`         |
    | Insert suggestion           | `codeium#Accept()`             | `<Tab>`         |
    | Manually trigger suggestion | `codeium#Complete()`           | `<M-Bslash>`    |
    | Accept word from suggestion | `codeium#AcceptNextWord()`     | `<C-k>`         |
    | Accept line from suggestion | `codeium#AcceptNextLine()`     | `<C-l>`         |

    - You can also use `:Codeium Enable` and `:Codeium Disable` to turn on/off the plugin.
    - More info see: [Codeium GitHub Repository](https://github.com/Exafunction/codeium.vim)

{% include image_caption.html imageurl="/images/nvim-codeium.jpg" title="Codeium Setting in Neovim" caption="Codeium Setting in Neovim" %}
---

Reference: 

📑 [How to make Linux terminal look awesome](https://www.geeksforgeeks.org/how-to-make-linux-terminal-look-awesome/)

📑 [7 Amazing Cli Tools](https://www.josean.com/posts/7-amazing-cli-tools)

📑 [Fastfetch Setup in Ubuntu](https://launchpad.net/~zhangsongcui3371/+archive/ubuntu/fastfetch)

📑 [AstroNvim Official Document](https://docs.astronvim.com/)

📑 [Codeium in Vim/Neovim](https://codeium.com/vim_tutorial)

📑 [Codeium Github Repository](https://github.com/Exafunction/codeium.vim)