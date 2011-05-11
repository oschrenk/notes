# Oracle Database #

## SQL ##

### `EXECUTE IMMEDIATE` implicitly commits before executing DDL statements ###

When executing a `SQL` command in `PL/SQL` via `EXECUTE IMMEDIATE` and its an DDL commmand it implicitly makes a commit.

## System ##

### Datenbank herunterfahren/neustarten ###

	sqlplus '/ as sysdba
	SQL> shutdown immediate
	SQL> shutdown startup
	
### SQLException: ORA-00911: invalid character ###

Das kann natürlich viele Ursachen haben. bei mir war es das ich ein valides Statement mit einem Semikolon abgeschlossen habe.

### Datenbank neu starten nach Reboot der Machine###

	ssh user@oracle-db
	 # listener neu	 starten
	lsnrctl start
	 # database neu starten
	sqlplus / as sysdba
	SQL> startup;
	SQL> exit;
	# Enterprise Manager starten:
	emctl start dbconsole

### Bestehende Verbindungen/Sessions entfernen

Leider blieben bei manchen Debugging sessions Verbindungen zur Datenbank bestehen. Diese haben
verhindert Import fahren zu können.

Man kann die aktuellen Verbindungen als user `sys` in der Rolle `sysdba` so einsehen:

	select * from v_$session

Bestehende Verbindungen beendet man mit:

	alter system kill session 'sid,serial#' immediate;

## Nützliche SQL Statements

### Oracle show trigger on table ###

	select    trigger_name,
					trigger_type,
					status
	from      dba_triggers
	where  
	table_name = '&table' 
	-- and owner = '&owner'
