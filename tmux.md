# tmux #

`tmux` lets you run numerous TTY’s (_TeleTYpewriter_) in the same terminal window. You can run multiple _panes_ in a single window, easily configuring the layout and switching between them.

*Installation*

	brew install tmux

## Default keybindings & Functionality ##

You can interact with `tmux` by pressing the `prefix` keyboard shortcut and then issuing a command. By default this prefix is `Ctrl+b`. For me `Ctrl+t` feels more natural to remember.

You can rebind this by putting the following in `~/.tmux.conf`:

	set -g prefix Ctrl-t

Some of the most used commands:

- `Ctrl-b c` Create new window
- `Ctrl-b d` Detach current client
- `Ctrl-b l` Move to previously selected window
- `Ctrl-b n` Move to the next window
- `Ctrl-b p` Move to the previous window
- `Ctrl-b &` Kill the current window
- `Ctrl-b ,` Rename the current window
- `Ctrl-b %` Split the window vertically
- `Ctrl-b :` Split the window horizontally
- `Ctrl-b q` Show pane numbers (used to switch between panes)
- `Ctrl-b o` Switch to the next pane
- `Ctrl-b ?` List all keybindings

### Basic Pane Handling

One of the most powerful features tmux offers is the ability to split up your current window into “panes”. Anyone whose familiar with tiling windows managers will feel quite at home.

- `Ctrl-b %` Split window vertically
- `Ctrl-b :` Split window horizontally
- `Ctrl-b o` Goto next pane
- `Ctrl-b q` Show pane numbers, when the numbers show up type the key to goto - that pane
- `Ctrl-b {` Move the current pane left
- `Ctrl-b }` Move the current pane right

I personally rebind the keys so `|` splits the current window vertically, and `-` splits it horizontally. You can achieve this by putting the following in `~/.tmux.conf`

	unbind %
	bind | split-window -h
	unbind :
	bind – split-window -v

## Other things

### Reloading configuration ###

You can reload the configuration with the source-file command. This can be done either from within tmux, by pressing the prefix shortcut and then `:` to bring up a command prompt, and typing

```
:source-file ~/.tmux.conf
```

Or simply from a shell:

```shell
$ tmux source-file ~/.tmux.conf
```
