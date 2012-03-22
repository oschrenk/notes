# Emacs #

## Installation ##

On Lion

	$ brew install emacs --cocoa --use-git-head --HEAD
	[...]
	Emacs.app was installed to:
	  /usr/local/Cellar/emacs/HEAD

	Command-line emacs can be used by setting up an alias:
	  alias emacs="/usr/local/Cellar/emacs/HEAD/Emacs.app/Contents/MacOS/Emacs -nw"

	 To link the application to a normal Mac OS X location:
	   brew linkapps
	 or:
	   ln -s /usr/local/Cellar/emacs/HEAD/Emacs.app /Applications

	Because the official bazaar repository might be slow, we include an option for
	pulling HEAD from an unofficial Git mirror:

	  brew install emacs --HEAD --use-git-head

	There is inevitably some lag between checkins made to the official Emacs bazaar
	repository and their appearance on the Savannah mirror. See
	http://git.savannah.gnu.org/cgit/emacs.git for the mirror's status. The Emacs
	devs do not provide support for the git mirror, and they might reject bug
	reports filed with git version information. Use it at your own risk.
	

	$ cp -r /usr/local/Cellar/emacs/HEAD/Emacs.app /Applications/


## Configuration ##

	$ git clone git://github.com/bbatsov/emacs-prelude.git path/to/local/repo
	$ ln -s path/to/local/repo ~/.emacs.d

[#Batsov:2011]

[#Batsov:2011]: Bozhidar Ivanov Batsov, [Getting Started With Emacs 24](http://batsov.com/articles/2011/10/09/getting-started-with-emacs-24/), 2011