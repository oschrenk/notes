# Firefox

## Development ##

## Make extensions work with newer version of Firefox

* Type `about:config` into Firefox's address bar and click the "I'll be careful, I promise!" button.
* Right-click anywhere. Choose `New > Boolean`. Make the name of your new config value `extensions.checkCompatibility` and set it to false.
* Make another new boolean pair called `extensions.checkUpdateSecurity` and set the value to false.
* Restart Firefox.