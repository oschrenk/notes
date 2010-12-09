# Linux #

## Crontab ##

From [here](http://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)

To edit your crontab

	crontab -e

### Syntax ###

Your cron job looks as follows for user jobs:

 
	1 2 3 4 5 /path/to/command arg1 arg2
 
OR

	1 2 3 4 5 /root/backup.sh
 
Where,

	1: Minute (0-59)
	2: Hours (0-23)
	3: Day (0-31)
	4: Month (0-12 [12 == December])
	5: Day of the week(0-7 [7 or 0 == sunday])
	/path/to/command - Script or command name to schedule

Easy to remember format:

	* * * * * command to be executed
	- - - - -
	| | | | |
	| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
	| | | ------- Month (1 - 12)
	| | --------- Day of month (1 - 31)
	| ----------- Hour (0 - 23)
	------------- Minute (0 - 59)
