# Redmine + Git #

- [Redmine Git Hosting Plugin](https://github.com/kubitron/redmine_git_hosting)

## Installation ##

### Redmine ###

The plugin I intend to use only works for Redmine 1.3 and Gitolite 2, so the installation instruction differ from the latest Redmine (the same goes for Gitolite).

- [Redmine 1.3 Installation Guide for Ubuntu 10.04](http://www.redmine.org/projects/redmine/wiki/HowToInstallRedmineOnUbuntuServer#Redmine-Installation)

Update System

	$ sudo apt-get update
	$ sudo apt-get upgrade

Install LAMP

	$ sudo tasksel install lamp-server

Install dependencies

	$ sudo apt-get install build-essential subversion git git-svn libmysqlclient-dev libdigest-sha-perl

Install ruby via rvm

	$ sudo apt-get install ruby-rvm
	$ rvm install 1.8.7
	$ rvm --default use 1.8.7
	$ rvm gem install rails

Download Redmine into `/user/share/redmine` directory

	$ sudo svn co http://redmine.rubyforge.org/svn/branches/1.3-stable /usr/share/redmine

Create an empty MySQL database and accompanying user named redmine for example.

	$ mysql -u root -p
	(enter the mysql root user password)
	> create database redmine character set utf8;
	> create user 'redmine'@'localhost' identified by '[password]';
	> grant all privileges on redmine.* to 'redmine'@'localhost' identified by '[password]';
	> exit

Copy `config/database.yml.example` to `config/database.yml` and edit this file in order to configure your database settings for "production" environment.

	$ sudo cp /usr/share/redmine/config/database.yml.example /usr/share/redmine/config/database.yml

	$ sudo nano /usr/share/redmine/config/database.yml

	Modify to the following and save (ctrl+x)

	production:
	  adapter: mysql
	  socket: /var/run/mysqld/mysqld.sock
	  database: redmine
	  host: localhost
	  username: redmine
	  password: [password]
	  encoding: utf8

Generate a session store secret.

	$ cd /usr/share/redmine
	$ sudo rake generate_session_store

Create the database structure, by running the following command under the application root directory:

	$ cd /usr/share/redmine
	$ sudo rake db:migrate RAILS_ENV="production"
