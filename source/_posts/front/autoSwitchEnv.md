---
title: 切换项目node环境
date: 2023-08-21 19:00:47
tags: 
  - 前端
  - 小技巧
category: developer
timeline: front-end
abstract: 在同时维护多个项目且存在历史项目，不可避免存在node版本不一致，如何启动项目根据项目node要求，自动切换shell中符合项目运行条件的node，以便快速响应维护开发。
---

大家都了解，在经历一定岁月，产品项目越来越多，老的项目在维护迭代，针对新的业务需求场景、技术迭代新的项目也孕育而生，在此背景下，各个项目依赖node版本随着不同。

当然，我们第一反应是nvm工具，同时项目中新增`.nvmrc`文件指定node版本，通过`nvm use`切换node环境，已经大大简化了。

::: tip
为追求更高效的开发效率，肯定不止于此啦～
:::

**期望 :fire:**

打开项目时，终端命令自动根据`.nvmrc`文件，自动切换好node版本环境，这样可以直接进行开发了。


**思路 :beach_umbrella:**

zsh (z shell)[$HOME/.zshrc]:

```zsh
# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

bash[$HOME/.bashrc]:

```bash
#
# Run 'nvm use' automatically every time there's 
# a .nvmrc file in the directory. Also, revert to default 
# version when entering a directory without .nvmrc
#
enter_directory() {
if [[ $PWD == $PREV_PWD ]]; then
    return
fi

PREV_PWD=$PWD
if [[ -f ".nvmrc" ]]; then
    nvm use
    NVM_DIRTY=true
elif [[ $NVM_DIRTY = true ]]; then
    nvm use default
    NVM_DIRTY=false
fi
}

export PROMPT_COMMAND="$PROMPT_COMMAND; enter_directory"
```


**效果 :orange_square:**

![1](/assets/front-end/autoSwitch.png)


**参考文献 :o2:**

* [stack overflow](https://stackoverflow.com/questions/23556330/run-nvm-use-automatically-every-time-theres-a-nvmrc-file-on-the-directory/48322289#48322289)