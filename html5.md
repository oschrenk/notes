# HTML5 #

*Doctype* must be declared as first line like so

	<!DOCTYPE html>

The *language* of the document is best set via HTTP Headers

	Content-Language: en

or by setting it directly on the html tag via an attribute

	<html lang="en">

The *encoding* is also best set via HTTP headers

	Content-Type: text/html; charset=iso-8859-1

or via a meta tag

	<meta charset="utf-8" />

Reference *external stylesheets* via link tag

	<link rel="stylesheet" href="stylesheet.css" />

Reference *favicon* via link tag

	<link rel="shortcut icon" href="/favicon.ico">
