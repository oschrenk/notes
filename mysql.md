# MySQL #

## Install

Unix

	sudo apt-get install mysql-server
	# Choose root password ...
	mysqladmin -u root -p create <database>

	mysql -u root -p

OS X

	$ brew install mysql
	$ unset TMPDIR
    $ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
	$ mysql.server start
    $ /usr/local/opt/mysql/bin/mysql_secure_installation

## Usage ##

Dump the database

	mysqldump -u <username> -p -h localhost <database> > file.sql

Import into local devel database

	mysql -u root <database> < file.sql
