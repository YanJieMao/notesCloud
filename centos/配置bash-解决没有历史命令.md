



## 解决没有历史命令，修改bash

 vim ~/.bashrc

```bash
# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# 历史记录1000行
HISTSIZE=1000
# 开头输入空格 将不计入历史记录
HISTCONTROL=ignorespace

```

然后

source ~/.bashrc