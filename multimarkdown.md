# Multimarkdown #

## Features ##

### Footnotes ###

	Here is some text containing a footnote.[^somesamplefootnote]
	...
	[^somesamplefootnote]: Here is the text of the footnote itself.

### Write letters with markdown using Latex ##

	latex input:        mmd-letterhead-header
	Title:              Test Letter
	Author:             John Doe
	email:              fletcher@example.net
	address:            123 Main St.
	                    Some City, ST  12345
	recipient:          Some Person
	Recipient Address:  321 Main St
	                    Some City, ST  54321
	phone:              (555) 555-5555
	Date:               December 15, 2007
	latex xslt:         custom-letterhead.xslt
	black and white:    true
	base header level:  2
	latex mode:         memoir
	latex footer:       mmd-letterhead-footer
	latex input:        mmd-letterhead-begin-doc

### Mathematics ###

Example within a paragraph \\({e}^{i\pi }+1=0\\).

And as an equation on it's own:

\\[ {x}_{1,2}=\frac{-b\pm \sqrt{{b}^{2}-4ac}}{2a} \\]

### Bibliography Support ###

Write following into the text

	... lorem ipsum from source [p. 23][#Doe:2006].

to use the citation also add a description of your reference

	[#Doe:2006]: John Doe. *Some Big Fancy Book*.  Vanity Press, 2006.

**Caveats**

- Don't forget the colon `:`!
- If you have multiple sources, they have to be separated with an empty line to work.
