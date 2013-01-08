# Bash #

Bash is a Unix shell written by Brian Fox for the GNU Project.

## Configuration ##

When bash starts it reads the following files every time you login. For the purposes of OS X, this means every time you open a new Terminal window.

	/etc/profile
	~/.bash_profile
	~/.bash_login   (if .bash_profile does not exist)
	~/.profile      (if .bash_login does not exist)

When you start a new shell by typing bash on the command line, it reads `.bashrc`.

Finally, OS X also uses `~/.MacOSX/environment.plist` to set more environment variables, including paths if necessary.

[#Folly:2009]

### A more detailed look at .bashrc, .bash_profile, .profile, /etc/profile, etc/bash.bashrc and others ##

A good graphic graphic can be found [here](http://www.solipsys.co.uk/new/BashInitialisationFiles.html)

> When bash is invoked as an interactive *login shell*, or as a non-interactive shell with the `--login` option, it first reads and executes commands from the file `/etc/profile`, if that file exists. After reading that file, it looks for `~/.bash_profile`, `~/.bash_login`, and `~/.profile`, in that order, and reads and executes commands from the first one that exists and is readable. The `--noprofile` option may be used when the shell is started to inhibit this behavior.
...
When an interactive shell that is *not a login shell* is started, bash reads and executes commands from `/etc/bash.bashrc` and `~/.bashrc`, if these files exist. This may be inhibited by using the `--norc` option. The `--rcfile` file option will force bash to read and execute commands from file instead of `/etc/bash.bashrc` and `~/.bashrc`.

- a *login shell* means a session where you log in to the system and directly end up in Bash, like a remote ssh session or logging in through a non-graphical text terminal
- a *non-login shell* is then the type of shells you open after logging in: typically in a graphical session when you open a new terminal window

So

`.profile` is for things that are not specifically related to Bash, like environment variables, `PATH` and friends, and should be available anytime. For example, `.profile` should also be loaded when starting a graphical desktop session.

`.bashrc` is for the configuring the interactive Bash usage, like Bash aliases, setting your favorite editor, setting the Bash prompt, etc.

`.bash_profile` is for making sure that both the things in `.profile` and `.bashrc` are loaded for login shells. For example, `.bash_profile` could be something simple like

	. ~/.profile
	. ~/.bashrc

As stated in the man page excerpt above, if you would omit `.bash_profile`, only `.profile` would be loaded.

[#Lippens:2005]

### Options ###

- `shopt -s autocd` Since 4.0-alpha. If set, a command name that is the name of a directory is executed as if it were the argument to the cd command.
- `shopt -s cdspell ` Correct minor errors in the spelling of a directory component in a cd command
- `shopt -s cmdhist` Save multi-line commands in history as single line
- `shopt -s dirspell` Since 4.0-alpha. Bash will perform spelling corrections on directory names to match a glob.
- `shopt -s globstar` Since 4.0-alpha. Recursive globbing with `**` is enabled
- `shopt -s histappend` If set, the history list is appended to the file named by the value of the `HISTFILE` variable when the shell exits, rather than overwriting the file
- `shopt -s nocaseglob` When typing part of a filename and press Tab to autocomplete, Bash does a case-insensitive search.

### History ###

Why use your brain to remember all the commands you have to use again and again. Just rely on your terminal and the history file it keeps.

Bash supports `Ctrl + r` to search the history. Per default the size history (and therefore the number of entries) is rather limited. You can do that by setting some variables in your `.profile` or (`.bash_profile`)

**WARNING**:OSX prefers the `.profile` to `.bash_profile`. If both exists **only** `.profile` will be read (see [.profile not read][1])

    #.bash_profile or .profile
    export HISTFILESIZE=1000000000      # sets history size to something really big
    export HISTSIZE=1000000             # sets history size to something really big

You can also set to ignore duplicates of commands (`ls`, `ps -ef` are such candidates)

    #.bash_profile or .profile
    export HISTCONTROL=ignoreboth

Other useful settings are

    # append to the same history file when using multiple terminals
    shopt -qs histappend

    # when typing part of a filename and press Tab to autocomplete, Bash does a case-insensitive search.
    shopt -qs nocaseglob

    # minor errors in the spelling of a directory component in a cd command will be corrected
    shopt -qs cdspell

## Usage ##

### Tab completion ###

With Bash completion enabled, the default is to complete the current path to the longest unique string it can on the first press of the Tab key, and then show a list of all the possible completions if the Tab key is then pressed a second time.

### Keyboard shortcuts ###

The following shortcuts work when using default (Emacs) key bindings. Vi-bindings can be enabled by running `set -o vi`.

- `Tab` Autocompletes from the cursor position.
- **`Ctrl + a`** Moves the cursor to the line start
- `Ctrl + b` Moves the cursor back one character (equivalent to the key `←`).
- **`Ctrl + c`** Sends the signal SIGINT to the current task, which aborts and closes it.
- `Ctrl + d` Sends an EOF marker, which (unless disabled by an option) closes the current shell (equivalent to the command exit). (Only if there is no text on the current line). If there is text on the current line, deletes the current character (then equivalent to the key `Delete`).
- **`Ctrl + e`** Moves the cursor to the line end (equivalent to the key End).
- `Ctrl + f` : Moves the cursor forward one character (equivalent to the key `→`).
- **`Ctrl + r`** Start search (just start typing), pressing multiple times toggles through occurrences in history
- **`Ctrl + l`** Clears the screen content

- `Ctrl + g` Abort the research and restore the original line.
- `Ctrl + h` Deletes the previous character (same as backspace).
- `Ctrl + i` Equivalent to the tab key.
- `Ctrl + j` Equivalent to the enter key.
- `Ctrl + k` Clears the line content after the cursor and copies it into the clipboard.
- `Ctrl + l` Clears the screen content (equivalent to the command clear).
- `Ctrl + n` (next) recalls the next command (equivalent to the key ↓).
- `Ctrl + o` Executes the found command from history, and fetch the next line relative to the current line from the history for editing.
- `Ctrl + p` (previous) recalls the prior command (equivalent to the key ↑).
- `Ctrl + r` (research) recalls the last command including the specified character(s). A second `Ctrl + r` recalls the next anterior command which corresponds to the research
- `Ctrl + s` Go back to the next more recent command of the research (beware to not execute it from a terminal because this command also launches its XOFF). If you changed that XOFF setting, use `Ctrl + q` to return.
- `Ctrl + t` Transpose the previous two characters.
- `Ctrl + u` Clears the line content before the cursor and copies it into the clipboard.
- `Ctrl + v` If the next input is also a control sequence, type it literally (e. g. `Ctrl + v, Ctrl+h` types `^H`, a literal backspace.)
- `Ctrl + w` Clears the word before the cursor and copies it into the clipboard.
- `Ctrl + x, Ctrl + e` Edits the current line in the `$EDITOR` program, or vi if undefined.
- `Ctrl + x, Ctrl + r` Read in the contents of the inputrc file, and incorporate any bindings or variable assignments found there.
- `Ctrl + x, Ctrl + u` Incremental undo, separately remembered for each line.
- `Ctrl + x, Ctrl + v` Display version information about the current instance of bash.
- `Ctrl + x, Ctrl + x` Alternates the cursor with its old position. (`C-x`, because `x` has a crossing shape).
- `Ctrl + y` (yank) adds the clipboard content from the cursor position.
- `Ctrl + z` Sends the signal `SIGTSTP` to the current task, which suspends it. To execute it in background one can enter bg. To bring it back from background or suspension fg ['process name or job id'] (foreground) can be issued.
- `Ctrl + _` Incremental undo, separately remembered for each line.
- `Alt + b` (backward) moves the cursor backward one word.
- `Alt + c` Capitalizes the character under the cursor and moves to the end of the word.
- `Alt + d` Cuts the word after the cursor.
- `Alt + f` (forward) moves the cursor forward one word.
- `Alt + l` Lowers the case of every character from the cursor's position to the end of the current word.
- `Alt + r` Cancels the changes and puts back the line as it was in the history.
- `Alt + u` Capitalizes every character from the cursor's position to the end of the current word.
- `Alt + .` Insert the last argument to the previous command (the last word of the previous history entry).

## FAQ/Problems

### OSX, Upgrade to Bash 4 ###

	# Install Bash 4 using homebrew
	brew install bash
	# Add the new shell to the list of legit shells
	sudo bash -c "echo /usr/local/bin/bash >> /private/etc/shells"
	# Change the shell for the user
	chsh -s /usr/local/bin/bash
	# Restart terminal.app (new window works too)
	# Check for Bash 4 and /usr/local/bin/bash...
	echo $BASH && echo $BASH_VERSION

### Bash doesn’t update the PATH; changes to .profile aren’t registered ###

Situation: Adding something like `export PATH=/usr/bin:$PATH` doesn’t update the `$PATH` environment variable.

The reason is that Bash tries to find local profile files in the following order:

    # ~/.bash_profile
    # ~/.bash_login
    # ~/.profile

`~/.profile` is the last file in the list. So if OS X finds a file it stops processing. If an installer puts a `.bash_login` in your home directory your `.profile` wouldn’t be read.

## Bibliography ##

[#Folly:2009]: Steve Folly. [Where does $PATH get set in OS X 10.6 Snow Leopard?](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard#69190) Stackoverflow, 2009

[#Lippens:2005]: Stefan Lippens. [about .bashrc, .bash_profile, .profile, /etc/profile, etc/bash.bashrc and others](http://stefaanlippens.net/bashrc_and_others). Personal Blog, 2005.
