# CVS #

## Sourceforge ##

To checkout complete projects using cvs from Sourceforge

	mkdir PROJECTNAME
	cd PROJECTNAME
	cvs -z3 -d:pserver:anonymous@PROJECTNAME.cvs.sourceforge.net:/cvsroot/PROJECTNAME checkout -P .
	
To checkout a single module from the project use  

	cvs -z3 -d:pserver:anonymous@PROJECTNAME.cvs.sourceforge.net:/cvsroot/PROJECTNAME checkout -P MODULENAME