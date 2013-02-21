# lsof #

`lsof` is a command meaning "list open files", which is used in many Unix-like systems to report a list of all open files and the processes that opened them.

- `lsof` lists **all open files** by all processes.
- `lsof /path/to/file` lists all the processes, which are using the **file** in some way
- `lsof +D /usr/lib` finds all files in the specified **directory** and all the subdirectories.
- `lsof -u john.doe` limits output of files opened only by a specific user
- `lsof -u rms,root` limits output of files opened only by specific **users name**
- `lsof -c apache` selects the listing of files for processes whose **process name** begins with `apache`.
- `lsof -p 450,980,333` filters out open files by program's id **PID**.
- `lsof -i` lists all processes with open **Internet sockets** (TCP and UDP).
- `lsof -i tcp` list only processes with **TCP sockets**.
- `lsof -i udp` list only processes with **UDP sockets**.
- `lsof -i :25` find processes using TCP or UDP **port** 25.
- ` lsof -i udp:53` finds who's using **specific UDP port**.
- `lsof -i tcp:80` finds who's using **specific TCP port**.
- `lsof -a -u john.doe -i` lists of **network usage by user** `john.doe`.
- `lsof -N` lists all **NFS** (Network File System) files.
- `lsof -U` list all **Unix** domain socket files.
- ` lsof -g 1234`  finds all files opened by **group processes** with `PGID 1234`.
- `lsof -d 2` lists all files that have been opened as **file descriptor** 2.
- `lsof -d mem` lists **memory-mapped files**.
- `lsof -d txt` lists **programs** loaded in memory and **executing**.

## Logical operators ##

- `lsof -u john.doe -c apache` The **default is to OR between options**. It means it will combine outputs of `-u john.doe` and `-c apache` producing a listing of all open files by `john.doe` and all open files by apache.
- `lsof -a -u john.doe -c bash` `-a` combines the options with **AND**. The output listing is files opened by bash, which is run under `john.doe` user.
- `lsof -u ^root` List all open files by all users **EXCEPT** root.

## Time operators ##

- `lsof -r 1` makes `lsof` repeatedly list files until interrupted. Argument `1` means repeat the listing every `1` second. This option is best combined with a narrower query such as **monitoring** user network file activity `lsof -r 1 -u john -i -a`

[Peteris Krumins, lsof](http://www.catonmat.net/blog/unix-utilities-lsof/)