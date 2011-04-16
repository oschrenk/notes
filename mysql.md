# MySQL #

	sudo apt-get install mysql-server
	# Choose root password ...
	mysqladmin -u root -p create <database>
	
	mysql -u root -p
	
	CREATE USER username@localhost IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON database.* TO 'username'@'localhost' WITH GRANT OPTION;
	CREATE USER 'username'@'%' IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON database.* TO 'username'@'%' WITH GRANT OPTION;
	
	FLUSH PRIVILEGES;