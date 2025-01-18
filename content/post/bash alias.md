---
title: "bash alias"
type: "post"
date: "2025-01-17T18:12:45+08:00"
categories: 
  - "筆記"
tags:
  - "bash"
---

厭倦一直要找某個很長的command了嗎
or打很長的command嗎

<!--more-->

---

如果只是單純的一行指令

推薦新增bash alias來完成這件事

(這邊操作預設都是linux上 或是windows的git bash上)

先更新bash config

`vim ~/.bashrc`

```bash
# Aliases
# alias alias_name="command_to_run"

# Long format list
alias ll="ls -la"

# Print my public IP
alias myip='curl ipinfo.io/ip'
```

改完之後再使用

`source ~/.bashrc`

就可以直接使用alias 的command使用

Ref :

[How to Create Bash Aliases | Linuxize](https://linuxize.com/post/how-to-create-bash-aliases/)