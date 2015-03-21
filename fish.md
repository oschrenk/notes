Fish shell
========================

Programming in fish
-----------------------

Tests the expression given and sets the exit status to 0 if true, and 1 if false. An expression is made up of one or more operators and their arguments.

The first form (`test`) is preferred. For compatibility with other shells, the second form is available: a matching pair of square brackets (`[ [EXPRESSION ] ]`).

The following operators are available to examine files and directories:

- `-b FILE` returns true if `FILE` is a block device.
- `-c FILE` returns true if `FILE` is a character device.
- `-d FILE` returns true if `FILE` is a directory.
- `-e FILE` returns true if `FILE` exists.
- `-f FILE` returns true if `FILE` is a regular file.
- `-g FILE` returns true if `FILE` has the set-group-ID bit set.
- `-G FILE` returns true if `FILE` exists and has the same group ID as the current user.
- `-L FILE` returns true if `FILE` is a symbolic link.
- `-O FILE` returns true if `FILE` exists and is owned by the current user.
- `-p FILE` returns true if `FILE` is a named pipe.
- `-r FILE` returns true if `FILE` is marked as readable.
- `-s FILE` returns true if the size of `FILE` is greater than zero.
- `-S FILE` returns true if `FILE` is a socket.
- `-t FD` returns true if the file descriptor `FD` is a terminal (TTY).
- `-u FILE` returns true if `FILE` has the set-user-ID bit set.
- `-w FILE` returns true if `FILE` is marked as writable; note that this does not check if the filesystem is read-only.
- `-x FILE` returns true if `FILE` is marked as executable.

The following operators are available to compare and examine text strings:

- `STRING1 = STRING2` returns true if the strings `STRING1` and `STRING2` are identical.
- `STRING1 != STRING2` returns true if the strings `STRING1` and `STRING2` are not identical.
- `-n STRING` returns true if the length of `STRING` is non-zero.
- `-z STRING` returns true if the length of `STRING` is zero.

The following operators are available to compare and examine numbers:

- `NUM1 -eq NUM2` returns true if `NUM1` and `NUM2` are numerically equal.
- `NUM1 -ne NUM2` returns true if `NUM1` and `NUM2` are not numerically equal.
- `NUM1 -gt NUM2` returns true if `NUM1` is greater than `NUM2`.
- `NUM1 -ge NUM2` returns true if `NUM1` is greater than or equal to `NUM2`.
- `NUM1 -lt NUM2` returns true if `NUM1` is less than `NUM2`.
- `NUM1 -le NUM2` returns true if `NUM1` is less than or equal to `NUM2`.

Note that only integers are supported. For more complex mathematical operations, including fractions, the `env` program may be useful. Consult the documentation for your operating system.

Expressions can be combined using the following operators:

- `COND1 -a COND2` returns true if both `COND1` and `COND2` are true.
- `COND1 -o COND2` returns true if either `COND1` or `COND2` are true.

Expressions can be inverted using the `!` operator:

- `! EXPRESSION` returns true if `EXPRESSION` is false, and false if `EXPRESSION` is true.

Expressions can be grouped using parentheses.

- `( EXPRESSION )` returns the value of `EXPRESSION`.

 Note that parentheses will usually require escaping with `\(` to avoid being interpreted as a command substitution.


