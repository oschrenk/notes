# zsh #

Install

	brew install zsh

Installation notes

	To use this build of Zsh as your login shell, add it to /etc/shells.

	If you have administrator privileges, you must fix an Apple miss
	configuration in Mac OS X 10.7 Lion by renaming /etc/zshenv to
	/etc/zprofile, or Zsh will have the wrong PATH when executed
	non-interactively by scripts.

	Alternatively, install Zsh with /etc disabled:

	  brew install --disable-etcdir zsh

Set as shell

	chsh -s /usr/local/bin/zsh