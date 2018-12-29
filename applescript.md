# AppleScript #

Compile to `.app`

	osacompile -o /path/to/applicationbundle.app /path/to/textfile.applescript

## Time and Date

If you assign the current time to a variable as in `set myDate to current date` and you just want to calculate the `current date plus 2 days`, then there is no need to use epoch-seconds.

```
set myNewDate to myDate + (2 * days) -- adds 2 days to the date and time stored in myDate.
set myNewDate to myDate + (4 * hours) -- adds 4 hours to the current time and date.
set myNewDate to myDate + (30 * minutes) -- adds 30 minutes to the current time and date.
```

If you need to set the date and time to the beginning of the current day, then use

```
set myTime to time of myDate
set myNewDate to myDate - myTime
```

The variable `myNewDate` now contains the current date and 00:00:00 as time.
