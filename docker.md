# docker #

You can think about containers as a process in a box. The box contains everything the process might need, so it has the file system, system libraries, shell and such, but by default none of it is started or run.

## boot2docker ##

> boot2docker is a lightweight Linux distribution based on Tiny Core Linux made specifically to run Docker containers. It runs completely from RAM, weighs ~24MB and boots in ~5-6s (YMMV).

	cd ~
	mkdir Docker
	cd Docker
	curl https://raw.github.com/steeve/boot2docker/master/boot2docker >
	boot2docker
	chmod +x boot2docker
	./boot2docker init
	./boot2docker up
	./boot2docker ssh

Once logged into the machine you need to format the virtual hard drive

	sudo -s
	fdisk /dev/sda
	n    # new primary partition
	p
	1   # first partition
	Enter  # default start
	Enter  # default end
	w  # write partition table and quit
	mkfs.ext4 /dev/sda1
	reboot

## Troubleshooting ##

### `dial unix /var/run/docker.sock: no such file or directory` ###

The docker daemon is not running. Start it.

## Resources ##

- [Docker--The Missing Tutorial](https://www.openshift.com/blogs/day-21-docker-the-missing-tutorial)