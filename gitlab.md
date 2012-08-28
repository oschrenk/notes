# Gitlab 

## Virtual Machine ##

Download the virtual machine

    wget http://dl.dropbox.com/u/71600828/GITLAB.ova

and import it into VirtualBox.

## Gitlab w/ Chef & Vagrant ##

### Prerequisites

1. Install [Virtual Box](https://www.virtualbox.org). Downloads can be found [here](https://www.virtualbox.org/wiki/Downloads)
2. You need at least [Ruby](http://www.ruby-lang.org/) 1.9 Install it by either running `brew install ruby` or by [downloading](http://rubyinstaller.org/) and running the appropiate installer for your operating system.
3. Install [Vagrant](http://vagrantup.com) by either running `gem install vagrant` or by [downloading](http://downloads.vagrantup.com/) and running the appropriate installer for your operating system

If you have problems installing or running vagrant you should consult the [official documentation](http://vagrantup.com/v1/docs/index.html).

### Usage

    vagrant box add precise32 http://files.vagrantup.com/precise32.box

or if you prefer a german locale for the system

    vagrant box add precise32 http://dl.dropbox.com/u/155311/precise32-de_DE.box

Clone repository

    git clone git://github.com/oschrenk/gitlab.git
    cd gitlab

Init and update the submodules

    git submodule init
    git submodule update

Start up vagrant
  
    vagrant up

Connect to vagrant machine and start rails process

    vagrant ssh

    cd /vagrant/gitlabhq
    bundle exec thin start -e production

On your local machine, update `/etc/hosts` and add:

    sudo bash -c 'echo "33.33.33.20 gitlab.local" >> /etc/hosts'

Open the site in your favorite browser or if you're using OS X, just run

    open http://gitlab.local

Login with:

    Email:    admin@local.host
    Password: 5iveL!fe