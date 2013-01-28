# Lego Mindstorms on OSX 10.6

## Installation

- [MINDSTORMS NXT 2.0 Download](http://mindstorms.lego.com/en-gb/News/ReadMore/Default.aspx?id=351334)

### Standard Lego software

Before you install any software be advised that the software on the CD will not work on Snow Leopard aka OS X 10.6. It will install without any error messages, even prompting you to restart, but the installer will not install the software (at least not all of it). Lego is aware of the fact and released a "patch", which is actually just a ruby script, which copies content from the CD to the desktop, deletes one file before running the installer. The [script][patch-sl] has some additional checks, but here is the important part (the disk has to be loaded):

	$ mkdir ~/Desktop/"MINDSTORMS\ NXT\ Desktop\ Copy
	$ cp -R /Volumes/"MINDSTORMS\ NXT"/* ~/Desktop/"MINDSTORMS\ NXT\ Desktop\ Copy
	$ rm ~/Desktop/"MINDSTORMS\ NXT\ Desktop\ Copy"/Parts/MindstormsUniv[RE][ed][tu].pkg/Contents/Resources/preflight
	$ open -a ~/Desktop/"MINDSTORMS\ NXT\ Desktop\ Copy"/Install.app

The script also checks if you have the retail or the educational version. There seems to be a different software package.

## Bluetooth ##

You will a bluetooth dongle or  built-in support for bluetooth on your Mac, and a bluetooth stack. LEJOS is distributed with a 3rd party stack that is configured by default -- bluecove.

### Java Development Kit

You will need a 32bit version of Java 1.5

### LEJOS

You can find the latest [LEJOS][lejos] release (0.85) [here][lejos.download]. Extract it into a directory of your choosing, I extracted it to `~/Developent/sdk/lejos-nxj`

## Configuration

### Environment variables ###

	export NXJ_HOME=~/Development/sdk/lejos_nxj
	export PATH=~/Development/sdk/lejos_nxj/:$PATH

	# optional for use with Eclipse plugin
	export DYLD_LIBRARY_PATH=$NXJ_HOME/bin;

	# make sure that you use Java 1.5
	# export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.5.0/Home

### Permissions

Make the script files in the bin directory executable

	cd ~/Development/sdk/lejos_nxj/bin
	chmod +x *

### USB

## Flashing the Firmware ##

LEJOS NXJ is a firmware replacement, you will need to to flash the firmware on your NXT. Be advise that you will have to run Java 1.5 32bit version. If you run into problems please consult the FAQ.

1. Connect the NXT to your Mac with the USB cable
2. Open a terminal and change into the `lejos_nxj/bin` directory
3. Run `./nxjflash`

You get something like:

	Building firmware image.
	VM file: /Users/Oliver/Development/sdk/lejos_nxj/bin/lejos_nxt_rom.bin
	Menu file: /Users/Oliver/Development/sdk/lejos_nxj/bin/StartUpText.bin
	VM size: 52752 bytes.
	Menu size: 38016 bytes.
	Total image size 91008/94208 bytes.
	Locating device in firmware update mode.
	No devices in firmware update mode were found.
	Searching for other NXT devices.
	Found NXT: NXT 0016530E78B5
	The following NXT devices have been found:
	  1:  NXT  0016530E78B5
	Select the device to update, or enter 0 to exit.
	Device number to update:

For me it was option 1, after that the console reports

	Attempting to reboot the device.
	Locating device in firmware update mode.
	Found NXT: %%NXT-SAMBA%% 1
	Connected to SAM-BA v1.4
	Opened device in firmware update mode.
	Unlocking pages.
	Writing firmware image.
	Verifying firmware.
	Verified 94208 bytes ok.
	Restarting the device.

and the device makes some noise reporting that its changing its firmware. After it has been flashed, it starts up with a new boot logo and startup sound and the device will greet you with a nice menu.

## Compiling using the command line ##

	cd /path/to/projects
	mkdir/lejos
	cd lejos
	touch HelloWorld.java

### Hello World

Use the following source as an example

	import lejos.nxt.*;

	public class HelloWorld {
		public static void main (String[] args) {
			System.out.println("Hello World");
			Button.waitForPress();
		}
	}

### Compile

	nxjc HelloWorld.java

### Create the binary

	nxjlink -v HelloWorld -o HelloWorld.nxj

The -v or --verbose flag causes a list of class names and method signatures included in the binary to be sent to the standard output.

### Upload a program ###

Again be advised that you need to run Java 1.5 32bit. Consult the FAQ if you have problems although the NXT is on and is connected properly.

	nxjupload HelloWorld.nxj

### Link, Upload, Run ###

After compiling you can immediately run the program on your NXT by running

	nxj -r HelloWorld

Without `-r` the application is just linked and uploaded, but not run.

## FAQs/Problems ##

### Cannot load a comm driver

When flashing the NXT you can get an error message like this:

	VM file: /path/to/lejos_nxj/bin/lejos_nxt_rom.bin
	Menu file: /path/to/lejos_nxj/bin/StartUpText.bin
	VM size: 52752 bytes.
	Menu size: 38016 bytes.
	Total image size 91008/94208 bytes.
	Locating device in firmware update mode.
	an error occurred: Cannot load a comm driver

The problem is that the Fantom/Bluecove drivers are not working properly with 64-bit JVM. Setting the JVM in "Java Preferences" doesn't fix the problem. Instead you will have to force nxj to run in 32-bit mode. To do this open all the non binary files (the ones that don't end with .bin) in the `/lejos_nxj/bin/` directory in your favorite code editor and add `-d32` to all java calls. For example in nxjbrowse:

	java -Dnxj.home="$NXJ_HOME" -DCOMMAND_NAME="$NXJ_COMMAND" -Djava.library.path="$NXJ_BIN" -classpath "$NXJ_CP_TOOL" lejos.pc.tools.NXJBrowser  "$@"

to

	java -d32 -Dnxj.home="$NXJ_HOME" -DCOMMAND_NAME="$NXJ_COMMAND" -Djava.library.path="$NXJ_BIN" -classpath "$NXJ_CP_TOOL" lejos.pc.tools.NXJBrowser  "$@"

## Bluetooth ##

I had some problems connecting the NXT device with my Mac bluetooth. When I started to pair from the NXT the pairing failed even if I got to the point where I had to type in the key phrase on my Mac. When i started the pairing process from my Mac, I wasn't asked for the key phrase on my NXT.

I notices though that OS X tried to pair with the key phrase `0000`. So what I did was choosing `Blueooth >  Change PIN` on my NXT and set it to `0000`, then I added it the device on my Mac. It found the NXT, tried to pair using `0000` and did so successfully.

Uploading the program takes significantly more time. Applications took more than a minute to got transferred.

[patch-sl]: http://cache.lego.com/upload/contentTemplating/Mindstorms2SupportFilesDownloads/otherfiles/download19F59F05CF386B50188A5B9FFE3C9BF2.zip

[lejos]: http://lejos.sourceforge.net/
[lejos.download]: http://sourceforge.net/projects/lejos/files/lejos-NXJ/0.8.5beta/lejos_NXJ_0_8_5beta.tar.gz/download

[lejos.sourceforge]: http://lejos.sourceforge.net/forum/viewtopic.php?t=1696&postdays=0&postorder=asc&start=30
[lejos.api]: http://lejos.sourceforge.net/p_technologies/nxt/nxj/api/index.html
