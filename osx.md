# OS X

## What I like

* **Window management**. Hierarchical approach.`cmd+[shift]+tab` switches applications, `cmd+[shift]+>` switches the windows of an application
* **Consistency**. `cmd+,` opens the settings of an application.`cmd+q` closes the application. `cmd+w` closes a window and only the window (!) - closing the last window doesn't close the application. 
    
## Hints

The hint I always give other mac users: Try pressing `Alt`. It works wonders. For example: _iTunes_ doesn't allow you to delete songs from your hard drive, when your are looking at a play list (not via `Cmd + Backspace` nor via context menu) pressing `Alt + Backspace` does the trick.

### Keyboard

#### Diacritical Marks

The normal approach on keboard layout with [dead keys](http://en.wikipedia.org/wiki/Dead_key) (like the german keyboard), would be to the following

- **é**, press `´` then `e`
- **è**, press `shift + ´ ` then `e`

If you have set up your keyboard to don't use dead keys, you can use OS C 10.7 feature th old down the appropiate base key, such as `e` and wait a second or two for a small pop up window to appear.

### Screenshots

*   `Command-Shift-3`: Take a screenshot of the screen, and save it as a file on the desktop
*   `Command-Shift-4`, then select an area: Take a screenshot of an area and save it as a file on the desktop
*   `Command-Shift-4, Space`, then click a window: Take a screenshot of a window and save it as a file on the desktop
*   `Command-Control-Shift-3`: Take a screenshot of the screen, and save it to the clipboard
*   `Command-Control-Shift-4`, then select an area: Take a screenshot of an area and save it to the clipboard
*   `Command-Control-Shift-4, Space`, then click a window: Take a screenshot of a window and save it to the clipboard
    
In Leopard, the following keys can be held down while selecting an area (via `Command-Shift-4` or `Command-Control-Shift-4`):

*   Space, to lock the size of the selected region and instead move it when the mouse moves
*   Shift, to resize only one edge of the selected region
*   Option, to resize the selected region with its center as the anchor point
    
### Change folder in (save) dialogs

Press `Command + Shift + G`

### Aggregated information about multiple files/directories

Select files and directories and press `Alt + Cmd + I`

### List known WLAN ###

	defaults read /Library/Preferences/SystemConfiguration/com.apple.airport.preferences KnownNetworks | egrep -o '(SSID_STR|_timeStamp).+' | sed 's/^.*= \(.*\);$/\1/' | sed 's/^"\(.*\)"$/\1/' | sed 's/\([0-9]\{4\}-..-..\).*/\1/' | sed 'N;s/\n/: /'

### Save Password when using Cisco VPN

Mac OS X asks you to manually enter the password every time you connect. TO change this behavior open the Keychain Access Application, select the System keychain and find your saved XAuth password entry in the list. Its Kind field will say IPSec XAuth Password.

Open it, then on the Access Control tab click the Plus button to add another application. The file we need to select, `/usr/libexec/configd`, resides in a hidden folder. To navigate there, press `Command-Shift-G`, enter `/usr/libexec`, then pick `configd` in the dialog. Save your changes and that's it --- your saved password should now work.

Depending on the security policy of the VPN Server this hint may not work.

### Problems decrunching zip archives formed `*.z00?`

1.  Change all the `.z01`, `.z02`, etc. file extensions to `.001`, `.002`, etc.
2.  Change the file extension of the `.zip` file to `.00X` (`X`= the last numeric file extension from the `.001`, `.002`, etc. files + 1)
3.  `$ cat filename.00? \> filename.zip`
4.  `$ unzip filename.zip`
    
The last command did output some warnings but it produced the correct file anyway

## Software

### OSX Mail - Setting  mail folders 

Mail needs to be configured which folders it needs to use for trash, drafts and spam, otherwise it will use its own folders leaving the other folder untouched and also annoyingly visible in the side bar.

Select the folder that matches the function and choose `Mailbox → Use This Mailbox For → [function name] (e.g. “Sent” again)`
:
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

IMPORTANT: You must copy (hold down Option in Finder prior to dragging), and not merely move, items. Moving items doesn't inherit the correct ACL rules. Moving doesn't change POSIX file attributes, permissions, ...

## FAQ/Problems

### The operation cannot be completed because the item \[whatever\] is in use.

Sometimes you will find OS not deleting a file from trash because it's in use. To find out which application "uses" the file

    $ lsof | grep [file]

Replace `[file]` with (part of) the filename which you can't remove from trash. It's case sensitive.

### Clean up double _Open with..._ entries ###

After I cloned my harddrive the _Open with..._ menu was showing double entries. Rebuilding my LaunchServices database helped:

	/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user

### Show hidden files in Dialogs ###

Press `⌘⇧.` and the the dialog box now will display any hidden files or folders within its list items. You can toggle between the hidden files and folders being displayed by pressing it again.

### OSX freezes when changing network locations ###

- http://reviews.cnet.com/8301-13727_7-10329065-263.html?tag=mfiredir
- https://discussions.apple.com/thread/1086078?start=15&tstart=0
- http://reviews.cnet.com/8301-13727_7-10329060-263.html

### Report problems ###

Apple offers a [bug reporting tool](https://bugreport.apple.com/) whre you can submit your bugs.