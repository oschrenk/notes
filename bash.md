# Shell

This is not a complete guide, but a collection of hints that makes the life with your shell easier. As my main system is a Mac, some hints might only work on OS X.

## Shortcuts

*   `ctrl + a` Jump to beginning of line
*   `ctrl + e` Jump to end of line
*   `ctrl + r` Start search (just start typing), pressing multiple times toggles through occurrences in history
*   `ctrl + l` Clear terminal
*   `ctrl + x, ctrl +e` Fires up `$EDITOR` to finish a long command in your favorite editor

## Redirecting/Appending Standard Output

*   `>` redirects *StdOut*, for example to a file `command > path` or to `/dev/null` (discards all data)
*   `>>` appends the output to the path `command >> path`

Similar for *StdErr*, but use `2` prefix like so `2>` or respectively. `2>>`.

## Using the history

Why use your brain to remember all the commands you have to use again and again. Just rely on your terminal and the history file it keeps.

Bash supports `Ctrl + r` to search the history. Per default the size history (and therefore the number of entries) is rather limited. You can do that by setting some variables in your `.profile` or (`.bash_profile`)

**WARNING**:OSX prefers the `.profile` to `.bash_profile`. If both exists **only** `.profile` will be read (see [.profile not read][1])

    #.bash_profile or .profile
    export HISTFILESIZE=1000000000      # sets history size to something really big
    export HISTSIZE=1000000             # sets history size to something really big

You can also set to ignore duplicates of commands (`ls`, `ps -ef` are such candidates)

    #.bash_profile or .profile
    export HISTCONTROL=ignoreboth

Other useful settings are

    # append to the same history file when using multiple terminals
    shopt -qs histappend

    # when typing part of a filename and press Tab to autocomplete, Bash does a case-insensitive search.
    shopt -qs nocaseglob

    # minor errors in the spelling of a directory component in a cd command will be corrected
    shopt -qs cdspell

## .inputrc

Next to `.profile` there is also `.inputrc`. This is the configuration file for `GNU` Readline (a library that allows to edit commands as they are typed in)

**An exemplary scenario** is when you have to login to multiple servers using `ssh ...` commands. Instead of toggling through all the occurrences in the history you can just start typing away @ssh @.

With the following snippet in the `.inputrc` file you can then just use your arrow keys to scroll through all the occurrences starting with the prefix you have typed in so far. This is especially useful when multiple logins share the same user but have different hosts.

   	# By default up/down are bound to previous-history
   	# and next-history respectively. The following does the
   	# same but gives the extra functionality where if you
   	# type any text (or more accurately, if there is any text
   	# between the start of the line and the cursor),
   	# the subset of the history starting with that text
   	# is searched (like 4dos for e.g.).
   	# Note to get rid of a line just Ctrl-C
   	"\e[A": history-search-backward
   	"\e[B": history-search-forward

It doesn’t stop there. If you like to move around in your command more quickly you might find use in the following snippet, which allows to jump over words in your command using `Strg` and the left or right arrow key.

   	# move around word for word with Strg + arrow keys
   	"\e[5C": forward-word
   	"\e[5D": backward-word
   	"\e\e[C": forward-word
   	"\e\e[D": backward-word

This one is very helpful when you just type away and realize later that the first character of is upper case instead of lower case (the default directories in the home directory of `OSX` all start with upper case letters).

   	# case insensitivity for tab-completion
   	set completion-ignore-case On

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

## File attributes

### Chmod

    read write execute = 111 = 1 + 2 + 4 = 7
    read write no execute = 110 = 4 + 2 = 6
    read no write execute = 101 = 4 + 1 = 5
    read no write no execute = 100 = 4
    no read write execute = 011 = 2 + 1 = 3
    no read write no execute = 010 = 2
    no read no write execute = 001 = 1
    no read no write no execute = 000 = 0

### ls -l “weirdness” +/@ symbol in `ls -l`

Sometimes when using `ls -l` there is a `+` or a `@` symbol

    	$ ls -la
    	-rwx------+ 1 testuser Admin     25600 Jul 12 02:04 file.ext
    	-rwx------@ 1 testuser Admin     25600 Jul 12 02:04 file.ext

The `+` indicates the use of ACL (Access Control Lists). Consult [ACL][2] for more infos.

The `@` indicates further file attributes. Consult [Apple File Attributes][3] for more infos.

### Access Control lists {#acl}

To view them just use `ls -le`.

This is a rather huge topic. Run `man chmod` and search for acl for a complete description.

To delete an acl entry you have to run `chmod -a` with the exact defintion of the acl. For example:

    	$ ls -le
    	-rw-rw----+ 1 Joe  staff      14336  1 Dez 12:23 file.ext
    	0: group:everyone deny delete
    	$ chmod -a "group:everyone deny delete" file.ext

### Apple File Attributes {#apple-file-attributes}

Apple adds an extra file attribute when files have been downloaded from the internet. It can be seen when using `ls -ls` indicated by the `@` symbol. By calling `ls -`@ you can see that an attribute `com.apple.quarantine` has been added. To remove that call `sudo xattr -d com.apple.quarantine path`

## Helpful commands ##

**Tar a directory**

	tar -pczf name_of_your_archive.tar.gz /path/to/directory

**Untar a tar.gz archive**

	tar xvfz /path/to/file.tar.gz

### Various ###

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

## FAQ/Problems

### Bash doesn’t update the PATH; changes to .profile aren’t registered {#profile-not-read}

Adding something like `EXPORT PATH=/usr/bin:$PATH` doesn’t update the `$PATH` environment variable. The reason is that Bash tries to find local profile files in the following order:

    # ~/.bash_profile
    # ~/.bash_login
    # ~/.profile

`~/.profile` is the last file in the list. So if OS X finds a file it stops processing. If an installer puts a `.bash_login` in your home directory your `.profile` wouldn’t be read.

### Reload .profile

    $ . ~/.profile

 [1]: #profile-not-read
 [2]: #acl
 [3]: #apple-file-attributes

### event not found ###	

I was trying to add a file to git that had an exclamation mark in the filename. Not a good idea.

	git add Play!.tmTheme
	git add "Play!.tmTheme"
	
both returned with	

	-bash: !.tmTheme: event not found
	
What helped was escaping the string with a single apostrohe

	git add -u 'Play!.tmTheme'

	
