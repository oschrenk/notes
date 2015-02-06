# Powerline #

- [powerline](https://github.com/powerline/powerline)

I used a patched version of Menlo for Powerline from [here](https://gist.github.com/1595572) and used it with [bash-powerline](https://github.com/milkbikis/powerline-bash) but the glyphs in powerline for vim are broken despite having the font set in my `.vimrc`

```
set guifont=Menlo\ for\ Powerline
```

Reading through the issues on Github and the documentation the cause is a remapping of the glyphs, so that the old fonts found in [vim-powerline patched fonts](https://github.com/Lokaltog/vim-powerline/wiki/Patched-fonts) aren't compatible anymore (at least Menlo), without making [changes to the config and the colorscheme](https:	//github.com/Lokaltog/powerline/issues/27)

To get a working copy of Menlo for Powerline on OS X I had to patch the font myself. I installed powerline as a python package

	pip install https://github.com/Lokaltog/powerline/tarball/develop

but didn't find the fontpatcher directory in `/Library/Python/2.7/site-packages/powerline/` so I just downloaded it directly from github

	brew install fontforge
	wget https://raw.github.com/Lokaltog/powerline/develop/powerline/fontpatcher/fontpatcher.py
	wget https://raw.github.com/Lokaltog/powerline/develop/powerline/fontpatcher/fontpatcher-symbols.sfd
	cp /System/Library/Fonts/Menlo.ttc .
	fontforge -script ./fontpatcher.py Menlo.ttc

I got the following messages

	Copyright (c) 2000-2012 by George Williams.
	 Executable based on sources from 14:57 GMT 31-Jul-2012-D.
	 Library based on sources from 14:57 GMT 31-Jul-2012.
	The following table(s) in the font have been ignored by FontForge
	  Ignoring 'hdmx' horizontal device metrics table
	The glyph named mu is mapped to U+00B5.
	  But its name indicates it should be mapped to U+03BC.
	The glyph named Delta is mapped to U+2206.
	  But its name indicates it should be mapped to U+0394.

Copy the font to the system fonts directory as all users should have access

	cp Menlo\ Regular\ for\ Powerline.otf /Library/Fonts/

To use powerline in vim with the font edit your `~/.vimrc` and add the following lines

	python import sys; sys.path.append("/Library/Python/2.7/site-packages")
	python from powerline.ext.vim import source_plugin; source_plugin()
	set guifont=Menlo\ Regular\ for\ Powerline
	:set laststatus=2

With this patched font bash-powerline and powerline (for vim) look beautiful.
