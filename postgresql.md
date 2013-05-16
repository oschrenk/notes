# PostgreSQL #

## Installation on OSX ##

	brew install postgresql
	[...]

## Usage ##

List databases

	\l

Drop database

	DROP DATABASE name;

## Troubleshooting ## ##

If you have an environment variable called `SCRIPTS` the compilation of postgresql and postgis will fail. Call `unset SCRIPTS` before installation.
