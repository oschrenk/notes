# screen #

Sometimes you just want to connect to a distant server, launch a command and quit. The prpblem is when you disconnect the connection the command dies with it.

`screen` allows you to open virtual consoles you can leave running and resume.

1. ssh into the target machine
2. Open screen via `screen`; the terminal changes
3. Run your long running command
4. Exit screen with `ctrl+a, d`. `ctrl+a` gets you into screen options, and pressing `d` *detaches* the session.
5. `screen -r` resumes the session

## Automatic resume ##

Taken from [here](http://taint.org/wk/RemoteLoginAutoScreen)

Add these lines at the top of ~/.bashrc on the target host:

	# Auto-screen invocation. see: http://taint.org/wk/RemoteLoginAutoScreen
	# if we're coming from a remote SSH connection, in an interactive session
	# then automatically put us into a screen(1) session.   Only try once
	# -- if $STARTED_SCREEN is set, don't try it again, to avoid looping
	# if screen fails for some reason.
	if [ "$PS1" != "" -a "${STARTED_SCREEN:-x}" = x -a "${SSH_TTY:-x}" != x ]
	then
	  STARTED_SCREEN=1 ; export STARTED_SCREEN
	  [ -d $HOME/lib/screen-logs ] || mkdir -p $HOME/lib/screen-logs
	  sleep 1
	  screen -RR && exit 0
	  # normally, execution of this rc script ends here...
	  echo "Screen failed! continuing with normal bash startup"
	fi
	# [end of auto-screen snippet]

Create `~/.screenrc` on the target host, containing:

	# see http://www4.informatik.uni-erlangen.de/~jnweiger/screen-faq.html
	# support color X terminals
	termcap  xterm 'XT:AF=\E[3%dm:AB=\E[4%dm:AX'
	terminfo xterm 'XT:AF=\E[3%p1%dm:AB=\E[4%p1%dm:AX'
	termcapinfo xterm 'XT:AF=\E[3%p1%dm:AB=\E[4%p1%dm:AX:hs:ts=\E]2;:fs=\007:ds=\E]2;screen\007'
	termcap  xtermc 'XT:AF=\E[3%dm:AB=\E[4%dm:AX'
	terminfo xtermc 'XT:AF=\E[3%p1%dm:AB=\E[4%p1%dm:AX'
	termcapinfo xtermc 'XT:AF=\E[3%p1%dm:AB=\E[4%p1%dm:AX:hs:ts=\E]2;:fs=\007:ds=\E]2;screen\007'

	# auto-screen support; see http://taint.org/wk/RemoteLoginAutoScreen
	# detach on hangup
	autodetach on
	# no startup msg
	startup_message off
	# always use a login shell
	shell -$SHELL

	# auto-log
	logfile $HOME/lib/screen-logs/%Y%m%d-%n.log
	deflog on
