$0, $1, $2, etc.

    Positional parameters, passed from command line to script, passed to a function, or set to a variable (see Example 4-5 and Example 15-16)
$#

    Number of command-line arguments [4] or positional parameters (see Example 35-2)
$*

    All of the positional parameters, seen as a single word

    Note	

    "$*" must be quoted.
"$@"

    Same as $*, but each parameter is a quoted string, that is, the parameters are passed on intact, without interpretation or expansion. This means, among other things, that each parameter in the argument list is seen as a separate word.
	
	$!

    PID (process ID) of last job run in background
	
	
	$_

    Special variable set to final argument of previous command executed.
	
	$?

    Exit status of a command, function, or the script itself (see Example 24-7)
	
	$$

    Process ID (PID) of the script itself. [5] The $$ variable often finds use in scripts to construct "unique" temp file names
	
	$1
	
	
	http://linuxsig.org/files/bash_scripting.html
	
	Variable 	Use
$# 	Stores the number of command-line arguments that were passed to the shell program.
$? 	Stores the exit value of the last command that was executed.
$0 	Stores the first word of the entered command (the name of the shell program).
$* 	Stores all the arguments that were entered on the command line ($1 $2 ...).
"$@" 	Stores all the arguments that were entered on the command line, individually quoted ("$1" "$2" ...).