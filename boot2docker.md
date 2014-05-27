# README #

## boot2docker ##


	https://github.com/aheissenberger/boot2docker



	https://github.com/aheissenberger/boot2docker

### On Ubuntu

#### Requirements ####

Install `git` and `curl`

	sudo apt-get install git curl

And have at least Virtual Box 4.3 (to use `VBoxManage modifyvm --vtxux`) which is not supported by default. To install Virtual Box 4.3

	sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list'
	wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
	sudo apt-get update
	sudo apt-get install virtualbox-4.3

### Execution ###

	git clone https://github.com/aheissenberger/boot2docker
	cd boot2docker
	bash boot2docker init



## Troubleshooting ##

### VBoxManage: error: Nonexistent host networking interface, name vboxnet0' (VERR_INTERNAL_ERROR) ###

	$ bash boot2docker up
	[2014-01-31 00:24:45] Starting boot2docker-vm...
	VBoxManage: error: Nonexistent host networking interface, name 'vboxnet0' (VERR_INTERNAL_ERROR)
	VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component Console, interface IConsole

## Get https://index.docker.io/v1/repositories/ubuntu/images: dial tcp: lookup index.docker.io: no DNS servers ##

	$ sudo docker pull ubuntu
	2014/01/31 00:34:39 POST /v1.8/images/create?fromImage=ubuntu&tag=
	Pulling repository ubuntu
	2014/01/31 00:34:39 Get https://index.docker.io/v1/repositories/ubuntu/images: dial tcp: lookup index.docker.io: no DNS servers

One hint was to add

	echo "nameserver 8.8.8.8" >> /etc/resolv.conf

in the VM. Restart your the VM.

What worked for me was to ping various docker services and add the appropiate IP to the VMs `/etc/hosts/` file.

ping registry-1.docker.io， get the ip , use it for cdn-registry-1.docker.io in /etc/hosts

	# `host registry-1.docker.io` and use that.
	# I got 54.224.119.89
	# Others got 54.234.135.251
	54.224.119.89 cdn-registry-1.docker.io index.docker.io

	# `host get.docker.io`
	162.159.251.135 get.docker.io

Keep in mind that these changes will not be preserved when you restart the machine.

It will timeout though

```
$ bash boot2docker build git@github.com:aheissenberger/boot2docker.git
[2014-01-31 01:02:17] Create TEMPDIR.
/var/folders/ld/w3p3zcjj4j10q_wnw_vt7rm40000gp/T/b2d.00LIeba8 ~/Docker/boot2docker
[2014-01-31 01:02:17] Download git@github.com:aheissenberger/boot2docker.git.
Cloning into '.'...
remote: Counting objects: 696, done.
remote: Compressing objects: 100% (313/313), done.
remote: Total 696 (delta 349), reused 691 (delta 346)
Receiving objects: 100% (696/696), 179.60 KiB | 148.00 KiB/s, done.
Resolving deltas: 100% (349/349), done.
Checking connectivity... done.
[2014-01-31 01:02:20] Start build
Pulling repository steeve/boot2docker-base
Get https://index.docker.io/v1/repositories/boot2docker/images: dial tcp 54.234.119.89:443: connection timed out
```

I tried


```
sudo kill $(cat /var/run/docker.pid)
sudo rm /var/run/docker.pid
echo "54.224.119.89 cdn-registry-1.docker.io index.docker.io" | sudo tee -a /etc/hosts
echo "162.159.251.135 get.docker.io" | sudo tee -a /etc/hosts
sudo touch /etc/resolv.conf
echo nameserver 8.8.8.8 | sudo tee -a /etc/resolv.conf
sudo /usr/local/bin/docker -d -dns=8.8.8.8 -dns 8.8.4.4 -H unix:// -H tcp:// &
```

## Repository steeve/boot2docker-base already being pulled by another client. Waiting. ##

Process was stuck.

I had to restart the Docker Daemon

	sudo kill $(cat /var/run/docker.pid)
	sudo rm -f /var/run/docker.pid
	sudo /usr/local/bin/docker -d -dns=8.8.8.8 -dns 8.8.4.4 -H unix:// -H tcp:// &