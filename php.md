# PHP #

## Troubleshooting

### Warning: require_once(file.php) [function.require-once]: failed to open stream: No such file or directory in /path/to/file2.php on line xx

Normally this just means that the file can't be found. Check the path and filename.

BUT it could also mean that open_basedir restriction in effect. PHP only spews the correct error message if you use a fully qualified path to the file if you intend to include it and not if you just give a relative path.

#### Warning: require_once(): open_basedir restriction in effect. ###

For instance

	Warning: require_once(Mail.php) [function.require-once]: failed to open stream: No such file or directory in /var/www/html/email.php on line 83
	Fatal error: require_once() [function.require]: Failed opening required 'Mail.php' (include_path='.:/usr/share/pear:/usr/share/php') in

in which a PEAR Module is included. If you are sure that the file exists, check if open_basedir restriction is in effect

If you include the file with a fully qualified name you will get another error message

	Warning: require_once(): open_basedir restriction in effect. File(/usr/share/php5/PEAR/Mail.php) is not within the allowed path(s): (/path/to/openbasedir/:/tmp) in xxx
