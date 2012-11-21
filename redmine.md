# Redmine #

> [Redmine](http://www.redmine.org/) is a flexible project management web application. Written using the Ruby on Rails framework, it is cross-platform and cross-database.

## Virtual Machine ##

- [Redmine](http://www.turnkeylinux.org/redmine) on [TurnkeyLinux](http://www.turnkeylinux.org/) 

Redmine configurations
- Installed from upstream source code to `/var/www/redmine`
- Supports Git, Bazaar, Mercurial and Subversion.
- Includes exemplary helloworld repositories.
- Loaded default roles, trackers, statuses, workflows and enumerations.
- Configured projects to use all available trackers (bug, feature, support).

## Installation ##

- [Installation Guide for Ubuntu](http://www.redmine.org/projects/redmine/wiki/HowTo_Install_Redmine_in_Ubuntu)

Installing the latest Redmine

	sudo add-apt-repository ppa:ondrej/redmine
	sudo apt-get update
	sudo apt-get install redmine redmine-mysql

## Configuration ##

Symlink `/usr/share/redmine/public` to your desired web-accessible location. E.g.:

	$ sudo ln -s /usr/share/redmine/public /var/www/redmine

By default, passenger runs as `nobody`, so you'll need to fix that. In `/etc/apache2/mods-available/passenger.conf`, add:

	PassengerDefaultUser www-data

You'll also need to configure the `/var/www/redmine` location in `/etc/apache2/sites-available/default` by adding:

	<Directory /var/www/redmine>
	RailsBaseURI /redmine
	PassengerResolveSymlinksInDocumentRoot on
	</Directory>

If you set your AppArmor mysqld profile to complain you ought to set it back to enforce:

	$ sudo aa-enforce /usr/sbin/mysqld

Enable passenger:

	$ sudo a2enmod passenger

Restart apache2

	$ sudo service apache2 restart

and you should be able to access Redmine at: [http://redmine.server.ip.address/redmine](http://redmine.server.ip.address/redmine)

If you receive a `403: Forbidden` error after setting up Redmine, the Redmine 'public' folder may have incorrect permissions set. The executable bit on the public folder must be enabled or you will receive a `403: Forbidden` error when attempting to access Redmine.

	$ sudo chmod a+x /usr/share/redmine/public

## Plugins ##

- [Redmine Git Hosting Plugin](https://github.com/kubitron/redmine_git_hosting)