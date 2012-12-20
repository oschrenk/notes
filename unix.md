# Unix #

## Shell ##

[What is the difference between shell, terminal and console?](http://superuser.com/questions/144666/what-is-the-difference-between-shell-console-and-terminal)

The **shell** is the program which processes command and returns output. Most shells also manage foreground  and background processes, command history and command line editing, with [Bash](http://www.gnu.org/software/bash/) being the most common shell in modern Linux systems.

A **terminal** refers to a wrapper program which runs a shell. Decades ago, this was a physical device consisting of little more than a monitor and keyboard. As Unix/Linux systems added better multiprocessing and windowing systems, this terminal concept was abstracted into software. Now you have programs such as Gnome Terminal which launches a window in a Gnome windowing environment which will run a shell into which you can enter commands.

The **console** is a special sort of terminal. Historically, the console was a single keyboard and monitor plugged into a dedicated serial console port on a computer used for direct communication at a low level with the operating system. Modern Linux systems provide for virtual consoles. These are accessed through key combinations (e.g. `Alt+F1`) which are handled at low levels of the Linux operating system -- this means that there is no special service which needs to be installed and configured to run. Interacting with the console is also done using a shell program.

### Bash ###

Wildcards in bash are referred to as pathname expansion. Pathname expansion is also sometimes referred to as **globbing**.

Pathname expansion that is done by the shell (in this case bash) and not by the operating system or by the program that is being run.

Pathname expansion "expands" the `*`, `?`, and `[...]` syntaxes when you type them as part of a command, for example:

	$ ls *.jpg         # List all JPEG files
	$ ls ?.jpg         # List JPEG files with 1 char names (eg a.jpg, 1.jpg)
	$ rm [A-Z]*.jpg    # Remove JPEG files that start with a capital letter

## Working with directories and files ##

`ls` 
- `-t` List files in order of last modification date, newest first. This is useful for very large directories when you want to get a quick list of the most recent files change. Probably most useful combined with `-l`. If you want the oldest files, you can add `-r` to reverse the list.
- `-X` Group files by extension; handy for polyglot code, to group header files and source files separately, or to separate source files from directories or build files.
- `-v` Naturally sort version numbers in filenames.
- `-S` Sort by filesize.
- `-R` List files recursively. This one is good combined with `-l`

### Finding files ###

`find` has a complex filtering syntax all of its own; the following examples show some of the most useful filters you can apply to retrieve lists of certain files:

- `find -name '*.c'` Find files with names matching a shell-style pattern. Use `-iname` for a case-insensitive search.
- `find -path '*test*'` Find files with paths matching a shell-style pattern. Use `-ipath` for a case-insensitive search.
- `find -mtime -5` Find files edited within the last five days. You can use `+5` instead to find files edited before five days ago.
- `find -newer server.c` Find files more recently modified than `server.c`.
- `find -type d` Find directories. For files, use `-type f`; for symbolic links, use `-type l`.

You can combine these to get even more powerful filters, for example a filter that shows C source code edited in the last two days.

	$ find -name '*.c' -mtime -2

### Searching in files ###

`grep [options] PATTERN [FILE...]` is useful here

- `grep -R somevar` This searches the current directory tree recursively for anything matching `someVar`
- `grep -iR`  By default grep works with fixed case. Change the behaviour by setting the insensitivity flag.
- `grep -l` Prints a list of files that match without printing the matches themselves with. This is very useful for building a list of files to edit in your chosen text editor.
- `grep -v` Exclude matches from result.
- `grep -F` Matches for a fixed string.

### Streams & Pipes ###

Most processes initiated by UNIX commands write to the standard output (that is, they write to the terminal screen), and many take their input from the standard input (that is, they read it from the keyboard). There is also the standard error, where processes write their error messages, by default, to the terminal screen.

The `cat` command can be used to write the contents of a file to the screen. Using `cat` without specifying a file to read from can be used to read from  standard input.

	$ cat

Press `Ctrl` key down and press `d` (written as `^D` for short) to end the input.

If you run the cat command without specifing a file to read, it reads the standard input (the keyboard), and on receiving the 'end of file' (`^D`), copies it to the standard output (the screen).

In UNIX, we can redirect both the input and the output of commands.

#### Redirecting the Output ####

The `>` symbol redirects the output of a command. For example, to create a file called `list1` containing a list of fruit, type  

	$ cat > list1

Then type in the names of some fruit. Press `Return` after each one.

	pear
	banana
	apple
	^D {this means press [Ctrl] and [d] to stop}

What happens is the `cat` command reads the standard input (the keyboard) and the `>` redirects the output, which normally goes to the screen, into a file called `list1`

#### Appending to a file

The form `>>` appends standard output to a file. So to add more items to the file `list1`, type

	$ cat >> list1

Then type in the names of more fruit

	peach
	grape
	orange
	^D (Control D to stop)

To read the contents of the file, type

	$ cat list1

It contains six fruit.

#### Redirecting the Input  

The `<` symbol redirects the input of a command.

The command `sort` alphabetically or numerically sorts a list. Type

	$ sort

Then type in the names of some animals. Press [Return] after each one.

	dog
	cat
	bird
	ape
	^D (control d to stop)

The output will be

	ape
	bird 
	cat 
	dog

Using `<` you can redirect the input to come from a file rather than the keyboard. For example, to sort the list of fruit, type

	$ sort < list1

To output the sorted list to a file, type,

	$ sort < list1 > slist

#### Pipes

To see who is on the system with you, type

	$ who

One method to get a sorted list of names is to type,

	$ who > names.txt
	$ sort < names.txt

This is a bit slow and you have to remember to remove the temporary file called names when you have finished. What you really want to do is connect the output of the who command directly to the input of the sort command. This is exactly what pipes do. The symbol for a pipe is the vertical bar `|`

For example, typing

	$ who | sort

will give the same result as above, but quicker and cleaner.

To find out how many users are logged on, type

	$ who | wc -l

## Filesystem ##

The [Filesystem Hierarchy Standard](http://www.pathname.com/fhs/pub/fhs-2.3.html) describes a set of requirements and guidelines for file and directory placement under UNIX-like operating systems. 

The following directories, or symbolic links to directories, are required in `/`.

	/bin	Essential command binaries
	/boot	Static files of the boot loader
	/dev	Device files
	/etc	Host-specific system configuration
	/lib	Essential shared libraries and kernel modules
	/media	Mount point for removeable media
	/mnt	Mount point for mounting a filesystem temporarily
	/opt	Add-on application software packages
	/sbin	Essential system binaries
	/srv	Data for services provided by this system
	/tmp	Temporary files
	/usr	Secondary hierarchy
	/var	Variable data

with optional

	/home	User home directories (optional)
	/lib<qual>	Alternate format essential shared libraries (optional)
	/root	Home directory for the root user (optional)

### Files ###

#### Unix File Permissions

The most common notation is symbolic notation. For example

	$ ls -la
	drwxr-xr-x 1 testuser Admin     25600 Jul 12 02:04 file.ext
	-rwx------ 1 testuser Admin     25600 Jul 12 02:04 file.ext

The first character indicates the file type:

- `-` denotes a regular file
- `d` denotes a directory
- `b` denotes a block special file
- `c` denotes a character special file
- `l` denotes a symbolic link
- `p` denotes a named pipe
- `s` denotes a domain socket

It is followed by three permission classes (or triad) with three characters each. Each of the three characters represent the read, write, and execute permissions respectively:

- `r` if the read bit is set, `-` if it is not.
- `w` if the write bit is set, `-` if it is not.
- `x` if the execute bit is set, `-` if it is not.

The binary (or decimal) notation for a triad is as follows

    read write execute = 111 = 1 + 2 + 4 = 7
    read write no execute = 110 = 4 + 2 = 6
    read no write execute = 101 = 4 + 1 = 5
    read no write no execute = 100 = 4
    no read write execute = 011 = 2 + 1 = 3
    no read write no execute = 010 = 2
    no read no write execute = 001 = 1
    no read no write no execute = 000 = 0

#### umask ####

> `umask` (user mask) is a command and a function in POSIX environments that sets the file mode creation mask of the current process which limits the permission modes for files and directories created by the process. A process may change the file mode creation mask with umask and the new value is inherited by child processes.

From [Umask](http://en.wikipedia.org/wiki/Umask) on Wikipedia.

`umask` defines the default permissions of *newly created* files and directories.

#### Access Control lists

Access Control lists are lists of file permissions attached to an object. They offer a far more complex system than basic Unix file permissions.

To see if a file has an ACL attached to it, you can use `ls -le`. 

    	$ ls -le
    	-rwx------+ 1 testuser Admin     25600 Jul 12 02:04 file.ext
    	-rwx------@ 1 testuser Admin     25600 Jul 12 02:04 file.ext

The `+` indicates the use of ACL (Access Control Lists). Consult [ACL][2] for more infos.

The `@` indicates further file attributes. Consult [Apple File Attributes][3] for more infos.

This is a rather huge topic. Run `man chmod` and search for acl for a complete description.

To delete an acl entry you have to run `chmod -a` with the exact definition of the acl. For example:
	
    	$ ls -le
    	-rw-rw----+ 1 Joe  staff      14336  1 Dec 12:23 file.ext
    	0: group:everyone deny delete
    	$ chmod -a "group:everyone deny delete" file.ext

#### Apple File Attributes ####

Apple adds an extra file attribute when files have been downloaded from the internet. It can be seen when using `ls -ls` indicated by the `@` symbol. By calling `ls -@` or `xattr`you can see that an attribute `com.apple.quarantine` has been added. To remove that call `sudo xattr -d com.apple.quarantine path` or `xattr -d com.apple.quarantine path`

## FreeBSD Interrupt Signals ##

| Signal Name | Signal Number | Signal Description | 
| :---- | :---- | :---- |
| SIGHUP | 1 | Terminal line hangup
| SIGINT | 2 | Interrupt program
| SIGQUIT | 3 | Quit program
| SIGILL | 4 | Illegal instruction
| SIGTRAP | 5 | Trace trap
| SIGABRT | 6 | Abort
| SIGEMT | 7 | Emulate instruction executed
| SIGFPE | 8 | Floating-point exception
| SIGKILL | 9 | Kill program
| SIGBUS | 10 | Bus error
| SIGSEGV | 11 | Segmentation violation
| SIGSYS | 12 | Bad argument to system call
| SIGPIPE | 13 | Write on a pipe with no one to read it
| SIGALRM | 14 | Real-time timer expired
| SIGTERM | 15 | Software termination signal
| SIGURG | 16 | Urgent condition on I/O channel
| SIGSTOP | 17 | Stop signal not from terminal
| SIGTSTP | 18 | Stop signal from terminal
| SIGCONT | 19 | A stopped process is being continued
| SIGCHLD | 20 | Notification to parent on child stop or exit
| SIGTTIN | 21 | Read on terminal by background process
| SIGTTOU | 22 | Write to terminal by background process
| SIGIO | 23 | I/O possible on a descriptor
| SIGXCPU | 24 | CPU time limit exceeded
| SIGXFSZ | 25 | File-size limit exceeded
| SIGVTALRM | 26 | Virtual timer expired
| SIGPROF | 27 | Profiling timer expired
| SIGWINCH | 28 | Window size changed
| SIGINFO | 29 | Information request
| SIGUSR1 | 30 | User-defined signal 1
| SIGUSR2 | 31 | User-defined signal 2
| SIGTHR | 32 | Thread interrupt

## Exit codes ##

Taken from the [FreeBSD Man Pages](http://www.freebsd.org/cgi/man.cgi?query=sysexits&sektion=3)

According to `style(9)`, it is not a good practice to call `exit(3)` with
arbitrary values to indicate a failure condition when ending a program.
Instead, the pre-defined exit codes from sysexits should be used, so the
caller of the process can get a rough estimation about the failure class
without looking up the source code.

The successful exit is always indicated by a status of `0`, or `EX_OK`.
Error numbers begin at `EX__BASE` to reduce the possibility of clashing
with other exit statuses that random programs may already return.	

The meaning of the codes is approximately as follows:

| Name | Number | Description | 
| :---- | :---- | :---- |
| EX_USAGE | 64 | The command was used incorrectly, e.g., with the wrong number of arguments, a bad flag, a bad syntax in a parameter, or whatever. |
| EX_DATAERR | 65 | The input data was incorrect in some way.  This should only be used for user's data and not system files. |
| EX_NOINPUT | 66 | An input file (not a system file) did not exist or was not readable.  This could also include errors like `No message` to a mailer (if it cared to catch it). |
| EX_NOUSER | 67 | The user specified did not exist.  This might be used for mail addresses or remote logins. |
| EX_NOHOST | 68 | The host specified did not exist.  This is used in mail addresses or network requests. |
| EX_UNAVAILABLE | 69 | A service is unavailable.  This can occur if a support program or file does not exist.  This can also  be used as a catchall message when something you wanted to do does not work, but you do not know why.
| EX_SOFTWARE | 70 | An internal software error has been detected.  This should be limited to non-operating system related errors as possible.
| EX_OSERR | 71 | An operating system error has been detected.  This is intended to be used for such things as `cannot fork`, `cannot create pipe`, or the like. It includes things like `getuid` returning a user that does not exist in the passwd file.
| EX_OSFILE | 72 | Some system file (e.g., `/etc/passwd`, `/var/run/utx.active`, etc.) does not exist, cannot be opened, or has some sort of error (e.g., syntax error).
| EX_CANTCREAT | 73 | A (user specified) output file cannot be created.
| EX_IOERR | 74 | An error occurred while doing I/O on some file.
| EX_TEMPFAIL | 75 | Temporary failure, indicating something that is not  really an error. In sendmail, this means that a mailer (e.g.) could not create a connection, and
 the request should be reattempted later.
| EX_PROTOCOL | 76 | The remote system returned something that was `not possible` during a protocol exchange.
| EX_NOPERM | 77 | You did not have sufficient permission to perform the operation.  This is not intended for file system problems, which should use `EX_NOINPUT` or `EX_CANTCREAT`, but rather for higher level permissions.
| EX_CONFIG | 78 | Something was found in an unconfigured or miscon figured state.

## Sources ##

- [Arabesque](http://blog.sanctum.geek.nz/)
- [Linux Journal](http://www.linuxjournal.com/content/bash-extended-globbing)