---
layout: post
title: "Upgrading Reverse Shell to Fully Interactive TTYs"
date: 2025-03-09
categories: [CHEATSHEET]
---

When we establish a reverse shell, it is often very limited in functionality and prone to breaking. 

Basic features like command history navigation (using up/down arrows) or autocomplete may not work. To resolve these limitations, we can upgrade our TTY using the `python` or `stty` method. 

Below is a step-by-step guide on how to do this.

## Spawn a TTY Shell  
Run this in the reverse shell:
```shell-session
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## Adjust Terminal Settings
Suspend the shell: `Ctrl+Z`, then in your local terminal, run:
    
```shell-session
stty raw -echo  
fg  
[Press Enter Twice]  
```
    
## Fix Terminal Size (Optional)

In a local terminal:

```shell-session
echo $TERM          # e.g., xterm-256color  
stty size           # e.g., 38 116  
```

Back in the reverse shell, set:
```shell-session
export TERM=xterm-256color  
stty rows 38 columns 116  
```

You now have a fully interactive TTY shell with command history, autocomplete, and full terminal features.

Reference: [Ropnop Blog](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/){:target="_blank"}
