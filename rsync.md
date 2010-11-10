# rsync #

    rsync -rv
    rsync -arvz

r (recursive)
:   copies directories and sub directories

v (verbose)
:   prints on the screen what is being copied, increase verbosity by adding more `v`, like `-vv` or `-vvv`

a (archive)
:   symbolic links, devices, attributes, permissions, ownerships, etc. are preserved

z (compressed)
:   data will be compressed over the network

To copy the contents of `path` to `/volume/path/`

    $ rsync -r /path/ /volume/path/

To copy the folder `path` and its contents to `/volume/path` (it will create the folder: path):

    $ rsync -r /path /volume/path

Without the final slash, rsync will copy the directory in its entirety. With the trailing slash, it will copy the contents of the directory but won’t recreate the directory

Exclude files by adding patterns, for example you can exclude hidden files ba adding

    --exclude=".*/"

rsync even has an built in option to exclude `cvs` or `svn` files, just add

    --cvs-exclude

## Useful examples ##

Copy files from `path` to `backup` deleting files that don't exist on the sending side, ignoring `.git`

	rsync -arvz --delete --exclude=.git /path /backup
	
## FAQ/Problems ## ##

### rsync: command not found ###

I was trying to sync data to a Solaris machine but ran into this error:

	> /usr/bin/rsync -avuz --stats someuser@remote-solaris-machine:/export/CVS-xcert/* /export/HCL-CVS 
	bash: rsync: command not found
	rsync: connection unexpectedly closed (0 bytes received so far) [sender]
	rsync error: remote command not found (code 127) at io.c(454) [sender=2.6.9]
	
Rsync was installed on both machines but somehow the solaris machine wasn't able find rsync in the given path. I guess this is its trying to sync over ssh, and when by doing so doesn't include the `/usr/local/bin` directory.

Luckily you can pass the path of rsync as a parameter

	rsync -arvz --stats --rsync-path=/usr/local/bin/rsync someuser@remote-solaris-machine:path/to/project/ /path/to/remote/project 
	
Taken from [Siddesh BG](http://siddesh-bg.blogspot.com/2009/02/rsync-command-not-found-error-even.html)
