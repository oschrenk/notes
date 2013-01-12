# SED #

**sed** (stream editor) is a Unix utility that parses text and implements a programming language which can apply transformations to such text. It reads input line by line (sequentially), applying the operation which has been specified via the command line (or a sed script), and then outputs the line.

## Usage ##

**Find and replace** `ugly` with `beautiful`

	$ sed -i 's/ugly/beautiful/g' file.txt

## Sed Command Options ##

- `-i` Edit files in-place, saving backups with the specified extension. If a zero-length extension is given, no backup will be saved.

## Sed Pattern Flags ##

 - `/g` Global replacement. sed work line by line and would normally only replace the first occurrence of a match in each line. If this flag is given, it will replace all occurrences.

