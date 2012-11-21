# Vagrant #

Installation

	gem install vagrant

Official Ubuntu boxes

- [Ubuntu Lucid 32 Bit](http://files.vagrantup.com/lucid32.box)
- [Ubuntu Lucid 64 Bit](http://files.vagrantup.com/lucid64.box)
- [Ubuntu Precise 32 Bit](http://files.vagrantup.com/precise32.box)
- [Ubuntu Precise 64 Bit](http://files.vagrantup.com/precise64.box)

## Creating vagrant base box ##

Build Ubuntu 12.04 Server base box for Vagrant (with German Locale)

	git clone git://github.com/oschrenk/vagrant-ubuntu.git
	cd vagrant-ubuntu
	brew install cdrtools
	brew link cdrtools

Unfortunately the `tar` command supplied in OS X 10.7.4 has a bug
	
	$ tar --version
	bsdtar 2.8.3 - libarchive 2.8.3

It won't extract the file. libarchive has been updated to 3.0.4. homebrew doesn't support it directly, because its duplicating os behavior. So I created my own updated version of libarchive for 3.0.4. and submitted a pull request to the adamv/homebrew-dupes repository (which accepts duplicates). So:

	$ brew install https://raw.github.com/oschrenk/homebrew-dupes/edbc2b464d0bf4420508297418481178299f420a/libarchive.rb
	$ /usr/local/bin/bsdtar --version
	bsdtar 3.0.4 - libarchive 3.0.4
	$ /usr/local/bin/bsdtar -xvf ubuntu-12.04-alternate-i386.iso 
	sudo rm /usr/bin/tar
	cd /usr/bin
	sudo ln -s /usr/local/bin/bsdtar tar

Every day a new bug to squash, a new yak to shave

	./build.sh