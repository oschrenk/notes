# OS X

## What I like

*  OS has built in spell checker -- no need to to reproduce that feature for an application
    
## Hints

The hint I always give other mac users: Try pressing `Alt`. It works wonders. For example: _iTunes_ doesn't allow you to delete songs from your hard drive, when your are looking at a play list (not via `Cmd + Backspace` nor via context menu) pressing `Alt + Backspace` does the trick.

### Keyboard

#### Diacritical Marks

**é**

> `´ then e`

**è**

> `shift + ´ then e`

### Screenshots

*   Command-Shift-3: Take a screenshot of the screen, and save it as a file on the desktop
*   Command-Shift-4, then select an area: Take a screenshot of an area and save it as a file on the desktop
*   Command-Shift-4, then space, then click a window: Take a screenshot of a window and save it as a file on the desktop
*   Command-Control-Shift-3: Take a screenshot of the screen, and save it to the clipboard
*   Command-Control-Shift-4, then select an area: Take a screenshot of an area and save it to the clipboard
*   Command-Control-Shift-4, then space, then click a window: Take a screenshot of a window and save it to the clipboard
    
In Leopard, the following keys can be held down while selecting an area (via Command-Shift-4 or Command-Control-Shift-4):

*   Space, to lock the size of the selected region and instead move it when the mouse moves
*   Shift, to resize only one edge of the selected region
*   Option, to resize the selected region with its center as the anchor point
    
### Change folder in (save) dialogs

Press `Command + Shift + G`

### Aggregated information about multiple files/directories

Select files and directories and press `Alt + Cmd + I`

### Force expanded dialogs

These commands force expanded dialog boxes for saving (top) and printing (bottom), if an app doesn't already have a custom setting. Use 'false' to reverse the commands.

    $ defaults write -g NSNavPanelExpandedStateForSaveMode -boolean true
    $ defaults write -g PMPrintingExpandedStateForPrint -boolean true

### Remove Spotight icon from menu bar

    $ sudo chmod 600 /System/Library/CoreServices/Search.bundle/Contents/MacOS/Search

Restart the menu bar

    killall SystemUIServer

### Disable Dashboard

    $ defaults write com.apple.dashboard mcx-disabled -boolean yes

### Disable Rosetta

To disable Rosetta, simply run the following command in Terminal:

    $ sudo sysctl -w kern.exec.archhandler.powerpc=/usr/libexec/oah/RosettaNonGrata

To re-enable Rosetta after it's been disabled, just run the following command in Terminal:

    $ sudo sysctl -w kern.exec.archhandler.powerpc=/usr/libexec/oah/translate

These commands set a system environment variable that tells the system which program to run when you try to launch a non-native app: to ask you to install Rosetta, or to run it using Rosetta.

### Save Password when using Cisco VPN

Mac OS X asks you to manually enter the password every time you connect. TO change this behavior open the Keychain Access Application, select the System keychain and find your saved XAuth password entry in the list. Its Kind field will say IPSec XAuth Password.

Open it, then on the Access Control tab click the Plus button to add another application. The file we need to select, `/usr/libexec/configd`, resides in a hidden folder. To navigate there, press `Command-Shift-G`, enter `/usr/libexec`, then pick `configd` in the dialog. Save your changes and that's it --- your saved password should now work.

Depending on the security policy of the VPN Server this hint may not work.

### Problems decrunching zip archives formed `*.z00?`

1.  Change all the .z01, .z02, etc. file extensions to .001, .002, etc.
1.  Change the file extension of the .zip file to .00X (X= the last numeric file extension from the .001, .002, etc. files + 1)
1.  $ cat filename.00? \> filename.zip
1.  $ unzip filename.zip
    
The last command did output some warnings but it produced the correct file anyway

## Software

### Mail

#### IMAP -- Used Default folder from Mail host instead of Apple's

Mail needs to be configured which folders it needs to use for trash, drafts and spam, otherwise it will use its own folders leaving the other folder untouched and also annoyingly visible in the side bar.

Select the folder that matches the function and choose `Mailbox → Use This Mailbox For → [function name] (e.g. “Sent” again)`

### iTunes

To use `Command+F` for searching.

	$ defaults write com.apple.iTunes NSUserKeyEquivalents -dict-add "Target Search Field" "@F"

To disable Ping DropDown Box
	
	$ defaults write com.apple.iTunes hide-ping-dropdown 1 

To restore default Ping DropDown Box behaviour 

	$ defaults delete com.apple.iTunes hide-ping-dropdown 
	
Restore iTunes Arrows

	$ defaults write com.apple.iTunes show-store-link-arrows 1 

Let the arrows point to my own iTunes library. This allows you to search for song/artist/album in your library. Very handy!

	$ defaults write com.apple.iTunes invertStoreLinks 1 

### VLC

After upgrading to VLC 1.0.5 VLC didn't start anymore. The console showed

    09.02.10 9. Feb 00:41:46	[0x0-0xc7fc7f].org.videolan.vlc[67847]	[0x4722b8] main interface error: no interface module matched "globalhotkeys,none"

Resetting the configuration worked for me

    /Applications/VLC.app/Contents/MacOS/VLC --reset-config

## Using multiple accounts

### Share any files between users on the same Mac

1.  Create a new group. The easiest way to do this is through the Accounts pane in System Preferences. Just click on the Plus sign to add a new account and then select Group from the New Account drop-down menu. Call this group anything you want; I called mine friday. Add all the users who you want to participate in the file sharing to your newly-created group.
1.  Open Terminal:
    
    1.  `$ cd /Users/Shared`
    2.  `$ mkdir Local`.
    3.  `$ sudo chown admin:local Local`
    4.  Change the default permissions, if you wish: `sudo chmod 770 Local` (this is optional if you're happy with the default permissions).
    5.  Create the ACL entry for the new folder:  
        `$ sudo chmod +a "group:local allow delete,readattr,writeattr,readextattr,writeextattr,list,search,add_file,add_subdirectory,delete_child,file_inherit,directory_inherit" Local`
        
You now have a folder where all members of the group friday can read, write and delete files, as well as read, write to and create new sub folders. The ACL rule takes precedence over standard UNIX file permissions and is automatically inherited. It's this automatic inheritance that is really important. Now you are ready to copy your iTunes, Aperture, iPhoto libraries, plus anything else you want to share, into the shared folder.

IMPORTANT: You must copy (hold down Option in Finder prior to dragging), and not merely move, items. Moving items doesn't inherit the correct ACL rules. Moving doesn't change POSIX file atributes, permssions, ...

## FAQ/Problems

### The operation cannot be completed because the item \[whatever\] is in use.

Sometimes you will find OS not deleting a file from trash because it's in use. To find out which application "uses" the file

    $ lsof | grep [file]

Replace `[file]` with (part of) the filename which you can't remove from trash. It's case sensitive.

### Mac would always eject discs ###

Try resettings SMC

What helped me (for a few dvds) was to flip the Macbook 90° (no joke) and trying to load the disc. This is a strong indicator for having hardware issues.

## How to reset SMC ##

Try each of the following steps in this order before you reset the SMC.  Test the issue after completing each troubleshooting step to determine if the issue still occurs.

 - Press Command + Option + Escape to force quit any application that is not responding.
 - Put your Mac to sleep by choosing the Apple () menu from the upper-left menu bar and then choosing Sleep. Wake the computer after it has gone to sleep.
 - Restart your Mac by by choosing the Apple () menu from the upper-left menu bar and then choosing Restart.
 - Shut down your Mac by by choosing the Apple () menu from the upper-left menu bar and then choosing Shut Down.


Resetting the SMC on Mac portables with a battery you can remove

1. Shut down the computer.
2. Disconnect the MagSafe power adapter from the computer, if it's connected.
3. Remove the battery.
4. Press and hold the power button for 5 seconds.
5. Release the power button.
6. Reconnect the battery and MagSafe power adapter.
7. Press the power button to turn on the computer.