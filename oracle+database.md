# Oracle Database #

## Concepts ##

### Synonyms ###

A synonym is an alternative name for a table, view, sequence and other database objects. The references object does not need to exist at the time of creation of the synonym.

A public synonym is owned by `public` and is therefore valid for each schema. It is also the normal usecase to access objects in another schema without referencing the other schema.

### Materialized Views ###

Materialized Views aren't live, they contain a view of pre-generated data for retrieval.

You should first analyze all involved tables

	analyze table <table> compute statistics;

Create the view

	create materialized view mv1
	  build deferred
	  refresh on demand
	  enable query rewrite
	  as
		<select statement>

- `build deferred` means that the view isn't created directly after command execution; you have to start the creation process manually

- `refresh on demand` means that re-creation/refreshing is done manually

## Managment Console ##

### To kill a long running process ###

1. point your browser `https://<IP>:1158/em` (IP being the IP adress of the machone Oracle runs on, `1158` the default port of Oracle Enterprise Managment)
2. login (ask your adminstrator for the password) with role sysdba
3. Change to `Performance` tab
4. Under `Additional Monitoring Links` (`zusätzliche Überwachungs-Links`), click on `SQL-Monitoring` (`SQL-Überwachung`)
5. Click on the particular statement, open the context menu and `close session`

## Helpful SQL Statements ##

### Show trigger on table ###

	select    trigger_name,	trigger_type, status
	from      dba_triggers
	where
	table_name = '&table'
	-- and owner = '&owner'

### Which database version am I running ###

	select version from v$instance;

## Troubleshooting ##

### Shutdown/restart server ###

	sqlplus '/ as sysdba'
	SQL> shutdown immediate
	SQL> shutdown startup

### SQLException: ORA-00911: invalid character ###

There are many possible reasons. I closed a valid statement with a semicolon.

###  Restart Oracle after machine reboot ###

	ssh user@oracle-db
	 # listener neu	 starten
	lsnrctl start
	 # database neu starten
	sqlplus / as sysdba
	SQL> startup;
	SQL> exit;
	# Enterprise Manager starten:
	emctl start dbconsole

### Close/Remove open connections/sessions ###

After some botched coding and debugging sessions there were some open connections left.

To view current connections as user `sys` in the role `sysdba`:

	select * from v_$session

Existing connections can be closed with:

	alter system kill session 'sid,serial#' immediate;

### `EXECUTE IMMEDIATE` implicitly commits before executing DDL statements ###

When executing a `SQL` command in `PL/SQL` via `EXECUTE IMMEDIATE` and its an DDL command it implicitly makes a commit.

## Resources ##

- [AskTom](http://asktom.oracle.com)
- [Oracle consulting support](http://dba-oracle.com/)
- [Oracle Expert](http://hoopercharles.wordpress.com/)

