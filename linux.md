# Linux #

## Update ##

	apt-get update
	apt-get upgrade
	
To search packages

	apt-cache --name-only search apache

## User Management ##

You shouldn't be working with a `root` account. Create another user first:

Add a user

	adduser --home /home/jdoe \
	        --shell /bin/bash \
	        --ingroup preexisting_group \
			jdoe
	passwd jdoe
	
The default configuration setting `NAME_REGEX="^[a-z][-a-z0-9]*\$"` rules which names aren't allowed and which aren't. You should follow the rule.

Just leave out the `ingroup` parameter for not adding the user to other groups.
		
Remove a user
	
	userdel -r  jdoe      ## Remove user incl. home directory

To give a user access to the sudo command you have to edit the `/etc/sudoers` file. Instead of editing it directly call

	visudo
	
In my case it opens `nano`. To give `joe` permission 

	# User privilege specification
	root    ALL=(ALL) ALL
	
add

	joe ALL=(ALL) ALL

Save the file. `visudo` takes care of sanity checks. 

I some distribution the option `targetpw` is set

	Defaults targetpw    # ask for the password of the target user i.e. root

It will ask for root's password regardless of the logged in user. Disable it to prompt for the user's password.

## Security ##

- keep your system up to date
- user strong password

### Lock down SSH ###

Edit the `/etc/ssh/sshd_config` and change the following settings

	PermitRootLogin no
	PasswordAuthentication no

- you shouldn't access your server as root
- you should use ssh keys to access your system

If you don't have an SSH key, you can generate one via

	ssh-keygen
	
By default your private key will be stored in `~/.ssh/id_rsa` and the private key into `~/.ssh/id_rsa.pub`.

You then want your key to be authorized. You do that by adding your key to the `~/.ssh/authorized_keys` file on the machine you want to log into.

	cat ~/.ssh/id_rsa.pub | ssh account@remoteserver.com "cat - >> .ssh/authorized_keys"