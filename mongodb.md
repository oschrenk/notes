# MongoDB #

## Installation ##

	brew install mongodb
	==> Caveats
	If this is your first install, automatically load on login with:
	    cp /usr/local/Cellar/mongodb/2.0.0-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
	    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

	If this is an upgrade and you already have the org.mongodb.mongod.plist loaded:
	    launchctl unload -w ~/Library/LaunchAgents/org.mongodb.mongod.plist
	    cp /usr/local/Cellar/mongodb/2.0.0-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
	    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

	Or start it manually:
	    mongod run --config /usr/local/Cellar/mongodb/1.6.5-x86_64/mongod.conf

## Usage ##

Start the database. If you used the `homebrew` installation and didn't create launch agents you have to start _mongoDB_ manually. Open a terminal and run

	 mongod run --config /usr/local/Cellar/mongodb/2.0.0-x86_64/mongod.conf
	
For just trying out some stuff you can use the mongo shell

	mongo 
	>
	
You can directly connect to a database by issuing

	mongo foo
	mongo 192.168.13.7/foo

### Basic Shell Commands ###

| Command | Description |
| :---- | :---- |
| `show dbs` | displays all the databases on the server you are connected to |
| `use db_name` | switches to db_name on the same server |
| `show collections` | displays a list of all the collections in the current database |
| `exit` | exit the shell |
| `help `| show help |

### Using the database ###

**Query a collection**

	db.foo.find();
	db.things.find({name:"mongo"}) # returns things with name = "mongo"
	db.things.findOne({name:"mongo"}) # syntactic sugar; returns the first thing with name = "mongo"
	db.things.find().limit(3); # specify the number of results

**Insert data**

	db.foo.save({ name : "sara"});
 
**Modify data**

	person = db.people.findOne( { name : "sara" } );
	person.city = "New York";
	db.people.save( person ); 

**Delete data**

| Command | Description |
| ------ | :---- |
| `db.foo.drop()` |	drop the entire foo collection |
| `db.foo.remove()` | remove all objects from the collection |
| `db.foo.remove( { name : "sara" } )`	|	remove objects from the collection where name is sara |