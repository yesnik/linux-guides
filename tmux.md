# tmux

**tmux** is a terminal multiplexer that allows you to run multiple Linux programs over a single connection.

Tmux also decouples your programs from the main terminal, protecting them from accidentally disconnecting. 
You can detach tmux from the current terminal, and all your programs will continue to run safely in the background.

```
dnf install tmux
```

## Commands

- `tmux` - launch a tmux server, create a default session (number 0) with a single window, and attach to it.
- `tmux ls` - list active tmux sessions
- `tmux attach` - attach to the last session
- `tmux attach -t 2` - attach to session Number 2

### Keyboard Commands

- `Ctrl + B`, `C` - create new window
- `  -"-  `, `W` - show windows
- `  -"-  `, `D` - detach from your tmux session
- `  -"-  `, `X` - close pane
- `  -"-  `, `"` - split window horizontally
- `  -"-  `, `%` - split window vertically
