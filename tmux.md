# tmux #

`tmux` lets you run numerous TTY’s (_TeleTYpewriter_) in the same terminal window. You can run multiple _panes_ in a single window, easily configuring the layout and switching between them.

Installation

	brew install tmux

## Default keybindings & Functionality ##

The default keybindings for tmux are actually pretty intuitive, though if you’re used to screen you’ll likely get a little peeved with the default action binding of C-b, though this is easily changed to mimic screens behavior:

*NOTE* If you’re like me the Ctrl-b binding isn’t horribly intuitive especially if you’re used to screen. You can rebind this by putting the following in ~/.tmux.conf:

	set -g prefix Ctrl-a

- `Ctrl-b c` Create new window
- `Ctrl-b d` Detach current client
- `Ctrl-b l` Move to previously selected window
- `Ctrl-b n` Move to the next window
- `Ctrl-b p` Move to the previous window
- `Ctrl-b &` Kill the current window
- `Ctrl-b ,` Rename the current window
- `Ctrl-b %` Split the current window into two panes
- `Ctrl-b q` Show pane numbers (used to switch between panes)
- `Ctrl-b o` Switch to the next pane
- `Ctrl-b ?` List all keybindings

### Basic Pane Handling

One of the most powerful features tmux offers is the ability to split up your current window into “panes”. Anyone whose familiar with tiling windows managers will feel quite at home.

- `Ctrl-b %` (Split the window vertically)
- `Ctrl-b :` “split-window” (Split window horizontally)
- `Ctrl-b o` (Goto next pane)
- `Ctrl-b q` (Show pane numbers, when the numbers show up type the key to goto - that pane)
- `Ctrl-b {` (Move the current pane left)
- `Ctrl-b }` (Move the current pane right)

Now some obviously the default bindings don’t encompass some of features, such as splitting horizontally. I personally rebind the keys so `|` splits the current window vertically, and `-` splits it horizontally. Not the easiest things to type, though easy to remember.

You can achieve this by putting the following in `~/.tmux.conf` or by typing it in the interactive prompt (`Ctrl-b :`). Keep in mind if you do the latter it will only be in effect for that session:

	unbind %
	bind | split-window -h
	bind – split-window -v
