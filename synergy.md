# Synergy #

OS X

	brew install synergy

Ubuntu

	sudo apt-get install synergy

## Configuration ##

- [Synergy Configuration Reference](http://synergy2.sourceforge.net/configuration.html)

Create config file

	$ touch /etc/synergy.conf
	$ sudo vi /etc/synergy.conf

Add following

	section: screens
	molly:
	windowspc:
	end

	section: aliases
	windowspc:
	192.168.1.101
	end

	section: links
	ubuntu:
	left = windowspc
	windowspc:
	right = ubuntu
	end

	section: options
	screenSaverSync = false
	end

Make world readable

	$ sudo chmod a+r /etc/synergy.conf

Start server in debug mode first` (-f` stands for foreground)

	$ synergys -f --config /etc/synergy.conf

To run it in background

	$ synergys --config /etc/synergy.conf

To run the client

	$ synergc synergy-server-hostnamapt-get

## Beware ##

The configuration must contain all screens (read names) or at least all aliases of the machines you want to connect, even itself.

You can supply a name using `--name <name>`
