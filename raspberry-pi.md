# Raspberry Pi #

Find your Pi on the network

```
sudo nmap -sP 192.168.11.0/24 | awk '/^Nmap/{ip=$NF}/B8:27/{print ip}'
```

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
