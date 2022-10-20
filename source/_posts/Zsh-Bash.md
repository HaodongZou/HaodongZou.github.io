---
title: Zsh/Bash
date: 2022-10-20 17:32:57
categories: 软件
tags: 
    - Zsh
    - Bash
    - 命令行
---

Zsh/Bash配置笔记

<!--more-->

[Zsh](https://www.zsh.org/)

> Z shell（Zsh）是一款可用作交互式登录的shell及脚本编写的命令解释器。Zsh对Bourne shell做出了大量改进，同时加入了Bash、ksh及tcsh的某些功能。

[https://github.com/ohmyzsh/ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)

> Oh My Zsh is an open source, community-driven framework for managing your [zsh](https://www.zsh.org/)
>  configuration.

```bash
# 查看当前环境SHELL
echo $SHELL

# 安装ZSH
brew install zsh
# 在 /etc/shells 文件中加入 /usr/local/bin/zsh
# 将ZSH设置为默认的SHELL
chsh -s /usr/local/bin/zsh

# 查看当前环境SHELL。如果没有变成ZSH，尝试重启终端
echo $SHELL

# 安装oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 添加ZSH扩展

在`~/.zshrc`中找到`plugins`关键字，就可以自定义启用的插件了，系统默认加载`git`。

- 添加****`zsh-autosuggestions`****

  [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

  ```bash
  # 克隆仓库到 $ZSH_CUSTOM/plugins (默认位置 ~/.oh-my-zsh/custom/plugins)
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  
  # 在～/.zshrc中修改plugin关键字
  plugins=( 
      # other plugins...
      zsh-autosuggestions
  )
  ```

- 添加****`zsh-syntax-highlighting`****

  [https://github.com/zsh-users/zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

  ```bash
  # 克隆仓库到 $ZSH_CUSTOM/plugins (默认位置 ~/.oh-my-zsh/custom/plugins)
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  
  # 在～/.zshrc中修改plugin关键字
  plugins=( 
      # other plugins...
      zsh-syntax-highlighting
  )
  ```

## 自定义ZSH

- 别名
  可以简化命令输入，在 `.zshrc` 中添加 `alias shortcut='this is the origin command'` 一行就相当于添加了别名。在命令行中输入 `alias` 可以查看所有的命令别名。

    ```bash
  # 自定义alias 比如设置环境变量
  # open vpn
  alias opv="export all_proxy=127.0.0.1:7890"
  # close vpn
  alias clv="unset all_proxy"
  
  # 常用自带alias
  1='cd -1' #返回cd stack中的上一个
  2='cd -2'
  _='sudo '
  la='ls -lAh'
  md='mkdir -p'
  rd=rmdir
  
  # 常用的git插件自带alias
  g=git
  
  gaa='git add --all'
  
  gb='git branch'
  
  gc='git commit -v' # 打开一个编辑器窗口写入message，并在其中unified形式展示修改
  gcam='git commit -a -m'
  gcsm='git commit -s -m' # 会在message后自动加一个签名显示提交人和邮箱
  
  gcb='git checkout -b'
  gcm='git checkout $(git_main_branch)'
  
  gcf='git config --list'
  
  gf='git fetch'
  
  gl='git pull'
  gpr='git pull --rebase'
  
  gp='git push'
  gpd='git push --dry-run'
    ```

## 常用技巧

- 目录浏览和跳转：输入`d`，即可列出你在这个会话里访问的目录列表，输入列表前的序号，即可直接跳转。
- 在当前目录下输入 `..` 或 `...` ，或直接输入当前目录名都可以跳转，你甚至不再需要输入 `cd` 命令了。在你知道路径的情况下，比如 `/usr/local/bin` 你可以输入 `cd /u/l/b` 然后按进行补全快速输入
- 更智能的历史命令。在用或者方向上键查找历史命令时，zsh支持限制查找。比如，输入`ls`,然后再按方向上键，则只会查找用过的`ls`命令。
