# Multimarkdown #

## Mathematics ##

Example within a paragraph \\({e}^{i\pi }+1=0\\).

And as an equation on it's own:
	
\\[ {x}_{1,2}=\frac{-b\pm \sqrt{{b}^{2}-4ac}}{2a} \\]

## Bibliography Support ##

Write following into the text

	... lorem ipsum from source [p. 23][#Doe:2006].

to use the citation also add a description of your reference

	[#Doe:2006]: John Doe. *Some Big Fancy Book*.  Vanity Press, 2006.

**Caveats**
	
- Don't forget the colon `:`!
- If you have multiple sources, they have to be seperated with an empty line to work.
