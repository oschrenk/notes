# Bash #

## Startup ##

When bash starts it reads the following files every time you login. For the purposes of OS X, this means every time you open a new Terminal window.

  /etc/profile
  ~/.bash_profile
  ~/.bash_login   (if .bash_profile does not exist)
  ~/.profile      (if .bash_login does not exist)

When you start a new shell by typing bash on the command line, it reads .bashrc

Finally, OS X also uses `~/.MacOSX/environment.plist` to set more environment variables, including paths if necessary.

[#Folly:2009]

### about .bashrc, .bash_profile, .profile, /etc/profile, etc/bash.bashrc and others ##

> When bash is invoked as an interactive *login shell*, or as a non-interactive shell with the `--login` option, it first reads and executes commands from the file `/etc/profile`, if that file exists. After reading that file, it looks for `~/.bash_profile`, `~/.bash_login`, and `~/.profile`, in that order, and reads and executes commands from the first one that exists and is readable. The `--noprofile` option may be used when the shell is started to inhibit this behavior.
...
When an interactive shell that is *not a login shell* is started, bash reads and executes commands from `/etc/bash.bashrc` and `~/.bashrc`, if these files exist. This may be inhibited by using the `--norc` option. The `--rcfile` file option will force bash to read and execute commands from file instead of `/etc/bash.bashrc` and `~/.bashrc`.

- a *login shell* means a session where you log in to the system and directly end up in Bash, like a remote ssh session or logging in through a non-graphical text terminal
- a *non-login shell* is then the type of shells you open after logging in: typically in a graphical session when you open a new terminal window

So

- `.profile` is for things that are not specifically related to Bash, like environment variables, `PATH` and friends, and should be available anytime. For example, `.profile` should also be loaded when starting a graphical desktop session.
- `.bashrc` is for the configuring the interactive Bash usage, like Bash aliases, setting your favorite editor, setting the Bash prompt, etc.
- `.bash_profile` is for making sure that both the things in `.profile` and `.bashrc` are loaded for login shells. For example, `.bash_profile` could be something simple like

  . ~/.profile
  . ~/.bashrc

As stated in the man page excerpt above, if you would omit `.bash_profile`, only `.profile` would be loaded.

[#Lippens:2005]

## Shortcuts ##

*   `ctrl + a` Jump to beginning of line
*   `ctrl + e` Jump to end of line
*   `ctrl + r` Start search (just start typing), pressing multiple times toggles through occurrences in history
*   `ctrl + l` Clear terminal
*   `ctrl + x, ctrl +e` Fires up `$EDITOR` to finish a long command in your favorite editor

## Globbing ##

Wildcards in bash are referred to as pathname expansion. Pathname expansion is also sometimes referred to as **globbing**

Pathname expansion that is done by the shell (in this case bash) and not by the operating system or by the program that is being run.2

Pathname expansion "expands" the `*`, `?`, and `[...]` syntaxes when you type them as part of a command, for example:

	$ ls *.jpg         # List all JPEG files
	$ ls ?.jpg         # List JPEG files with 1 char names (eg a.jpg, 1.jpg)
	$ rm [A-Z]*.jpg    # Remove JPEG files that start with a capital letter

## Bash Completion ##

With Bash completion enabled, the default is to complete the current path to the longest unique string it can on the first press of the Tab key, and then show a list of all the possible completions if the Tab key is then pressed a second time.

## Output Streams ##

*   `>` redirects *StdOut*, for example to a file `command > path` or to `/dev/null` (discards all data)
*   `>>` appends the output to the path `command >> path`

Similar for *StdErr*, but use `2` prefix like so `2>` or respectively. `2>>`.

## Options ##

- `shopt -s autocd` Since 4.0-alpha. If set, a command name that is the name of a directory is executed as if it were the argument to the cd command.
- `shopt -s cdspell ` Correct minor errors in the spelling of a directory component in a cd command
- `shopt -s cmdhist` Save multi-line commands in history as single line
- `shopt -s dirspell` Since 4.0-alpha. Bash will perform spelling corrections on directory names to match a glob.
- `shopt -s globstar` Since 4.0-alpha. Recursive globbing with `**` is enabled
- `shopt -s histappend` If set, the history list is appended to the file named by the value of the `HISTFILE` variable when the shell exits, rather than overwriting the file
- `shopt -s nocaseglob` When typing part of a filename and press Tab to autocomplete, Bash does a case-insensitive search.

## OSX ##

### Upgrade to Bash 4 ###

	# Install Bash 4 using homebrew
	brew install bash
	# Add the new shell to the list of legit shells
	sudo bash -c "echo /usr/local/bin/bash >> /private/etc/shells"
	# Change the shell for the user
	chsh -s /usr/local/bin/bash
	# Restart terminal.app (new window works too)
	# Check for Bash 4 and /usr/local/bin/bash...
	echo $BASH && echo $BASH_VERSION
	
## Examples ##

### Using the history ###

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

## .inputrc ##

Next to `.profile` there is also `.inputrc`. This is the configuration file for `GNU` Readline (a library that allows to edit commands as they are typed in)

**An exemplary scenario** is when you have to login to multiple servers using `ssh ...` commands. Instead of toggling through all the occurrences in the history you can just start typing away @ssh @.

With the following snippet in the `.inputrc` file you can then just use your arrow keys to scroll through all the occurrences starting with the prefix you have typed in so far. This is especially useful when multiple logins share the same user but have different hosts.

   	# By default up/down are bound to previous-history
   	# and next-history respectively. The following does the
   	# same but gives the extra functionality where if you
   	# type any text (or more accurately, if there is any text
   	# between the start of the line and the cursor),
   	# the subset of the history starting with that text
   	# is searched (like 4dos for e.g.).
   	# Note to get rid of a line just Ctrl-C
   	"\e[A": history-search-backward
   	"\e[B": history-search-forward

It doesn’t stop there. If you like to move around in your command more quickly you might find use in the following snippet, which allows to jump over words in your command using `Strg` and the left or right arrow key.

   	# move around word for word with Strg + arrow keys
   	"\e[5C": forward-word
   	"\e[5D": backward-word
   	"\e\e[C": forward-word
   	"\e\e[D": backward-word

This one is very helpful when you just type away and realize later that the first character of is upper case instead of lower case (the default directories in the home directory of `OSX` all start with upper case letters).

   	# case insensitivity for tab-completion
   	set completion-ignore-case On

## FAQ/Problems

### Bash doesn’t update the PATH; changes to .profile aren’t registered

Adding something like `export PATH=/usr/bin:$PATH` doesn’t update the `$PATH` environment variable. The reason is that Bash tries to find local profile files in the following order:

    # ~/.bash_profile
    # ~/.bash_login
    # ~/.profile

`~/.profile` is the last file in the list. So if OS X finds a file it stops processing. If an installer puts a `.bash_login` in your home directory your `.profile` wouldn’t be read.

### Reload .profile

    $ . ~/.profile

[#Folly:2009]: Steve Folly. [Where does $PATH get set in OS X 10.6 Snow Leopard?](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard#69190) Stackoverflow, 2009
[#Lippens:2005]: Stefan Lippens. [about .bashrc, .bash_profile, .profile, /etc/profile, etc/bash.bashrc and others](http://stefaanlippens.net/bashrc_and_others). Personal Blog, 2005.