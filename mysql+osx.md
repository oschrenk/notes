# MySQL on OSX #

## Install ##

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
	    cp /usr/local/Cellar/mysql/5.5.20/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

	* if this is an upgrade and you already have the homebrew.mxcl.mysql.plist loaded:
	    launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
	    cp /usr/local/Cellar/mysql/5.5.20/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
	    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

	You may also need to edit the plist to use the correct "UserName".
	
Following the instructions	

	$ unset TMPDIR
	$ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
	
	To start mysqld at boot time you have to copy
	support-files/mysql.server to the right place for your system

	PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !
	To do so, start the server, then issue the following commands:

	/usr/local/Cellar/mysql/5.5.20/bin/mysqladmin -u root password 'new-password'
	/usr/local/Cellar/mysql/5.5.20/bin/mysqladmin -u root -h schmendrick.local password 'new-password'

	Alternatively you can run:
	/usr/local/Cellar/mysql/5.5.20/bin/mysql_secure_installation

	which will also give you the option of removing the test
	databases and anonymous user created by default.  This is
	strongly recommended for production servers.

	See the manual for more instructions.

	You can start the MySQL daemon with:
	cd /usr/local/Cellar/mysql/5.5.20 ; /usr/local/Cellar/mysql/5.5.20/bin/mysqld_safe &

	You can test the MySQL daemon with mysql-test-run.pl
	cd /usr/local/Cellar/mysql/5.5.20/mysql-test ; perl mysql-test-run.pl

	Please report any problems with the /usr/local/Cellar/mysql/5.5.20/scripts/mysqlbug script!
	
Again we do as told

	$ mysql.server start
	$ /usr/local/Cellar/mysql/5.5.20/bin/mysql_secure_installation

## Uninstall ##

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
