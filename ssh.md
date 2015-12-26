# SSH #

[SSH](http://tools.ietf.org/html/rfc4252) is a protocol for authenticating and encrypting remote shell sessions.

## Configuration ##

- per-user configuration is in `~/.ssh/config`.
- system-wide client configuration is in `/etc/ssh/ssh_config`.
- system-wide daemon configurtion is in `/etc/ssh/sshd_config`.

### Creating a key ###

    $ ssh-keygen -t rsa

**Using a secure password is recommended**. Otherwise someone who has access to your terminal, or your key will automatically have access to all hosts. Worst case scenario: Someone with a minute and a USB stick to spare, copies your key-pair and your `known_hosts` file. So **please use a password**. It might sound silly as you will still have to use a password, but there is a solution for that namely `ssh-agent`.

You will get something like:

    	Your identification has been saved in ~/.ssh/id_rsa.
    	Your public key has been saved in ~/.ssh/id_rsa.pub.

Guard the private key as if it were your password. You should at least give only your user the right to read the files.

    	# protect the keys from being read
    	cd ~/.ssh
    	$ chmod 600 id*
    	$ chmod 600 known_hosts

### Using an SSH Agent ###

Call `ssh-add` for every private key.

    	$ ssh-add -K ~/.ssh/id_rsa

### Could not open a connection to your authentication agent ###

The `ssh-agent` hasn't started or isn't properly configured. To manually start an agent, enter

    	$ ssh-agent bash

Or just include this startup script on your `.profile`

    	SSH_ENV="$HOME/.ssh/environment"

    `function start_agent {
    	     echo "Initializing new SSH agent..."
    	     /usr/bin/ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
    	     echo succeeded
    	     chmod 600 "${SSH_ENV}"
    	     . "${SSH_ENV}" > /dev/null
    	     /usr/bin/ssh-add;
    	}`

    `# Source SSH settings, if applicable`

    `if [ -f "${SSH_ENV}" ]; then
    	     . "${SSH_ENV}" > /dev/null
    	     #ps ${SSH_AGENT_PID} doesn't work under cygwin
    	     ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
    	         start_agent;
    	     }
    	else
    	     start_agent;
    	fi`

### Change SSH Passphrase ###

You can easily change the passphrase of your key by calling

    $ ssh-keygen -p
    Enter file in which the key is (/Users/username/.ssh/id_rsa):
    Key has comment '/Users/username/.ssh/id_rsa'
    Enter new passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved with the new passphrase.

### Copy SSH key to server ###

The best way is to rely on `ssh-copy-id`

    ssh-copy-id <user>@<hostname>

If the command does not exist, you can always try manually

    cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> "cat - >> .ssh/authorized_keys"

Which might fail, if `.ssh` directory doesn't exist. To create it on the fly and to give it sensible permission (`read-write` for owner only), try

    cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> 'umask 0077; mkdir -p .ssh; cat >> .ssh/authorized_keys && echo "Key copied"'

### Verify SSH Host fingerprint ###

    The authenticity of host 'host.domain (194.77.33.57)' can't be established.
    RSA key fingerprint is a8:04:af:ea:e0:8a:16:cf:f9:62:ba:1e:d2:16:f5:5a.
    Are you sure you want to continue connecting (yes/no)?

If this host is only accessible in your internal network, it might be safe to just answer yes to that question. You can easily go to your admin and just ask him if you feel unsure. But what if you want to access a more public server? How can you be sure you're not the victim of a man-in-the-middle attack.

Some site publish the fingerprint in public. Make sure that you read that fingerprint over HTTPS and not HTTP (which can be corrupted). If the fingerprint matches the one you read on the terminal the risk of being under attack is massively reduced (attackers still might have broken into the web server and changed the information there).

If you still feel unsure you have to ask the admin over telephone or in person, or even check the fingerprint yourself.

To verify a fingerprint you have to login in locally and NOT via network (as you might be under attack). To do so you can use `ssh-keygen -l -f public-key-file.pub`, which hashes the key and displays it in hexadecimal (for better readability).

On Linux you can

    $ ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key.pub

On OSX the public key is hiding in
`/etc/ssh_host_rsa_key.pub`, so:

    $ ssh-keygen -l -f /etc/ssh_host_rsa_key.pub

## Type less ##

	vi ~/.ssh/config

	Host aliasname
	HostName yourdomain.com
	User yourusername

## Troubleshooting ##

### Permissions are too open ###

After moving my profile from a cygwin installation back to my linux machine, I got the following error messages when I tried to clone a git repository:

	@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
	@ WARNING: UNPROTECTED PRIVATE KEY FILE! @
	@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
	Permissions 0644 for '/home/me/.ssh/id_rsa_targethost' are too open.
	It is recommended that your private key files are NOT accessible by others.
	This private key will be ignored.
	bad permissions: ignore key: /home/me/.ssh/id_rsa_targethost

The problem can't be better explained, so I just leave you with

	$ chmod -v -R 600 ~/.ssh

or in human readable form:

	$ chmod -v -R a-rwx ~/.ssh
	$ chmod -v -R u+rw ~/.ssh

### SSH Agent (key) forwarding on Mac OS Lion doesn't work anymore ###

The problem is, thht Lion's new feature to reopen all windows on restart somehow broke the ssh-agent. It won't start before Terminal.app

In OS X Lion if the login keychain is set to lock on sleep, the ssh-agent becomes confused. Instead of asking to unlock the login keychain and using the stored password from that, it instead asks for the ssh key pass phrase. If the ssh-agent is killed off after the keychain is locked, the process spawned as a result of further requests acts properly (asking to unlock the keychain).

[ssh-agent-locker](https://github.com/gdcbyers/ssh-agent-locker) helps with this. I installed it via

	brew install https://raw.github.com/rsenk330/homebrew/ssh-agent-locker/Library/Formula/ssh-agent-locker.rb

	If this is your first install, automatically load on login with:
	  mkdir -p ~/Library/LaunchAgents
	  cp /usr/local/Cellar/ssh-agent-locker/0.1.0/com.seaandco.geoff.ssh-agent-locker.plist ~/Library/LaunchAgents/
	  launchctl load -w ~/Library/LaunchAgents/com.seaandco.geoff.ssh-agent-locker.plist
