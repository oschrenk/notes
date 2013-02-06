# Eclipse #

## Plugins ##

| Name | Description | Update Site |
| :---- | :---- | :---- |
| [Ecl Emma](http://www.eclemma.org/) | Code coverage | [Update Site](http://update.eclemma.org/) |
| [EGit](http://www.eclipse.org/egit/) | Git Team Provider | [Update Site](http://download.eclipse.org/egit/updates) |
| [JAutoDoc](http://jautodoc.sourceforge.net/) | Automatically add Javadoc  | [Update Site](http://jautodoc.sourceforge.net/update/) |
| [m2eclipse](http://eclipse.org/m2e/) | Apache Maven support in the Eclipse IDE | [Update Site](http://download.eclipse.org/technology/m2e/releases) |
| [Maven Inspector](http://prapicault.github.com/MavenInspector/) | Provides an eclipse view called "Maven Execution" that presents for a given pom the phases and associated MOJOs | [Update Site](http://prapicault.github.com/MavenInspector/repository)
| [PMD](http://pmd.sourceforge.net/integrations.html#eclipse) | Static Code analysis | [Update Site](http://pmd.sf.net/eclipse) |
| [Workspace Mechanic](http://code.google.com/a/eclipselabs.org/p/workspacemechanic/) | Workspace Mechanic automates maintenance and migration of your Eclipse environment in your company | [Update Site](http://workspacemechanic.eclipselabs.org.codespot.com/git.update/mechanic/). [Beta Update Site](http://workspacemechanic.eclipselabs.org.codespot.com/git.update/mechanic-testing/) |
|[startexplorer](https://github.com/basti1302/startexplorer)| File Manager and Shell integration | [Update Site](http://basti1302.github.com/startexplorer/update/) |

## Configuration ##

Some default settings are weird, especially the encoding and line delimiter settings. Keep them consistent within your development team.

*   Deactivate Spell Checking. `General > Editors > Text Editors > Spelling`
*   Refresh source tree automatically `Preferences > General > Workspace > Refresh Automatically`
*   Change encoding `General -> Workspace > Text File Encoding -> Other -> UTF-8`
*   Line Delimiter to Unix `General -> Workspace -> New text file line delimiter -> Other -> Unix`

### Commit from project explorer with keyboard shortcut ###

[From Stackoverflow](http://stackoverflow.com/questions/8469313)

Ensure `Git` is checked within the `Command Groups Availability` tab of the `Window > Customize Perspective menu`.

### Change the @author JavaDoc value ###

The default is that Eclipse uses the currently logged in username. To change the default settings edit the `eclipse.ini` and add

	-Duser.name=My Name

In OSX the  `eclipse.ini` can be found under `path/to/Eclipse.app/Contents/MacOS/eclipse.ini`

If you need some more flexibility e.g.. setting it depending on your workspace you might want to look into the usage of [JAutoDoc](http://jautodoc.sourceforge.net/) and changing the default template and setting a property.

### Show hidden (`.*`) files ###

In the project explorer there is a small triangle or arrow facing down. Pressing it will open the _View Menu_. Choose `Customize View > Filters` and deselect `.*-resources`

### Convert General Project to Java Project by adding Java Nature ###

Eclipse doesn't seem to offer a GUI variant of changing adding the nature of a project. You can do it manually by opening the relevant `.project` file and add (or change it to, if it is already there)

    <natures>
    	<nature>org.eclipse.jdt.core.javanature</nature>
    </natures>

## Appendix ##

To get to "New and Noteworthy" section open the [Download Overview](http://download.eclipse.org/eclipse/downloads/index.html), click on the Milestone (under Stable Build), and click on the "New and Noteworthy" link (that looks like a header)

### Notable Changes in 3.7 ###

A new lightweight refresh mechanism was introduced. Files discovered to be out-of-sync by the workspace, for example while accessing the file content by an editor, are automatically asynchronously refreshed. In Eclipse 3.7 and 3.8 you have to enable this yourself via Preferences > General > Workspace and select Refresh on access. In Eclipse SDK 4.2 the mechanism is enabled by default.

### Notable Changes in 3.8 ###

- Quick Assist to convert enhanced `for` loop.
	- Convert to indexed `for` loop is available for expressions of `array`- and `List`-based types.
	- Convert to Iterator-based `for` loop is available for expressions of type `Iterable`
- Resource Leak Detection. The compiler can now detect leakage of resources i.e. local variables of type `java.lang.AutoCloseable` (compliance >= 1.7) and `java.io.Closeable` (compliance <= 1.6).
- Annotation-based null analysis. JDT can now be configured to use annotations designated as null annotations to perform enhanced inter-procedural null analysis. This feature can be enabled in `Preferences > Java > Compiler > Errors/Warnings > Null analysis`
- Null analysis for fields. JDT can now raise null related errors/warnings for fields. You can configure null analysis for fields in Preferences > Java > Compiler > Errors/Warnings > Null analysis.
- Export detail formatters. Detail formatters can now be exported as separate preferences.
- Improved bracket matching support in Java editor. Also, the `Navigate > Go To > Matching Bracket action (Ctrl+Shift+P)` now works everywhere in a file. If a bracket is not selected before invoking the action, the action navigates to the nearest enclosing end bracket.
- Enhanced diagnostics for detection of incomplete switch statements
- Toggle Full Screen command is now supported on MacOS X Lion

### Notable Changes in 4.3 ###

- The new Convert `if-else` to `switch` quick assist (`Ctrl+1`) allows you to convert an `if-else` statement to an equivalent `switch` statement.
- The compiler can now detect unused type parameters. The Unused type parameter diagnostic is set to Ignore by default and can be enabled on the `Java > Compiler > Errors/Warnings preference page.
- Performance enhancements (M4)
- Quick assist to combine Strings
- Option to inherit null annotations
- Null annotations analysis for fields

### FAQ/Problems ###

#### Delete obsolete workspaces under OS X ####

The used workspaces are stored in
`/Applications/eclipse/configuration/.settings/org.eclipse.ui.ide.prefs`

#### Waiting For Virtual Machine To Exit ####

I had this error when I was trying to deploy a web application to a local JBoss Application Server. The problem was that the ANT home setting of Eclipse were pointing to non existing directories and files. So I removed the false entries and reset me ANT Home in `Preferences > Ant > Runtime`

#### Out of Heap Space, Give Run/Debug Configurations more RAM ####

Goto `Run > [Run|Debug] Configurations` and select the configuration you want to change, select the `Arguments` tab and add sensible parameter to the `VM Arguments` box. For example `-Xms512m -Xmx768m`

#### Unexpected element "{}assembly" {antlib:org.apache.tools.ant}assembly Ant Buildfile Problem ####

This seems to be a bug in Eclipse. Rightclick and delete it.

To disable validation for good:

1. Select the Validation category in the left pane.
2. Find the Validator named "XML Validator" (in the right pane) and click its ellipses (...) button.
3. In the following dialogue, select the "Exclude Group" and click "Add Rule...".
4. On the first page of the New Filter Rule Wizard, select the Content Type option and click Next>.
5. Finally, on the Content Type drop down, select "Ant Buildfile".
6. Click Finish and a couple of OKs and that should take care of existing and future Ant Build file validation warnings.
