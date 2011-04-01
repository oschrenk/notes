# Bash Scripting #

## Built in Shell Variables ##

| Variable | Use |
| :---- | :---- |
| `$#` | Stores the number of command-line arguments that were passed to the shell program. |
| `$?` | Stores the exit value of the last command that was executed. |
| `$0` | Stores the first word of the entered command (the name of the shell program). |
| `$*` | Stores all the arguments that were entered on the command line ($1 $2 ...). |
| `"$@"` | Stores all the arguments that were entered on the command line, individually quoted ("$1" "$2" ...). |

## Sources ##

[Shell Programming](http://linuxsig.org/files/bash_scripting.html)