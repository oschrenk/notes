# Rdio #

## "Flash is required in this browser" error message ##

View issue [online](http://help.rdio.com/customer/portal/questions/433093)

Right now there is no solution, to support the Rdio team

	defaults write com.rdio.desktop WebKitDeveloperExtras -bool true

- Open the Mac app, and log in
- Right-click on the white-space of the app, and select "Inspect element," then click over to the "Console" tab, then start playing music
- When you encounter the pop up, copy the last 100 or so lines from the console, and paste them into an email and send them to us for inspection
