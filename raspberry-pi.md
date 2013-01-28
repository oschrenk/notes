# Raspberry Pi #

## Hardware ##

My setup

	Raspberry Pi Model B 	45 €
	4GB SD Card			 	 4 €
	Micro USB Power Supply   4 €
	SD Card Reader			 2 €
	Edimax EW-7811Un		11 €
						   =====
						    66 €

### Wireless Adapter ###

- [Edimax EW-7811Un wireless adapter](http://www.edimax.com/en/produce_detail.php?pd_id=347&pl1_id=1&pl2_id=44)
- [ASUS USB-N10 Wireless Lan USB Adapter](http://www.asus.de/Networks/Wireless_Adapters/USBN10/)

### Other ###

- [New Trent iCruiser 11000mAh External Battery Pack](http://www.amazon.com/dp/B003ZBZ64Q?tag=lavivastore-20)
- [Microsoft Lifecam Cinema](http://www.microsoft.com/hardware/en-us/p/lifecam-cinema)

## Operating System ##

There are many options. The official choices are to be found one [raspberrypi.org/downloads](http://www.raspberrypi.org/downloads).

I chose [Adafruit Raspberry Pi Educational Linux Distro](http://learn.adafruit.com/adafruit-raspberry-pi-educational-linux-distro/occidentalis-v0-dot-1) because it runs SSHD and AHAVI daemon (Bonjour Client) on the first boot, so that you can find your RaspPi on your local network under `raspberrypi.local`. As the default password user/password is `pi/raspberry` you should not put the RaspPi on an accessible network until you changed the password.

Disconnect your SD Reader/Card

	$ df -h

Connect your SD Reader/Card

	$ df -h

There should be one device that wasn't listed the last time (in my case	`/dev/disk2s1`). Unmount the volume

	$ sudo diskutil unmount /dev/disk2s1

The raw device name is the name of the volume without `s1` suffix but with `r` prefix. So in my case `rdisk2`

	$ sudo dd bs=1m if=~/Downloads/Occidentalist.img of=/dev/rdisk2

Eject the volume

	$ hdiutil eject /dev/disk2s1

Insert the SD Card into your RaspPi, connect it to the local network and power it up. Ping it

	$ ping raspberrypi.local

### WLAN Setup ###

Luckily the distribution I chose supported my WiFi adapter. So I just needed to configure my networks.

[Wireless Network Setup Tutorial](http://www.raspberrypi-tutorials.co.uk/set-raspberry-pi-wireless-network/)

	$ sudo touch /etc/wpa.config

Fill it using the rules for [wpa_supplicant.conf](http://www.daemon-systems.org/man/wpa_supplicant.conf.5.html)

	network={
		ssid="home"
		scan_ssid=1
		psk="<passphrase>"
		id_str="home"
	}
	network={
		ssid="work"
		scan_ssid=1
		psk="<passphrase>"
		id_str="work"
	}

For a little more security use

	$ wpa_passphrase ssid passphrase

Then edit the interface

	$ vi /etc/network/interfaces

The content should be something like

	# The loopback network interface
	auto lo
	iface lo inet loopback

	allow-hotplug eth0
	iface eth0 inet dhcp

	#Added by user
	allow-hotplug wlan1
	iface wlan1 inet manual
	   wpa-roam /etc/wpa.config

	iface home inet dhcp
	iface work inet dhcp

	#this line must always be here
	iface default inet dhcp

## Software ##

- [Cross-compiling with distcc](http://midnightyell.wordpress.com/2012/10/14/a-good-compromise-cross-compiling-with-distcc/)

### node.js ###

To run node.js 0.8.x see these [notes](https://github.com/nneves/R2C2_WebInterface/blob/master/specs/Compile_RaspberryPi_NodeV0.8.x.md)

Install dependencies

	$ sudo apt-get install git-core build-essential libssl-dev

Get sources, apply patch

	$ git clone https://github.com/gflarity/node_pi.git
	$ git clone https://github.com/joyent/node.git
	$ cd node
	$ git checkout origin/v0.8.2-release -b v0.8.2-release
	$ git apply --stat ../node_pi/v0.8.2-release-raspberrypi.patch

Set EXPORT vars to be used during compilation:

	$ export GYP_DEFINES='armv7=0'
	$ export CXXFLAGS='-march=armv6 -mfpu=vfp -mfloat-abi=hard -DUSE_EABI_HARDFLOAT'
	$ export CCFLAGS='-march=armv6 -mfpu=vfp -mfloat-abi=hard -DUSE_EABI_HARDFLOAT'

Configure and make (disable snapshot). This will take 3+ hours!

	$ ./configure --shared-openssl --without-snapshot --prefix=/opt/node
	$ make
	$ sudo make install

### shairport ###

Stream music from iTunes to the Raspberry Pi.

Install dependencies

	$ sudo apt-get install build-essential libssl-dev libcrypt-openssl-rsa-perl libao-dev libio-socket-inet6-perl libwww-perl avahi-utils pkg-config git

Clone sources

	$ git clone git://github.com/albertz/shairport.git
	cd shairport
	make

Test

	$ perl shairport.pl
