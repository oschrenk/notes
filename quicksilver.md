# Quicksilver #

## Usage ##

To relaunch Quicksilver

    Command-Control-Q

### Text input mode ###

press period `.`

## Useful triggers ##

### Access latest download ###

In order to enable “latest download” you need to turn on advanced features (in the main preferences tab) and proxy objects in `Quicksilver Preferences > Catalog > Quicksilver > Proxy Objects`

Something I was not aware of is that Quicksilver can access your latest download simply by you typing `latest download`! This is really handy and something I’ll be using a lot. I instantly made that into a trigger set to `⌘-⌥-^-L`.

## Quickly start Quicksilver ##

If for some reason quicksilver has crashed you can quickly start/restart it using this:

1. Open Automator, and select 'Service' when it asks you what kind of 
workflow you'd like to create.
2. Change the 'Service receives selected' menu to 'No input'.
3. Drag the 'Launch Application' action onto the workflow.
4. Under the drop-down menu, choose 'Quicksilver'
5. Save your workflow with a name like 'LaunchQuicksilver'
6. Close Automator, and open up the 'Keyboard' preference pane in System 
Preferences.
7. Click the 'Keyboard Shortcuts' tab, and select 'Services' from the 
sidebar.
8. Scroll down until you see the 'LaunchQuicksilver' service that you 
created.
9. Make sure that it's checked, and then double-click on the empty space to 
the right of it, and you'll be able to choose a shortcut.
10. Voila! A makeshift trigger that allows you to open QS.

## FAQ/Problems ##

## Define directories as content types ##

Just type

    'fold' 

in the "Types" field with the apostrophe
