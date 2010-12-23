# MacPorts

## Updates

### Update Macports

To update macports and its ports tree, run

	$ sudo port selfupdate

### Show outdated ports

	$ sudo port outdated

### Upgrading Ports and dependencies

After the `selfupdate` run to update the ports

	$ sudo port -vu upgrade outdated´
	
- `-R` := upgrading dependents (ports that depend on the port being upgraded)
- `-u` := uninstall the old version after installing the new version
- `-v` := verbose output 

## Usage

### Find a port

Ports can be found [here](http://www.macports.org/ports.php)

### Install a new port

To install a port run 

	$ sudo port install <portname>

### Uninstall

#### Uninstall specific port

	$ sudo port -f uninstall <portname> [... <other_portname>]

You can spefify multiple ports to uninstall by just appending their names to the command

- -f := Force Mode

#### Uninstall all non active ports

	$ sudo port -f uninstall inactive

- -f := Force Mode

#### Uninstall all ports ####

	$ sudo port -f uninstall installed
	
#### Remove macports rests ####

	sudo rm -rf \
	    /opt/local \
	    /Applications/DarwinPorts \
	    /Applications/MacPorts \
	    /Library/LaunchDaemons/org.macports.* \
	    /Library/Receipts/DarwinPorts*.pkg \
	    /Library/Receipts/MacPorts*.pkg \
	    /Library/StartupItems/DarwinPortsStartup \
	    /Library/Tcl/darwinports1.0 \
	    /Library/Tcl/macports1.0 \
	    ~/.macports

### Activate a port ###

	$ sudo port -f activate portname

### clean ###

Deletes temporary and cached files. Normally you would only use it to save some space., but there are occurrences where cleaning resolves some build problems.
To clean a specific port run 

	$ sudo port clean <portname>

and to clean all installed ports run

	$ sudo port clean --all installed

## Advanced usage

### How to upgrade a port's version locally and report the version change

This note is taken from [here](http://trac.macports.org/wiki/howto/Upgrade)

 * Audience: Those who don't want to wait for a port to be updated
 * Requires: MacPorts >=1.6

Sometimes ports will fall behind the currently-available version.  When other ports are updated this can cause issues when the newer version of a port is needed for compatibility.  The Portfile for a port can be updated locally to allow you to upgrade it now, without waiting for an official update from the maintainer.

Anytime you see `<portname>` in this document, replace with the actual name of the port in which you are interested, it is only a placeholder here.

#### Step 1: Getting into the port directory and keeping a copy of the original ####

First, cd into the port's directory (which contains the Portfile) by running:

	$ cd $(port dir <portname>)

Then save a copy of the Portfile:

	$ sudo cp -p Portfile Portfile.orig

#### Step 2: Editing the Portfile ####

Use:

	$ sudo port edit <portname>

to edit the Portfile for the given port (this will open it in whatever editor you have defined via the VISUAL or EDITOR environment variables, or vi if not defined).  Since you're already in the directory containing the Portfile, you can also open it directly from here, but port edit always works.

As of MacPorts 1.7, you can also choose your editor from the command line directly instead of the environment variables:

	sudo port edit --editor nano <portname>

This will open nano to edit the Portfile for the given port.

#### Step 3: Updating the version ####

Once the Portfile has been opened, find the line which starts with version:

	version              1.4.1

Update the version given on that line (1.4.1 in this example) to the newly-desired version, then save the Portfile.  For example, if the new version is 1.5, it should simply look like:

	version              1.5

Also, search for a revision line like:

	revision             2

and if found, delete it.  Updated versions should start with revision 0 (which is the default when revision isn't present).

#### Step 4: Fetching and updating the checksums ####

Now fetch and run the checksum phase (which will fail, since it hasn't been updated for the new version) by running:

	$ sudo port -d checksum <portname>

This will fetch the new version you've specified then run a checksum against the downloaded file.  This will fail and since the debug (-d) flag was used, specify the checksums from the new file (among other lines):

	...
	checksums      md5     2e39a43b93b50c2ca90bcade26010878 \
                   sha1    2766d858b15d5d76da61e096fa6ffeb55b0469fb \
               	   rmd160  29488b09cc6d8013716c8a7d190fe6bc9625e568
	...

Copy this section (all three lines), use sudo port edit <portname> again, and change the checksum lines to be what you just copied.

Similarly, if the Portfile has a livecheck section which uses livecheck.check md5, run:

	$ sudo port -d livecheck <portname>

and update the Portfile's livecheck.md5 key with the new md5sum.

#### Step 5: Installing the new version ####

Now that the correct checksum has been specified, you can install the new version with:

	$ sudo port -d install <portname>

Use the debug flag again so that, in case something bad happens, the error message will be seen.  If it doesn't succeed, that goes beyond the scope of this document.

Otherwise, the port will be installed with the latest version specified.

#### Step 6: Filing an update ticket ####

Since it succeeded, [file a ticket](http://trac.macports.org/newticket) to upgrade the port to the new version. The best way to do this is to attach a diff against the Portfile to the new ticket, so generate a diff by running:

	$ diff -u Portfile.orig Portfile | sudo tee <portname>.diff

Then specify the `<portname>.diff` as a file to be attached to the new ticket.
	
If you edit the ticket please CC the maintainer, remove the version number put `haspatch` as keyword, put in the correct portname and maybe give it directly to the maintainer.	

#### Step 7: Cleaning up ####

Do a little clean up so extra files aren't left around:

	$ sudo rm <portname>.diff
	$ sudo mv Portfile.orig Portfile