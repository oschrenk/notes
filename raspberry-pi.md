# Raspberry Pi #

Find your Pi on the network

```
sudo nmap -sP 192.168.11.0/24 | awk '/^Nmap/{ip=$NF}/B8:27/{print ip}'
```

## Hardware ##

- [Edimax EW-7811Un wireless adapter](http://www.edimax.com/en/produce_detail.php?pd_id=347&pl1_id=1&pl2_id=44)
- [New Trent iCruiser 11000mAh External Battery Pack](http://www.amazon.com/dp/B003ZBZ64Q?tag=lavivastore-20)

## Operating System ##

There are many options. The official choices are to be found one [raspberrypi.org/downloads](http://www.raspberrypi.org/downloads).

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
