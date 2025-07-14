## Step

```bash
# 查看当前使用的 shell
echo $SHELL

# 查看系统安装的 shell
cat /etc/shells

# 如果没有 zsh，安装它（使用 Homebrew）
brew install zsh

# 切换默认 shell 为 zsh
chsh -s /bin/zsh

# https://zimfw.sh/
curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
# 安装完成后，会自动生成 ~/.zshrc 和 ~/.zimrc 配置文件。

# 插件配置
vim ~/.zimrc

# 环境变量
vim ~/.zshrc
```

## Config

### .zimrc

```bash
# Start configuration added by Zim install {{{
#
# This is not sourced during shell startup, and it's only used to configure the
# zimfw plugin manager.
#

#
# Modules
#

# Sets sane Zsh built-in environment options.
zmodule environment
# Provides handy git aliases and functions.
zmodule git
# Applies correct bindkeys for input events.
zmodule input
# Sets a custom terminal title.
zmodule termtitle
# Utility aliases and functions. Adds colour to ls, grep and less.
zmodule utility

#
# Prompt
#

# Exposes to prompts how long the last command took to execute, used by asciiship.
zmodule duration-info
# Exposes git repository status information to prompts, used by asciiship.
zmodule git-info
# A heavily reduced, ASCII-only version of the Spaceship and Starship prompts.
zmodule prompt-pwd
# Themes
zmodule steeef

#
# Completion
#

# Additional completion definitions for Zsh.
zmodule zsh-users/zsh-completions --fpath src
# Enables and configures smart and extensive tab completion.
# completion must be sourced after all modules that add completion definitions.
zmodule completion

#
# Modules that must be initialized last
#

# Fish-like syntax highlighting for Zsh.
# zsh-users/zsh-syntax-highlighting must be sourced after completion
zmodule zdharma-continuum/fast-syntax-highlighting
# Fish-like history search (up arrow) for Zsh.
# zsh-users/zsh-history-substring-search must be sourced after zsh-users/zsh-syntax-highlighting
zmodule zsh-users/zsh-history-substring-search
# Fish-like autosuggestions for Zsh.
zmodule zsh-users/zsh-autosuggestions
# }}} End configuration added by Zim install

# zmodule Aloxaf/fzf-tab
```

### .zshrc

```bash
# .zshrc
# 添加自定义环境变量
export PATH="$HOME/bin:$PATH"
export http_proxy="http://127.0.0.1:9910"
export https_proxy="http://127.0.0.1:9910"

# 常用别名
alias proxy_on="export http_proxy=http://v2v2:147268359@192.168.1.33:10810; export https_proxy=http://v2v2:147268359@192.168.1.33:10810; echo 'HTTP代理已开启'"
alias proxy_geph_on="export http_proxy=http://127.0.0.1:9910; export https_proxy=http://127.0.0.1:9910; echo 'HTTP代理已开启'"
alias proxy_off="unset http_proxy; unset https_proxy; echo 'HTTP代理已关闭'"
```

## Set

### 光标按照单词快速移动设置

![image-20250714150836249](./assets/image-20250714150836249.png)

- 刪除一整行：Action 選擇 Send Hex Code：輸入0x15
- 刪除一個字詞：Action 選擇 Send Hex Code：輸入0x17
- 跳到行頭：Action 選擇 Send Hex Code：輸入0x01
- 跳到行尾：Action 選擇 Send Hex Code：輸入0x05
- 跳到前一個字詞：Action 選擇 Send Escape Sequence：輸入 f
- 跳到後一個字詞：Action 選擇 Send Escape Sequence：輸入 bv

### status bar

![image-20250714165413555](./assets/image-20250714165413555.png)

![image-20250714165438439](./assets/image-20250714165438439.png)