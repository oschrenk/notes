# Textmate #

## Important shortcuts ##

- *⌃⌘T* **Selecting bundle item**

- *⌥⌘.* Close XML Tag

with

- ⌃ Control
- ⌥ Alt
- ⇧ Shift
- ⌘ Command

## Regular Expressions ##

Replace

	strtof(argv[1]);
	[..]
	strtof(argv[12]);

with

	strtof(argv[1]);
	[..]
	strtof(argv[12]);

Through the magic of regular expressions:

	Find:    strtof\(argv\[?(\d{1,2})\]\)
	Replace: strtof(argv[$1],NULL)

`(`, `)`, `[`, `]`, `{`, `}` must be escaped with `\` as  they are reserved for the regex syntax.

- `()` is used to define a capture group
- `$1` to `$n` are used to identify capture group
- `$0` is the entire match

Fore more infos and magic (case foldings, conditionals) consult [Regex Syntax](http://manual.macromates.com/en/regular_expressions#syntax_oniguruma) and [Replacement String Syntax](http://manual.macromates.com/en/regular_expressions#replacement_string_syntax_format_strings)

## Appearance ##

### Themes for Webpreview ###

You might have to create the directory containing the user themes
first.

    $ mkdir ~/Library/Application\ Support/TextMate/Themes/WebPreview

Create a subdirectory for each theme you intend to create. Then let
your creative juices flow and create your own stylesheets. Just
remember to call the stylesheet

    style.css

All selectors should be prefixed with `.<themename>` to avoid
affecting other themes.

## Useful Plugins/Bundles ##

If you put a new bundle into the `Bundles` directory you might want
and Textmate is running and don't want to close it, you can
reload your bundles with

    $ osascript -e 'tell app "TextMate" to reload bundles'

### XMLMate ###

> The XMLMate Plug-In adds an XML parsing palette to the popular TextMate text
> editor for Mac OS X. While editing an XML (or XHTML) document in TextMate,
> you can open the XMLMate palette to conveniently check your document for
> well-formedness or validity against a DTD, W3C XMLSchema, RELAX NG schema, or
> Schematron schema.

Get it [here](//www.ditchnet.org/xmlmate/).

### Git.tmbundle ###

    $ mkdir -p ~/Library/Application\ Support/TextMate/Bundles
    $ cd ~/Library/Application\ Support/TextMate/Bundles
    $ git clone git://github.com/timcharper/git-tmbundle.git Git.tmbundle

*   In the TextMate preferences, advanced tab, shell variables, set
    the TM\_GIT variable to point to your installation of git (i.e.
    `/usr/local/bin/git`)
*   Many shortcuts are available from the Git-shortcut (Ctrl-Shift-G).
    Subversion commands are Command-Option-g.  Less frequent commands
    are accessed via the menu.
*   Update your bundle by running the "Update Git Bundle"
    command.
