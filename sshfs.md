# SSHFS #

	brew install sshfs

To mount

	sshfs host: mountpoint

If the username is different

	sshfs username@host: mountpoint

You can also specify a directory after the `:`.  The default is the home directory.

To unnount

	umount -u mountpoint
