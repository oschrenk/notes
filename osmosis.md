# Osmosis #

- [openstreetmap / osmosis](https://github.com/openstreetmap/osmosis) Github repository

Install via

	$ brew install osmosis

## Import planet.osm.bz2 into a MySQL Database ##

Install MySQL

	$ brew install mysql
	$ unset TMPDIR
    $ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
	$ mysql.server start
    $ /usr/local/opt/mysql/bin/mysql_secure_installation

There is no official support for MySQL so we have to create schema by hand. Fortunately somebody did this

	$ wget http://gweb.bretth.com/apidb06-mysql-latest.sql

We follow the naming scheme in the script and create database and user accordingly

	$ mysql -u root -p
	mysql> CREATE DATABASE api06_test;
	mysql> GRANT ALL PRIVILEGES ON api06_test.* TO osm@localhost IDENTIFIED BY 'osm';
	mysql > exit;

Create the schema using the script

	mysql -u osm -p -h localhost api06_test < apidb06-mysql-latest.sql

Read data and populate database

	osmosis --read-xml file="bremen.osm.bz2" --write-apidb-0.6 host="127.0.0.1" dbType="mysql" database="api06_test" user="osm" password="osm" validateSchemaVersion=no

## Problems ##

### Task type write-mysql doesn't exist. ###

I was tring to execute

	osmosis --read-xml file="bremen.osm.bz2" --write-mysql host="localhost" database="osm" user="root"

I got

	SEVERE: Execution aborted.
	org.openstreetmap.osmosis.core.OsmosisRuntimeException: Task type write-mysql doesn't exist.
		at org.openstreetmap.osmosis.core.pipeline.common.TaskManagerFactoryRegister.getInstance(TaskManagerFactoryRegister.java:60)
		at org.openstreetmap.osmosis.core.pipeline.common.Pipeline.buildTasks(Pipeline.java:50)
		at org.openstreetmap.osmosis.core.pipeline.common.Pipeline.prepare(Pipeline.java:112)
		at org.openstreetmap.osmosis.core.Osmosis.run(Osmosis.java:86)
		at org.openstreetmap.osmosis.core.Osmosis.main(Osmosis.java:37)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:601)
		at org.codehaus.plexus.classworlds.launcher.Launcher.launchStandard(Launcher.java:329)
		at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:239)
		at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:409)
		at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:352)
		at org.codehaus.classworlds.Launcher.main(Launcher.java:47)

The [documentation](http://wiki.openstreetmap.org/wiki/Osmosis/Detailed_Usage) is weird. All goals listed are defaulting to a specific API version of OpenStreetMap, which is `0.6` for `osmosis` `0.4.1`. This version does not exist for the goal `--write-mysql` but is only available for `0.5`

You have to use `write-apidb` goal instead.

### org.openstreetmap.osmosis.core.OsmosisRuntimeException: Unable to create resultset. OR com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'osm.schema_migrations ###

I tried

	osmosis --read-xml file="bremen.osm.bz2" --write-apidb-0.6 host="127.0.0.1" dbType="mysql" database="osm" user="osm" password="osm"

and got

	org.openstreetmap.osmosis.core.OsmosisRuntimeException: Unable to create resultset.
	at org.openstreetmap.osmosis.apidb.common.DatabaseContext.executeQuery(DatabaseContext.java:429)
	at org.openstreetmap.osmosis.apidb.v0_6.impl.SchemaVersionValidator.validateDBVersion(SchemaVersionValidator.java:82)
	at org.openstreetmap.osmosis.apidb.v0_6.impl.SchemaVersionValidator.validateVersion(SchemaVersionValidator.java:55)
	at org.openstreetmap.osmosis.apidb.v0_6.ApidbWriter.initialize(ApidbWriter.java:324)
	at org.openstreetmap.osmosis.apidb.v0_6.ApidbWriter.process(ApidbWriter.java:1089)
	at org.openstreetmap.osmosis.xml.v0_6.impl.NodeElementProcessor.end(NodeElementProcessor.java:139)
	at org.openstreetmap.osmosis.xml.v0_6.impl.OsmHandler.endElement(OsmHandler.java:107)
	at org.apache.xerces.parsers.AbstractSAXParser.endElement(Unknown Source)
	at org.apache.xerces.parsers.AbstractXMLDocumentParser.emptyElement(Unknown Source)
	at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl.scanStartElement(Unknown Source)
	at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl$FragmentContentDispatcher.dispatch(Unknown Source)
	at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl.scanDocument(Unknown Source)
	at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
	at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
	at org.apache.xerces.parsers.XMLParser.parse(Unknown Source)
	at org.apache.xerces.parsers.AbstractSAXParser.parse(Unknown Source)
	at org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser.parse(Unknown Source)
	at org.apache.xerces.jaxp.SAXParserImpl.parse(Unknown Source)
	at javax.xml.parsers.SAXParser.parse(SAXParser.java:195)
	at org.openstreetmap.osmosis.xml.v0_6.XmlReader.run(XmlReader.java:111)
	at java.lang.Thread.run(Thread.java:722)

	Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'osm.schema_migrations' doesn't exist
		at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
		at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
		at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
		at java.lang.reflect.Constructor.newInstance(Constructor.java:525)
		at com.mysql.jdbc.Util.handleNewInstance(Util.java:411)
		at com.mysql.jdbc.Util.getInstance(Util.java:386)
		at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:1052)
		at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3609)
		at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3541)
		at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2002)
		at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2163)
		at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2618)
		at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2568)
		at com.mysql.jdbc.StatementImpl.executeQuery(StatementImpl.java:1557)
		at org.openstreetmap.osmosis.apidb.common.DatabaseContext.executeQuery(DatabaseContext.java:424)
		... 20 more

The error message is Table `geo.schema_migrations`doesn't exist. This is a table that contains the version information of the schema. It is important that your schema is the one that osmosis is expecting.

MySQL is no longer supported so there is no way to get a current schema for MySQL, on the other hand you would not need it for anything. You could pass `validateSchemaVersion=no` to osmosis to prevent it from checking the version, however that can cause an error in the process or the result.

### com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'osm.nodes' doesn't exist ###

I executed

	osmosis --read-xml file="bremen.osm.bz2" --write-apidb-0.6 host="127.0.0.1" dbType="mysql" database="osm" user="osm" password="osm" validateSchemaVersion=no

and got

	org.openstreetmap.osmosis.core.OsmosisRuntimeException: Unable to execute statement.
		at org.openstreetmap.osmosis.apidb.common.DatabaseContext.executeStatement(DatabaseContext.java:330)
		at org.openstreetmap.osmosis.apidb.common.DatabaseContext.disableIndexes(DatabaseContext.java:208)
		at org.openstreetmap.osmosis.apidb.v0_6.ApidbWriter.initialize(ApidbWriter.java:372)
		at org.openstreetmap.osmosis.apidb.v0_6.ApidbWriter.process(ApidbWriter.java:1089)
		at org.openstreetmap.osmosis.xml.v0_6.impl.NodeElementProcessor.end(NodeElementProcessor.java:139)
		at org.openstreetmap.osmosis.xml.v0_6.impl.OsmHandler.endElement(OsmHandler.java:107)
		at org.apache.xerces.parsers.AbstractSAXParser.endElement(Unknown Source)
		at org.apache.xerces.parsers.AbstractXMLDocumentParser.emptyElement(Unknown Source)
		at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl.scanStartElement(Unknown Source)
		at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl$FragmentContentDispatcher.dispatch(Unknown Source)
		at org.apache.xerces.impl.XMLDocumentFragmentScannerImpl.scanDocument(Unknown Source)
		at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
		at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
		at org.apache.xerces.parsers.XMLParser.parse(Unknown Source)
		at org.apache.xerces.parsers.AbstractSAXParser.parse(Unknown Source)
		at org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser.parse(Unknown Source)
		at org.apache.xerces.jaxp.SAXParserImpl.parse(Unknown Source)
		at javax.xml.parsers.SAXParser.parse(SAXParser.java:195)
		at org.openstreetmap.osmosis.xml.v0_6.XmlReader.run(XmlReader.java:111)
		at java.lang.Thread.run(Thread.java:722)
	Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'osm.nodes' doesn't exist
		at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
		at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
		at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
		at java.lang.reflect.Constructor.newInstance(Constructor.java:525)
		at com.mysql.jdbc.Util.handleNewInstance(Util.java:411)
		at com.mysql.jdbc.Util.getInstance(Util.java:386)
		at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:1052)
		at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3609)
		at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3541)
		at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2002)
		at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2163)
		at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2618)
		at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2568)
		at com.mysql.jdbc.StatementImpl.execute(StatementImpl.java:842)
		at com.mysql.jdbc.StatementImpl.execute(StatementImpl.java:681)
		at org.openstreetmap.osmosis.apidb.common.DatabaseContext.executeStatement(DatabaseContext.java:327)
		... 19 more

You have to import a valid schema before you can poulate the database. Unfortunately MySQL is no longer officialy supported, so there is no official schema available.

I found one [here](http://gweb.bretth.com/apidb06-mysql-latest.sql)
