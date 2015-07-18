# SSHH - A SSH session duplicator for tmux

SSHH is a simple helper tool which executes the same SSH command running at a specified window/pane on tmux sessions. You can quickly duplicate your current SSH session into a new pane or window without searching your command history by using the `sshh` command.

![Screenshot](https://raw.githubusercontent.com/yudai/sshh/master/sshh.gif)

SSHH is inspired by [the cdd command](https://github.com/m4i/cdd).

## Installation

Put the `sshh` file to any directory included in the `$PATH` environment variable.

## Usage

You can use the `sshh` command simply as a CLI tool.

```sh
Usage: sshh [window_index],[pane_index] [session_name]
```

```sh
# Run the same SSH command running at pane 0 window 3 in the current session
$ sshh 3

# Run the same SSH command running at pane 2 of window 3 in the current session
$ sshh 3,2

# Run the same SSH command at pane 3 of the current window
$ sshh ,3

# Run the same SSH command at window 1 in the session named `develop`
$ sshh 1 develop
```

When the process running at the specified window/pane is not the `ssh` command, `sshh` shows a prompt to ask if you really want to run it.

##  Usage with `split-window`

By adding (a little bit tricky) `bind-key` settings, you can open a new pane and duplicate your current SSH session into it at once. If you are a Zsh user, add the following lines to your `.tmux.conf`. Users of the other shells need to replace the `zsh` in the lines with your prefered shell command.

```
# Assign C-s to split pane horizontally and start a new SSH session
bind-key C-s run-shell "tmux split-window -h \"SSHH_INDEX=$(tmux display -p \",#{pane_index}\") zsh -l\"" \; send-keys ' sshh ${SSHH_INDEX}' ENTER

# Assign C-w to split pane vertically and start a new SSH session
bind-key C-w run-shell "tmux split-window -v \"SSHH_INDEX=$(tmux display -p \",#{pane_index}\") zsh -l\"" \; send-keys ' sshh ${SSHH_INDEX}' ENTER
```

## Usage with `new-window`

When you want to open a new window, you can write like:

```
bind-key C-n run-shell "tmux new-window \"SSHH_INDEX=$(tmux display -p \"#{window_index},#{pane_index}\") zsh -l\"" \; send-keys ' sshh ${SSHH_INDEX}' ENTER
```
