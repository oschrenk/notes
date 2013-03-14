# ln #

The `ln` utility creates a new directory entry (linked file) which has the same modes as the original file.

	ln -s /existing/path /destination/path

To replace an existing symbolic link to a file, use the `-f` option, so that you don't have to `rm` it before your link it.

	ln -sf /existing/file /destination/file

To replace an existing symbolic link to a directory, use the `-nf` option.

	ln -nsf /existing/dir /destination/dir

Options

- `-s` Create a symbolic link.
- `-f` If the `target_file` already exists, then unlink it so that the link may occur.
- `-n` If the `target_file` or `target_dir` is a symbolic link, do not follow
it. This is most useful with the `-f` option, to replace a symlink which may point to a directory.
- `-v` Cause `ln` to be verbose, showing files as they are processed.