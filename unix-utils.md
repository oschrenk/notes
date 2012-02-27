# Unix Tools #

## pv ##

Found on [Peteris Krumins' blog ](http://www.catonmat.net/blog/unix-utilities-pipe-viewer/)

`pv` - Pipe Viewer - is a terminal-based tool for monitoring the progress of data through a pipeline. It can be inserted into any normal pipeline between two processes to give a visual indication of how quickly data is passing through, how long it has taken, how near to completion it is, and an estimate of how long it will be until completion.

### Installation ###

	$ brew install pv # OSX
	$ apt-get install pv # Debian

### Usage ###

Suppose you want to compress a logfile

	$ gzip -c access.log > access.log.gz

The file is so huge (several gigabytes), you have no idea how long to wait. Will it finish soon? Or will it take another 30 mins?

Using `pv` you can precisely time how long it will take

	$ pv access.log | gzip > access.log.gz
	611MB 0:00:11 [58.3MB/s] [=>      ] 15% ETA 0:00:59

Pipe viewer acts as `cat` here, except it also adds a progress bar. We can see that gzip processed 611MB of data in 11 seconds. It has processed 15% of all data and it will take 59 more seconds to finish.

You may stick several pv processes in between. For example, you can time how fast the data is being read from the disk and how much data is gzip outputting:

	$ pv -cN source access.log | gzip | pv -cN gzip > access.log.gz
	source:  760MB 0:00:15 [37.4MB/s] [=>     ] 19% ETA 0:01:02
	  gzip: 34.5MB 0:00:15 [1.74MB/s] [  <=>  ]

Here we specified the "-N" parameter to pv to create a named stream. The `-c` parameter makes sure the output is not garbaged by one pv process writing over the other.

## Finding stuff

*   **Sorts the current directory after size** in human readable form `ls -lSrh`
*   **Biggest directory** `du -kx | egrep -v "\./.+/" | sort -n`
*   **Finds a file and shows space in use** `locate somefile | xargs ls -lsh`

### Grep

`find . -name "*" -exec grep -il "searchphrase" {} \;`

    -i ignores case
    -l shows filename

### Search in man pages

Just type `/` and then the word you search for.

### Useful search queries

*   Files **greater than 1GB** `find /path -size +1024000k -print`
*   Files **less than 7 days old** `find /path -mtime -7 -print`
*   Files **longer than 7 days old** `find /path -mtime +7 -print`
*   **Executable files** `find /path -perm -100 -print`
*   Files **with `-r--r--r--` permissions** `find /path -perm 444 -print`
*   **Directories with write permissions** `find /path -type -perm 777 -print`
*   Files **with specific extension** `find /path -name '*.ext' -print`

## Helpful commands ##

**Tar a directory**

	tar -pczf name_of_your_archive.tar.gz /path/to/directory

**Untar a tar.gz archive**

	tar xvfz /path/to/file.tar.gz

### Various ###

**Kill process via grep**
	
	ps -ef | grep <name> | grep -v grep | awk '{print $2}' | xargs kill -9

**Resolve to canonical path**. Useful if you used symbolic links to switch into a directory

    pwd -P

**Create symbolic link** in `/destination/path` to `/existing/path`

    ln -s /existing/path /destination/path

**Change to previous working directory**

    cd -@ 

Run the **last command as root**

    sudo !!

Runs **previous command but replacing first instance of foo with bar**

    ^foo^bar

Runs **previous command but replacing all instances of foo with bar**

    !!:gs/foo/bar

**Copy ssh key** to server

    cat ~/.ssh/id_rsa.pub | ssh account@remoteserver.com "cat - >> .ssh/authorized_keys"

**Spider a website**

    httrack "http://www.all.net/" -O "/tmp/www.all.net" "+*.all.net/*" -v

**Query wikipedia via dns**

    dig +short txt <keyword>.wp.dg.cx

**Stopwatch** (ctrl-d to stop)

    time read

**SSH connection through host in the middle**

    ssh -t reachable_host ssh unreachable_host

**Sharing file through port 80**, access with `http://ip-address/`

    nc -q 1 -w 5 -v -l -p 80 < file.ext

**Disk usage in readable form**

    du -sh [file ...]

**Rename all files in directory to lowercase**

    for x in *; do mv "$x" "`echo $x | tr [A-Z] [a-z]`"; done

**Replace “space” char with “dot”** char in current directory file names

    ls -1 | while read a; do mv "$a" `echo $a | sed -e 's/\ /\./g'`; done

**Recursive Search and Replace** with given String 
	
	$ find . -type f -exec sed -ie "s/old/new/g" {} +;
	
`-l` means that only lines that match will be displayed

**Batch rename files, replacing *foo* with *bar***

    for file in *.*; do mv $file `echo $file | sed 's/bar/foo/g'` ; done

**Create missing directory when moving files**

    mkdir -p ./some/path/; mv yourfile.txt $_
	
**Reverse output** 

	tac
	
eg. `svn log | tac` or if `tac` is not available

	awk '{ x = $0 "\n" x } END { printf "%s", x }'
 	
**Download a file with “authorization” cookie**

    wget --server-response --continue --load-cookies cookies.txt http://host.com/file.ext

**Recursively remove `.svn` directories**

    find . -type d -name '.svn' -print0 | xargs -0 rm -rdf

**Get public IP address**

    curl -s checkip.dyndns.org|sed -e 's/.*Current IP Address: //' -e 's/<.*$//'

**Get geographic locatio from ip**

    curl -s "http://www.geody.com/geoip.php?ip=10.0.0.1" | sed '/^IP:/!d;s/<[^>][^>]*>//g' ;
    curl -s "http://geoip.pidgets.com?ip=10.0.0.1&format=json"

**Copy directory structure without files keeping attributes, timestamps, permissions, …**

    rsync -a /path/from/ /path/to/ --include \*/ --exclude \*