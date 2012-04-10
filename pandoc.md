# Pandoc #

	$ brew install haskell-platform
	$ cabal install pandoc
	
## Usage ##

Normally pandoc will write to `stdout` (Exception: if the output format is `odt`, `docx`, or `epub`)

	pandoc --from=<FORMAT> --to=<FORMAT> path/to/input
