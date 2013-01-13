# Gitlab

[Installation](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md)

## Development VM w/ Chef & Vagrant ##

The preferred installation method for a virtual environment is using Chef & Vagrant.

### Prerequisites

1. Install [Virtual Box](https://www.virtualbox.org). Downloads can be found [here](https://www.virtualbox.org/wiki/Downloads)
2. Install [Vagrant](http://vagrantup.com) by either running `gem install vagrant` or by [downloading](http://downloads.vagrantup.com/) and running the appropriate installer for your operating system
3. Install [Bundler](http://gembundler.com/) by running `$ gem install bundler`

If you have problems installing or running vagrant you should consult the [official documentation](http://vagrantup.com/v1/docs/index.html).

### Recipe

I had to switch to my admin account for many purposes as I normally work with a user with very little permissions.

    git clone https://github.com/gitlabhq/gitlab-vagrant-vm
    cd gitlab-vagrant-vm
    bundle install
    bundle exec librarian-chef install

Then spin up the machine (it will download and install on first start). As this process will alter `/etc/exports` you will need admin permissions.

    vagrant up

Run tests

    vagrant ssh
    cd /vagrant/gitlabhq/
    bundle exec rake gitlab:test

### Information

- Virtual Machine IP: `192.168.3.14`
- User/password: `vagrant/vagrant`
- MySQL user/password: `vagrant/Vagrant`
- MySQL root password: `nonrandompasswordsaregreattoo`
