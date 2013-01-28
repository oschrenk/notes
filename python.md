# Python #

Homebrew's python is newer and comes with pip, so

	brew install python

	Distribute and Pip have been installed. To update them
	  pip install --upgrade distribute
	  pip install --upgrade pip

	To symlink "Idle" and the "Python Launcher" to ~/Applications
	  `brew linkapps`

	You can install Python packages with (the outdated easy_install or)
	  `pip install <your_favorite_package>`

	They will install into the site-package directory
	  /usr/local/lib/python2.7/site-packages
	Executable python scripts will be put in:
	  /usr/local/share/python
	so you may want to put "/usr/local/share/python" in your PATH, too.

	See: https://github.com/mxcl/homebrew/wiki/Homebrew-and-Python
