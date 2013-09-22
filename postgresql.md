# PostgreSQL #

Install

	brew install postgresql

If this is your first install, create a database with

	initdb /usr/local/var/postgres -E utf8

This will create a database in

	/usr/local/var/postgres

Start

	postgres -D /usr/local/var/postgres

Stop

	pg_ctl -D /usr/local/var/postgres stop

## Usage ##

List databases

	\l

Drop database

	DROP DATABASE name;

## Troubleshooting ## ##

## unset SCRIPTS ##

If you have an environment variable called `SCRIPTS` the compilation of postgresql and postgis will fail. Call `unset SCRIPTS` before installation.
