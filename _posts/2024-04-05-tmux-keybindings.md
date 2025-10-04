---
layout: post
title: "Tmux Keybindings"
date: 2024-04-05
categories: [CHEATSHEET]
---

Tmux commands typically start with a "prefix key", which is `Ctrl-b` by default, followed by a command key. Here are some commonly used tmux keybindings:

## Creating and Managing Windows and Panes
  * `Ctrl-b c`: Creates a new window within tmux.
  * `Ctrl-b ,`: Rename the current window; useful for keeping track of multiple windows.
  * `Ctrl-b %`: Splits the current pane vertically into two panes.
  * `Ctrl-b "`: Splits the current pane horizontally.
  * `Ctrl-b o`: Cycles through panes within the current window.
  * `Ctrl-b x`: Closes the current pane.
 
## Navigating Between Windows
  * `Ctrl-b n`: Moves to the next window.
  * `Ctrl-b p`: Moves to the previous window.
  * `Ctrl-b [0-9]`: Switches to a window by its index number.

## Detaching and Re-attaching
  * `Ctrl-b d`: Detaches the current session (leaving it running in the background).
  * Reattach to a session with `tmux attach-session -t [session name]`.

## Scrolling and Copy Mode
`Ctrl-b [`: Enters the copy mode, which allows for scrolling and copying text. You can navigate using the arrow keys or `vim`-like keys (`j`, `k`, `h`, `l`).

## Tmux Configuration File
### Changing the Prefix Key
  * `unbind C-b`: Removes the default binding for `Ctrl-b`.
  * `set-option -g prefix C-a`: Changes the prefix to `Ctrl-a`.

### Improving Pane Navigation
  * `bind-key -n C-h select-pane -L`: Move to the left pane using `Ctrl-h`.
  * `bind-key -n C-j select-pane -D`: Move to the bottom pane using `Ctrl-j`.
  * `bind-key -n C-k select-pane -U`: Move to the top pane using `Ctrl-k`.
  * `bind-key -n C-l select-pane -R`: Move to the right pane using `Ctrl-l`.


### Automatically Renaming Windows
 `set-option -g automatic-rename on`: Automatically renames windows to reflect the current application in use.

After editing `~/.tmux.conf`, you can apply the changes by either restarting tmux or by executing `tmux source-file ~/.tmux.conf` within a tmux session.

