# RVM #

[RVM](https://rvm.io/) is a Ruby environment manager. Youn can easily switxh between various versions of ruby and asocciated sets of gems with it.

Install latest stable release via

	mkdir .rvm
	curl -L get.rvm.io | bash -s stable

Source `rvm` script in your profile. I added the following to my `.profile`

	# rmv ruby environment manager
	if [ -f ~/.rvm/scripts/rvm ]; then
	  source ~/.rvm/scripts/rvm
	fi

Usage

 - list known rubies `rvm list known`
 - install a ruby version `rvm install 1.9.3`
 - usa a ruby version `rvm use 1.9.3`

Optionally, you can set a version of Ruby to use as the default for new shells. Note that this overrides the 'system' ruby:

	user$ rvm use 1.9.2 --default

To use an RVM installed Ruby as default, instead of the system ruby:

    rvm install 1.8.7 # installs patch 357: closest supported version
    rvm system ; rvm gemset export system.gems ; rvm 1.8.7 ; rvm gemset import system.gems # migrate your gems
    rvm alias create default 1.8.7

And reopen your terminal windows.

A nice introduction on Ruby, RVM and Rails on OSX can be found [here](http://www.asconix.com/howtos/mac-os-x/ruby-rails-rvm-mac-os-x-howto).
