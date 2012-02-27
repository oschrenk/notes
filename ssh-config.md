# ssh-config #

ssh can be configured with a configuration file and via command line options. It obtains its options in the following order: command-line options, user's configuration file (`~/.ssh/config`) and system-wide configuration file ( `/etc/ssh/ssh_config` ).

A configuration file can specify settings for hosts via wildcard matching. An asterisk matches all the hosts, but only the first matching option is used. So be sure to put specific host options before more generic ones. For example

	Host remotehost
	ForwardX11 yes
	User remoteuser

	Host *
	ForwardX11 no

enables the `ForwardX11` option for the given `remotehost` but disables it for all other hosts.

## Options ##

- `ForwardX11 [yes|no]` or `ssh -X` Forwards X11 sessions from the remote machine to your local machine.
- `Compression[yes|no]`
- `ServerAliveInterval n` where n is time in seconds to send null packet, to request a response
- `ServerAliveCountMax n` If server becomes unresponsive, the number of retries before ssh disconnects
