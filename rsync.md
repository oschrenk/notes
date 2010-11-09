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
