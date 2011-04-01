# Unix #

## FreeBSD Interrupt Signals ##

Signal Name 	Signal Number 	Signal Description

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
