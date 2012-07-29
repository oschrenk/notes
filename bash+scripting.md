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

## FAQ ##

### Find directory and path to script file ###

Taken from Stackoverflow question [Can a Bash script tell what directory it's stored in?](http://stackoverflow.com/questions/59895/can-a-bash-script-tell-what-directory-its-stored-in)

    DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

Is a useful one-liner which will give you the full directory name of the script no matter where it is being called from

Or, to get the dereferenced path (all directory symlinks resolved), do this:

    DIR="$( cd -P "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

These will work as long as the last component of the path used to find the script is not a symlink (directory links are OK). If you want to also resolve any links to the script itself, you need a multi-line solution:

    SOURCE="${BASH_SOURCE[0]}"
    DIR="$( dirname "$SOURCE" )"
    while [ -h "$SOURCE" ]
    do 
      SOURCE="$(readlink "$SOURCE")"
      [[ $SOURCE != /* ]] && SOURCE="$DIR/$SOURCE"
      DIR="$( cd -P "$( dirname "$SOURCE"  )" && pwd )"
    done
    DIR="$( cd -P "$( dirname "$SOURCE" )" && pwd )"

This last one will work with any combination of aliases, `source`, `bash -c`, symlinks, etc.

## Sources ##

[Shell Programming](http://linuxsig.org/files/bash_scripting.html)