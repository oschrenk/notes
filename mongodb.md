@mongo @shell

# MongoDB #

	brew install mongodb

## Usage ##

Start the database. If you used the `homebrew` installation and didn't create launch agents you have to start _mongoDB_ manually. Open a terminal and run

	mongod run --config /usr/local/etc/mongod.conf

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

## Working with Mongo Shell

*connecting to replica set*

prefix a comma separated list of hosts with the name of the replica set

```shell
mongo --quiet -u readonly -p <pass> --host <rs_name>/<host1>:34339,<host2>:34332 my_db --eval 'db.collection_name.find({})'
```

*testing things*

```shell
test `mongo --quiet -u readonly -p <pass> --host <rs_name>/<host1>:34339,<host2>:34332 my_db --eval 'db.my_collection.find({creationDate: {$lt: new Date((new Date())-1000*60*60*24*14)}, "status": "Success"}).count() > 0' | grep -E '(true|false)'` = false
```

*querying things*

Getting proper output with mongo shell is hard

1. the shell limits your output to 20 documents
2. it prints log output to stdout

To solve (1) we wrap the query in a `printjson` statement, to solve (2) we silently curse at mongo and  just know that for our settings it prints 3 useless lines at the beginning and cut them off.

```shell
mongo --quiet -u readonly -p <pass> --host <rs_name>/<host1>:34339,<host2>:34332 my_db --eval 'printjson(db.my_collection.find({images: {"$exists": true}}, {images:1}).limit(40).toArray())' | tail -n +4 > images.json
```

For profit you can go on and filter out all urls (which ij this case requires [jq](https://stedolan.github.io/jq/)

```shell
cat images.json | jq '.[].images' | jq '.[]' | jq --raw-output '.url' > urls.txt
```

