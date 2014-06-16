# PDF #

## Merge PDFs in OSX via Command line ##

OSX ships a command line tool to merge PDFs. No longer fiddling with drag and drop in Preview.

The suggestion from is to create an alias

	alias PDFconcat "/System/Library/Automator/Combine\ PDF\ Pages.action/Contents/Resources/join.py"

[#Stelluto:2010]

## Extract Images from pdf

	brew install xpdf
	pdfimages -j contact.pdf foo

`-j` should extract pictures as `.jpg`. In my case it didn't, but did so as `.ppm`, so



