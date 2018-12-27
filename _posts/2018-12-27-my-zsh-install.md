---
layout: post
title: "备忘: 在macbookpro上安装zsh"
date: 2018-12-27 13:00:00
description: 记录自己在macbookpro上安装zsh的步骤
headline:
category: others
tags: [zsh MacOS iTerm2]
comments: true
imagefeature: true
mathjax:
---

###1.install iTerm2

    brew install caskroom/cask/iterm2

###2.install on-my-zsh

    sh -c "\$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

###3.install powerlevel9k theme

    git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k

###4.install syntax-highlighting (zsh plugin)

    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \$ZSH_CUSTOM/plugins/zsh-syntax-highlighting

###5.install autosuggestions (zsh plugin)

    git clone https://github.com/zsh-users/zsh-autosuggestions \$ZSH_CUSTOM/plugins/zsh-autosuggestions

###6.install shell integration (zsh plugin)

    curl -L https://iterm2.com/misc/install_shell_integration.sh | bash

###7.install fonts

    brew tap caskroom/fonts
    brew cask install font-hack-nerd-font

###8.my .zshrc file

    https://github.com/neilchencn/myzsh

###9.my zsh style

![My zsh screenshot](/images/post/pic.png)
