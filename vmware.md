# VM Ware

## Could not open /dev/vmmon: No such file or directory ##

	Could not open /dev/vmmon: No such file or directory. Please make sure that the kernel module `vmmon' is loaded

	sudo vmware-modconfig --console --install-all

## Shortcut to Start VM

	/usr/bin/vmplayer - X /path/to/guest.vmx