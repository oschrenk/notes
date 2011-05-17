# Oracle Database #

## SQL ##

### `EXECUTE IMMEDIATE` implicitly commits before executing DDL statements ###

When executing a `SQL` command in `PL/SQL` via `EXECUTE IMMEDIATE` and its an DDL commmand it implicitly makes a commit.

## Concepts ##

### Synonyms ###

A synonym is an alternative name for a table, view, sequence and other database objects. The references object does not need to exist at the time of creation of the synonym.

A public synonym is owned by `public` and is therefore valid for each schema. It is also the normal usecase to access objects in another schema without referencing the other schema.

## FAQ/Problems ##

### Shutdown/restart server ###

	sqlplus '/ as sysdba
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

Adter some botched coding and debugging sessions there were some open connections left.

To view current connections as user `sys` in the role `sysdba`:

	select * from v_$session

Existing connections can be closed with:

	alter system kill session 'sid,serial#' immediate;

## Helpful SQL Statements ##

### Oracle show trigger on table ###

	select    trigger_name,	trigger_type, status
	from      dba_triggers
	where  
	table_name = '&table' 
	-- and owner = '&owner'
