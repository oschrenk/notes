# Git Annex #

- Git is hard to use for the _normal_ user
- Some files aren't fit for Git as they don't delta compress
- Git is perfect for us but not for monolithic big files

## Git and large files ##

- 2x disk space
- memory use
- auto repack slow and IO heavy (can really suck for big data)
- need local disk space for all files in repo

## Features ##

- push data via xmpp