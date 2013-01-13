# Fuse4X #

To install

	brew install fuse4x

Do as the screen tells you

	In order for FUSE-based filesystems to work, the fuse4x kernel extension
	must be installed by the root user:

	  sudo cp -rfX /usr/local/Cellar/fuse4x-kext/0.8.14/Library/Extensions/fuse4x.kext /System/Library/Extensions
	  sudo chmod +s /System/Library/Extensions/fuse4x.kext/Support/load_fuse4x

	If upgrading from a previous version of Fuse4x, the old kernel extension
	will need to be unloaded before performing the steps listed above. First,
	check that no FUSE-based filesystems are running:

	  mount | grep fuse4x

	Unmount all FUSE filesystems and then unload the kernel extension:

	  sudo kextunload -b org.fuse4x.kext.fuse4x

TL;DR

	sudo cp -rfX /usr/local/Cellar/fuse4x-kext/0.8.14/Library/Extensions/fuse4x.kext /System/Library/Extensions
	sudo chmod +s /System/Library/Extensions/fuse4x.kext/Support/load_fuse4x

I use [sshfs](http://fuse.sourceforge.net/sshfs.html)

	$ brew install sshfs

For usage of sshfs check it's note.
