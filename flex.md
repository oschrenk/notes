# Flex #

## Installation ##

### OSX ###

    $ wget http://download.macromedia.com/pub/flex/sdk/flex_sdk_3.5.zip
    $ sudo cp -r flex_sdk_3 /Developer/SDKs/

Add it to the path by editing your `.profile`

    export PATH="/Developer/SDKs/flex_sdk_3/bin:$PATH"/

If you have the Flex Builder installed, the SDK should be found under `/Developer/SDKs/flex_sdk_3`. Just add it to the path in that case.

## Data Bindings ##

### Bindable ###

If you use `[Bindable]` on your class (or on the respective fields/methods) make sure that the instance of the class is also marked as `[Bindable]`.

## Troubleshooting ##

### Invalid Embed directive in stylesheet `can't resolve source Embed(/assets/path/file.png)` ###

This can be a pain. You might try clearing the cache, cleaning the project, deleting your files in the `bin` directory but still run into this error.

The cause was not the css file but a custom component using an Embed statement. In this statement a graphic resource has been referenced that wasn't there yet. For some reasons Flex Builder spews the above errors first but the root cause later

    unable to resolve '/assets/path/file2.png' for transcoding	project/components	Component.mxml	line 224	1263378739408	3433

If you try to resolve your compile error from top to bottom, you won't see the error and, even worse, if you have more than 100 Embed directives in your css file you won't even see the other error message, as Flex Builder per default only shows the first 100 error messages.

To show more error messages use the white triangle on the top right corner in the problems pane and choose `Preferences...` to change the number of errors.

### Building project via Ant results in OutOfMemoryError ###

When trying to compile the sources via Ant from the command line I got this output

	[mxmlc] Error: Java heap space
	[mxmlc]
	[mxmlc] java.lang.OutOfMemoryError: Java heap space
	[mxmlc] 	at macromedia.asc.parser.NodeFactory.getExpression(NodeFactory.java:869)
	[mxmlc] 	at macromedia.asc.parser.NodeFactory.getExpression(NodeFactory.java:864)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseAttributeExpression(Parser.java:2213)
	[mxmlc] 	at macromedia.asc.parser.Parser.parsePostfixExpression(Parser.java:2028)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseUnaryExpression(Parser.java:2930)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseMultiplicativeExpression(Parser.java:2960)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseAdditiveExpression(Parser.java:3050)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseShiftExpression(Parser.java:3118)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseRelationalExpression(Parser.java:3196)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseEqualityExpression(Parser.java:3298)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseBitwiseAndExpression(Parser.java:3373)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseBitwiseXorExpression(Parser.java:3430)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseBitwiseOrExpression(Parser.java:3487)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseLogicalAndExpression(Parser.java:3544)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseLogicalXorExpression(Parser.java:3601)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseLogicalOrExpression(Parser.java:3658)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseConditionalExpression(Parser.java:3715)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseAssignmentExpression(Parser.java:3810)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseListExpression(Parser.java:3895)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseLabeledOrExpressionStatement(Parser.java:4352)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseAnnotatedDirectiveOrStatement(Parser.java:5287)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseDirective(Parser.java:5223)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseDirectives(Parser.java:5601)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseBlock(Parser.java:4398)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseBlock(Parser.java:4384)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseFunctionCommon(Parser.java:1623)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseFunctionDefinition(Parser.java:6514)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseAnnotatableDirectiveOrPragmaOrInclude(Parser.java:5532)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseAnnotatedDirectiveOrStatement(Parser.java:5341)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseDirective(Parser.java:5223)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseDirectives(Parser.java:5601)
	[mxmlc] 	at macromedia.asc.parser.Parser.parseBlock(Parser.java:4398)

    `BUILD FAILED
    ...`

What helped was to allow ANT to take more RAM by running

    $ export ANT_OPTS=-Xmx1024M

prior to the build.

When running Ant through Flex Builder and setting `ANT_OPTS` in your `.profile` doesn't work, you can pass an argument to the JRE when starting Ant

1.  Run Menu > External Tools > Open External Tools Dialog…
2.  Select your Ant build file on the left
3.  Click the JRE tab on the right
4.  Enter “-Xmx640m” in the VM arguments field ( without quotes )
5.  Click Apply, then Run

### Missing Ant in Flex Builder ###

Why Adobe chooses to include Ant Tasks with the SDK but not to install the Ant Tools for Eclipse by default is beyond me.

To install them separately

1.  Launch Flex Builder 3
2.  Go to `Help > Software Updates > Find and Install`
3.  Choose `Search for new features to install`, click `Next`
4.  Select `The Eclipse Project Updates`, click
    `Finish`;
5.  Let the Update Manager do the search and go grab a coffee
6.  In `Eclipse Project Updates > Eclipse SDK Eclipse 3.3.2`, select `Eclipse Java Development Tools`, or otherwise known as JDT, click `Next`
7.  Accept the license agreement, click `Next`;
8.  Click `Finish` to start the download process
9.  Click `Install all` to install Java Development Tools
10. Restart Flex Builder

## Links ##

### XPATH Library ###

[code.google.com/p/xpath-as3](http://code.google.com/p/xpath-as3/)
