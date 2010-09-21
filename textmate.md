# Textmate #

## Shortcuts ##

<dl>
	<dt>⌃</dt>
	<dd>Control</dd>
	<dt>⌥</dt>
	<dd>Alt</dd>
	<dt>⇧</dt>
	<dd>Shift</dd>
	<dt>⌘</dt>
	<dd>Command</dd>
</dl>

⌥⌘. Close <span class="caps">XML</span> Tag

⌃⌘T Selecting bundle item

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

> The XMLMate Plug-In adds an <span class="caps">XML</span> parsing
> palette to the popular TextMate text editor for Mac OS X. While
> editing an <span class="caps">XML</span> (or <span
> class="caps">XHTML</span>) document in TextMate, you can open the
> XMLMate palette to conveniently check your document for
> well-formedness or validity against a <span class="caps">DTD</span>,
> W3C <span class="caps">XML</span> Schema, <span
> class="caps">RELAX</span> NG schema, or Schematron schema.

Get it [here](//www.ditchnet.org/xmlmate/).

### Git.tmbundle ###

    $ mkdir -p ~/Library/Application\ Support/TextMate/Bundles
    $ cd ~/Library/Application\ Support/TextMate/Bundles
    $ git clone git://github.com/timcharper/git-tmbundle.git Git.tmbundle

*   In the TextMate preferences, advanced tab, shell variables, set
    the TM\_GIT variable to point to your installation of git (ie
    `/usr/local/bin/git`)
*   Many shortcuts are available from the Git-shortcut (Ctrl-Shift-G).
    Subversion commands are Command-Option-g.  Less frequent commands
    are accessed via the menu.
*   Update your bundle by running the "Update Git Bundle"
    command.
