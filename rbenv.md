# rbenv #

	brew install rbenv
	brew install ruby-build

Add rbenv to your path

	export PATH=~/.rbenv/shims:$PATH

Rubies don't build with Clang

	env CC=/usr/bin/gcc rbenv install 1.9.3-p385

You have to rehash when installing binaries

	rbenv rehash

Set

	rbenv global 1.9.3-p385