# Tar #

- Conventionally, uncompressed tar archive files have names ending in `.tar`, compressed ones use the extension of the applied compressionn utility, most of the time being *gzip*, e.g. `.tar.gz`

## Syntax ##

Basic syntax

	tar option(s) archive_name file_name(s)

- `c` create archive
- `f` next argument will be the name of a new archive file
- `v` verbose - display list of included files
- `-j, --bzip2` compression using [bzip2](http://www.linfo.org/bzip2.html)
- `z` compression using gzip
- `Z` compression
- `x` extract files

I always forget the syntax for (un-)packing tarballs.

- *Pack tarball* `tar -cvf file.tar myfile.txt`
- *Unpack tarball* `tar xvf file.tar`
- *Pack compressed tarball* `tar -czvf file.tar.gz home/`
- *Unpack compressed tarball* `tar -xzvf file.tar.gz`
