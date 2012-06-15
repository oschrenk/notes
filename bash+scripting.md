# Bash Scripting #

## Built in Shell Variables ##

| Variable | Use |
| :---- | :---- |
| `$#` | Stores the number of command-line arguments that were passed to the shell program. |
| `$?` | Stores the exit value of the last command that was executed. |
| `$0` | Positional parameters, passed from command line to script, passed to a function, or set to a variable. `$0` is the first word of the entered command (the name of the shell program). |
| `$*` | Stores all the arguments that were entered on the command line ($1 $2 ...). |
| `"$@"` | Stores all the arguments that were entered on the command line, individually quoted ("$1" "$2" ...). |
| `$!` | PID (process ID) of last job run in background |
| `$_` | Special variable set to final argument of previous command executed. |
| `$$` | Process ID (PID) of the script itself. The `$$` variable often finds use in scripts to construct "unique" temp file names |

## Sources ##

[Shell Programming](http://linuxsig.org/files/bash_scripting.html)