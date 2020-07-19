# Raspberry Pi #

Find your Pi on the network

```
sudo nmap -sP 192.168.11.0/24 | awk '/^Nmap/{ip=$NF}/B8:27/{print ip}'
```

## Boot Raspberry 4 from USB
1. Boot from a microSD card
2. Update OS by typing:

```
sudo apt update
sudo apt full-upgrade
```

3. Edit the `/etc/default/rpi-eeprom-update` file and change the `FIRMWARE_RELEASE_STATUS` value from `critical` to `stable.`

```
sudo vim s/etc/default/rpi-eeprom-update
```

4. Install the new bootloader by entering:

```
sudo rpi-eeprom-update -d -a
```
You should see that the firmware date is June 15th (the first stable release to support USB booting) or later.

5. Copy your microSD card to your USB drive or burn a new Raspberry Pi OS

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
