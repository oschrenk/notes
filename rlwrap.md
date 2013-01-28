# rlwrap #

Some REPLs have no support for Readline. That means that if you enter an Up arrow, you get garbage output like this:

	$ sml
	Standard ML of New Jersey v110.75 [built: Fri Jan 11 10:32:43 2013]
	- ^[[A

Simply install rlwrap via your operating system’s package manager (or manually), and then invoke the REPL via `rlwrap`:

	$ rlwrap sml

At this point, even if you do nothing else, the REPL is now at least able to handle Up and Down arrows to scroll through history, and you can use basic Readline commands like `Ctrl-a` and `Ctrl-e` to go to the start and end of the line respectively, `Ctrl-u` to delete back to the prompt (and save the deleted material), `Ctrl-y` to paste saved text back into the prompt and so on. One other nice thing is that `rlwrap` saves history per command across invocations.

## Autocompletion ##

When you invoke rlwrap, you can feed it a file filled with additional completions. rlwrap reads the file before starting, splits the file into words and then supports Tab-completion for all those additional words. For example, if you create a file with all of SML’s keywords, then you can load those into rlwrap when you start SML’s REPL:

	rlwrap -f keywords sml

To autoload these kind of files

	mkdir ~/.rlwrap

Added to your `.bashrc`

	export RLWRAP_HOME="$HOME/.rlwrap"

Now simply save your keywords file in that directory as `$RLWRAP_HOME/.<command>_completions`. `rlwrap` will now automagically load those completions every time you call `rlwrap` command

[arpinum.org, Learn Yourself a Better REPL for Great Good!](http://ithaca.arpinum.org/2013/01/20/rlwrap.html)