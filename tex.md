# Tex #

- [Detexify](http://detexify.kirelabs.org/classify.html) Draw to recognize symbol

## Setting up xetex ##

Xetex allows proper UTF-8 support and using otf fonts

Write this into the header of your texfile

    	%!TEX TS-program = xelatex
    	%!TEX encoding = UTF-8 Unicode

Your editor of choice should recognize it. When not use command line and run `xelatex`

**DON'T** use `%\usepackage[utf8x]{inputenc}` as xetex already manages the encoding. When you use the package you will get weird errors as xetex does something to the file and again the `utf8x` package producing wrong UTF-8 character sequences.

## Using otf fonts ##

    otfinfo --family /Library/Fonts/GaramondPremrPro.otf

Your tex distribution should come with `otfinfo` tool which identifies the name of the font family for use with

   	\usepackage{fontspec}
   	\setromanfont{Bembo Std}

I ran into problems under Snow Leopard. The error was something like "...256..." as if the font wasn't installed. Clearing the font cache with Onyx did the trick, restarting your terminal might also help.

## Setting up pdfsync ##

`pdfsync` allows switching from the viewer into the editor directly to the line producing the content and vice versa

- Select Skim as my viewer in the Textmate preferences. `Bundles > LaTeX > Preferences ...`
- Select "View in Skim" from the LaTeX preferences in Textmate.
- Sync (from inside Skim preferences) with Textmate.
- Put the `pdfsync.sty` file in `\user\texmf\tex\latex` (normally comes with distribution)
- Inserted the `\usepackage{pdfsync}` in my document. (not needed with xetex)
- Forward search with "shift-command-click" and backward search with "Show in Viewer (pdfsync)".

## Skim ##

Once you have Skim installed, activate `Skim > Preferences > Sync > Check for file changes`. TextMate should already be entered as the default editor.

## Under version control ##

Tex creates a lot of temporary files that would only clutter the version control. 

A pretty complete list can be found [here](https://github.com/github/gitignore/blob/master/LaTeX.gitignore)

	*.acn
	*.acr
	*.alg
	*.aux
	*.bbl
	*.blg
	*.dvi
	*.fdb_latexmk
	*.glg
	*.glo
	*.gls
	*.idx
	*.ilg
	*.ind
	*.ist
	*.lof
	*.log
	*.lot
	*.maf
	*.mtc
	*.mtc0
	*.nav
	*.nlo
	*.out
	*.pdfsync
	*.ps
	*.snm
	*.synctex.gz
	*.toc
	*.vrb
	*.xdy

### SVN ###

Create a file called "ignore-these" with the (patterns of) files you want to ignore. Then run

    svn -R propset svn:ignore . -F ignore-these

To edit these properties later use  
	
	svn propedit svn:ignore .

### Git ### 

Just write the patterns to `.gitignore` and commit the file.