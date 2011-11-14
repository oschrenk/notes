# Eclipse #

## Configuration ##

Some default settings are weird, especially the encoding and line delimiter settings. Keep them consistent within your development team.

*   Deactivate Spell Checking. `General > Editors > Text Editors > Spelling`
*   Refresh source tree automatically `Preferences > General > Workspace > Refresh Automatically`
*   Change encoding `General -> Workspace > Text File Encoding -> Other -> UTF-8`
*   Line Delimiter to Unix `General -> Workspace -> New text file line delimiter -> Other -> Unix`

### Change the @author JavaDoc value ###

The default is that Eclipse uses the currently logged in username. To change the default settings edit the `eclipse.ini` and add

	-Duser.name=My Name

In OSX the  `eclipse.ini` can be found under `path/to/Eclipse.app/Contents/MacOS/eclipse.ini`

If you need some more flexibility e.g.. setting it depending on your workspace you might want to look into the usage of [JAutoDoc
](http://jautodoc.sourceforge.net/) and changing the default template and setting a property.

## Hints ##

### Show hidden (`.*`) files ###

In the project explorer there is a small triangle or arrow facing down. Pressing it will open the _View Menu_. Choose `Customize View > Filters` and deselect `.*-resources`

### Convert General Project to Java Project by adding Java Nature ###

Eclipse doesn't seem to offer a GUI variant of changing adding the nature of a project. You can do it manually by opening the relevant `.project` file and add (or change it to, if it is already there)

    <natures>
    	<nature>org.eclipse.jdt.core.javanature</nature>
    </natures>

## FAQ/Problems ##

### Delete obsolete workspaces under OS X ###

The used workspaces are stored in
`/Applications/eclipse/configuration/.settings/org.eclipse.ui.ide.prefs`

### Waiting For Virtual Machine To Exit ###

I had this error when I was trying to deploy a web application to a local JBoss Application Server. The problem was that the ANT home setting of Eclipse were pointing to non existing directories and files. So I removed the false entries and reset me ANT Home in `Preferences > Ant > Runtime`

### Out of Heap Space, Give Run/Debug Configurations more RAM ###

Goto `Run > [Run|Debug] Configurations` and select the configuration you want to change, select the `Arguments` tab and add sensible parameter to the `VM Arguments` box. For example `-Xms512m -Xmx768m`