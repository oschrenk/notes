# MySQL #

## Install

	sudo apt-get install mysql-server
	# Choose root password ...
	mysqladmin -u root -p create <database>
	
	mysql -u root -p

### On OS X

		$ brew install mysql
	
	Set up databases to run AS YOUR USER ACCOUNT with:
    unset TMPDIR
    mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

	To set up base tables in another folder, or use a different user to run
	mysqld, view the help for mysqld_install_db:
	    mysql_install_db --help

	and view the MySQL documentation:
	  * http://dev.mysql.com/doc/refman/5.5/en/mysql-install-db.html
	  * http://dev.mysql.com/doc/refman/5.5/en/default-privileges.html

	To run as, for instance, user "mysql", you may need to `sudo`:
	    sudo mysql_install_db ...options...

	Start mysqld manually with:
	    mysql.server start

	    Note: if this fails, you probably forgot to run the first two steps up above

	A "/etc/my.cnf" from another install may interfere with a Homebrew-built
	server starting up correctly.

	To connect:
	    mysql -uroot

	To launch on startup:
	* if this is your first install:
	    mkdir -p ~/Library/LaunchAgents
	    cp /usr/local/Cellar/mysql/5.5.25a/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

	* if this is an upgrade and you already have the homebrew.mxcl.mysql.plist loaded:
	    launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
	    cp /usr/local/Cellar/mysql/5.5.25a/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

	You may also need to edit the plist to use the correct "UserName".
	
Again we do as told

	$ mysql.server start
	$ /usr/local/Cellar/mysql/5.5.2a/bin/mysql_secure_installation

## Settings ##

	CREATE USER username@localhost IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON database.* TO 'username'@'localhost' WITH GRANT OPTION;
	CREATE USER 'username'@'%' IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON database.* TO 'username'@'%' WITH GRANT OPTION;
	
	FLUSH PRIVILEGES;

## Backup ##

Dump the database

	mysqldump -u <username> -p -h localhost <database> > file.sql

Import into local devel database

	mysql -u root <database> < file.sql

## Unistall ##

On OSX

From [forums.mysql](http://forums.mysql.com/read.php?11,71860,72130#msg-72130)

*   Main directory folder at `/usr/local/mysql` (maybe with a version
    suffix) which contains all the subdirectories. If it contains a
    version suffix there will also be a soft link called mysql that
    points to the actual directory folder.
*   A folder at `/Library/StartupItems/MySQLCOM`
*   A preference pane in `/Library/PreferencePanes/` as well and that
    is all there is to it.
*   Also if a `.pkg` install you will have an entry in
    `/Library/Receipts` as well that you can remove.

All locations need to be deleted as root, so you will need to use sudo
to do it.  
If you are not used to doing this, then post again after checking the
above four locations to make sure that's where everything is
installed. That way some one can give you the exact commands needed to
delete the folders.

*  An entry in `/etc/hostconfig` file `MYSQLCOM=-YES-` which you can change to `MSQLCOM=-NO-` or delete the line. Again you will need to use sudo to do this as well.
