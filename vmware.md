# VM Ware

## Uninstall ##

	sudo vmware-installer --uninstall-product vmware-player

Find out which products which you have installed

	vmware-installer --list-products

You should get a table of product names and versions. To uninstall a product

	sudo vmware-installer --uninstall-product <PRODUCT>

## Could not open /dev/vmmon: No such file or directory ##

	Could not open /dev/vmmon: No such file or directory. Please make sure that the kernel module `vmmon' is loaded

	sudo vmware-modconfig --console --install-all

## Shortcut to Start VM

	/usr/bin/vmplayer - X /path/to/guest.vmx
