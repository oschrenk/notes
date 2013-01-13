# Tomcat #

## Installation ##

	apt-get update
	apt-get upgrade --show-upgraded
	apt-get install tomcat6
	apt-get install tomcat6-docs tomcat6-examples tomcat6-admin

	/etc/init.d/tomcat6 start
	/etc/init.d/tomcat6 stop
	/etc/init.d/tomcat6 restart

You can find the webapps directory under

	/usr/share/tomcat6/webapps

To gain access to the admin application you first have to add some roles/user privileges to the `tomcat-users.xml`

Add the following roles

	<role rolename="manager"/>
	<role rolename="admin"/>
	<user username="jdoe" password="complicatedpass" roles="manager,admin"/>

And restart the server via

	/etc/init.d/tomcat6 restart

Now you can access the manager on [localhost:8080/manager/html](localhost:8080/manager/html)
