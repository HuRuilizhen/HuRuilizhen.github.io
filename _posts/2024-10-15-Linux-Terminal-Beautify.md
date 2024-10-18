---
layout: post
title: "Linux Terminal Beautify"
description: "A quick start on how to use Docker"
date: 2024-10-15
feature_image: images/terminal.png
tags: ['command-line-interface', 'linux']
---

Ever since I mistakenly refreshed my desktop to `Ubuntu` system and left it idle for a long time, I now try to turn it into a productivity tool. The first thing needs to be done is to beautify the terminal just like the one in my `Macbook Air`. Here is how I did it: 💻

<!--more-->

---

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

- **Nerd Font**:
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

Reference: 

📑 [How to make Linux terminal look awesome](https://www.geeksforgeeks.org/how-to-make-linux-terminal-look-awesome/)