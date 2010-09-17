# Textile #

## Reference ##

### Adding anchor links ###

Set your anchor links in your header tags by adding `(#yourAnchorId)`
like so:

    h1(#yourAnchorId). 

Link to it by using the the normal link `"Text":#yourAnchorId`

### Adding non textile html blocks ###

Just add `<notextile>` blocks like so

    <notextile>
    	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    	<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    		<head>
    			<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    			<title>Title</title>
    		</head>
    		<body>
    </notextile>

Especially useful for including header information.

## <span class="caps">FAQ</span>/Problems ##

### Commands aren't replaced leaving `<command>. content` in output ###

This can happen if there is a tab after the command instead of the
space, which is not distinguishable from one another if a tab
represents four spaces and the command is two spaces long.

Remove the tab and replace it with a space character.
