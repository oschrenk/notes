# Unix #

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

	0    # successful termination 
	64   # base value for error messages 
	64   # command line usage error 
	65   # data format error 
	66   # cannot open input     
	67   # addressee unknown     
	68   # host name unknown 
	69   # service unavailable 
	70   # internal software error 
	71   # system error (e.g., can't fork) 
	72   # critical OS file missing 
	73   # can't create (user) output file 
	74   # input/output error 
	75   # temp failure; user is invited to retry 
	76   # remote error in protocol 
	77   # permission denied 
	78   # configuration error