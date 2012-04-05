# Unix #

## Terminal ##

### Bash ###

Wildcards in bash are referred to as pathname expansion. Pathname expansion is also sometimes referred to as **globbing**

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

- `grep -F` Matches for a fixed string-

## File attributes

### Chmod

    read write execute = 111 = 1 + 2 + 4 = 7
    read write no execute = 110 = 4 + 2 = 6
    read no write execute = 101 = 4 + 1 = 5
    read no write no execute = 100 = 4
    no read write execute = 011 = 2 + 1 = 3
    no read write no execute = 010 = 2
    no read no write execute = 001 = 1
    no read no write no execute = 000 = 0

### ls -l “weirdness” +/@ symbol in `ls -l`

Sometimes when using `ls -l` there is a `+` or a `@` symbol

    	$ ls -la
    	-rwx------+ 1 testuser Admin     25600 Jul 12 02:04 file.ext
    	-rwx------@ 1 testuser Admin     25600 Jul 12 02:04 file.ext

The `+` indicates the use of ACL (Access Control Lists). Consult [ACL][2] for more infos.

The `@` indicates further file attributes. Consult [Apple File Attributes][3] for more infos.

### Access Control lists {#acl}

To view them just use `ls -le`.

This is a rather huge topic. Run `man chmod` and search for acl for a complete description.

To delete an acl entry you have to run `chmod -a` with the exact defintion of the acl. For example:
	
    	$ ls -le
    	-rw-rw----+ 1 Joe  staff      14336  1 Dez 12:23 file.ext
    	0: group:everyone deny delete
    	$ chmod -a "group:everyone deny delete" file.ext

### Apple File Attributes {#apple-file-attributes}

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