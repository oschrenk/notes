# readline #

> GNU Readline is a software library that provides line-editing and history capabilities for interactive programs with a command-line interface, such as Bash.
> It allows users to move the text cursor, search the command history, control a kill ring (a more flexible version of a copy/paste clipboard) and use tab completion on a text terminal.

## The magic ##

There are some nice `.inputrc` tricks you can use.

```
set completion-ignore-case on
```

When completing case will not be taken into consideration.

```
set completion-prefix-display-length 2
```

This one is insanely useful when you have a folder with lots of similarly named files and you are not sure how far the completion has gone when you press TAB. The first part that has been completed will be replaced by "...", and it is simple to see what you need to type to finish the completion.

Example:

```
$ ls
longFileNameLINUX-2.6.37-4  longFileNameLINUX-2.6.37-7  longFileNameLINUX-2.6.38-11  VeryCompliCATEDfileNAME.txt
longFileNameLINUX-2.6.37-6  longFileNameLINUX-2.6.37-8  longFileNameLINUX-2.6.38-9
$ ls long<TAB>
...7-4   ...7-6   ...7-7   ...7-8   ...8-11  ...8-9
$ ls longFileNameLINUX-2.6.3
```
