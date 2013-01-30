# Homebrew #

- `brew update` Fetch the newest version of Homebrew from GitHub using git.
- `brew upgrade` Upgrade outdated brews.

## Usage ##

Use `man brew` to view the manpage.

| Command 								| Consequence 	|
| ------------------------------------- | -----------:-	|
| `brew install foo`					| Installs foo 	|
| `brew search`							| Lists all formula that you can install |
| `brew search foo`						| Searches for foo in formula available to install |
| `brew search /foo/`					| Same as above but parses /foo/ as a regex |
| `brew list` 				  			| Lists all installed formulae |
| `brew list foo`			  			| Lists the installed files for foo |
| `brew info --github foo` 	  			| Open your browser at the Github History page for formula foo |
| `brew info` 				  			| Summarises all installed Homebrew packages |
| `brew info foo` 			  			| Gives all available information for installed or not installed formula foo |
| `brew home` 				  			| Open's Homebrew's homepage in your default browser |
| `brew home foo` 			  			| Opens foo's homepage in your default browser |
| `brew update` 						| Update homebrew formulae and homebrew itself |
| `brew remove foo`						| Uninstalls foo |
| `brew create [url]` 					| Generates a formula for the downloadable file at url and opens it in TextMate [^1] |
| `brew create url-of-tarball --cache` 	| Generates a formula, then downloads the tarball. Adds the md5 to the formula template for you |
| `brew create --macports foo` 			| Open your browser at the MacPorts package search page, so you can see how they do foo |
| `brew create --fink foo`				| Same thing, but for Fink |
| `brew edit foo`						| Opens the formula in TextMate |
| `brew link foo`						| Symlinks all of foo's installed files into the Homebrew prefix [^2] |
| `brew unlink foo`						| Unsymlinks foo from the Homebrew prefix |
| `brew prune`							| Removes dead symlinks from the Homebrew prefix [^3] |
| `brew outdated` 						| Shows formula that have an updated version available . To install the new version of foo: @brew install foo`|
| `brew --config` 						| Print some useful system configuration to the console |
| `brew --prefix` 						| Display the real path to your Homebrew prefix (Usually /usr/local) |
| `brew --prefix (formula)`				| Display the path where this formula is installed |
| `brew --cellar` 						| Display the real path to your Homebrew Cellar (Usually `/usr/local/Cellar`)  |
| `brew --cache` 						| Display the real path to where Homebrew caches downloads (Usually ~/Library/Caches/Homebrew)  |
| `brew doctor` 						| Checks your installation for common issues |
| `brew audit` 							| Audits all formulae for common code and style issues |
| `brew cleanup foo` 					| For all installed or specific formulae, removes any older versions from the cellar [^4] |

## Homebrew in a Multi-User Environment ##

Create a new group called `Local` (name is irrelevant)

The easiest way to do this is through the Accounts pane in System Preferences. Just click on the Plus sign to add a new account and then select Group from the New Account drop-down menu. Add all the users who you want to participate in your newly-created group.

Homebrew uses two directories (per default)

1. `/usr/local` as installation directory
2. `/Library/Caches/Homebrew` as cache (`$ brew --cache`)

Both directories should be made available to `Local`. While you don't want to mess with the current contents of `/usr/local`, we can safely delete the cache, so that there won't be any permission problems.

Open the Terminal

    $ sudo chown admin:Local /usr/local
    $ sudo mkdir -p /Library/Caches/Homebrew
    $ sudo rm -rf /Library/Caches/Homebrew/*

Change the default permissions, if you wish: `sudo chmod 770 Local` (this is optional if you're happy with the default permissions).

Change the ACL entry for the directories:

    $ sudo chmod +a "group:Local allow delete,readattr,writeattr,readextattr,writeextattr,list,search,add_file,add_subdirectory,delete_child,file_inherit,directory_inherit" /usr/local
    $ sudo chmod +a "group:Local allow delete,readattr,writeattr,readextattr,writeextattr,list,search,add_file,add_subdirectory,delete_child,file_inherit,directory_inherit" /Library/Caches/Homebrew

Now all users in the local group should be able to brew.

IMPORTANT: You must copy (hold down Option in Finder prior to dragging), and not merely move, items. Moving items doesn't inherit the correct ACL rules. Moving doesn't change POSIX file attributes, permissions, ...

## FAQ/Problems ##

### Alternative Formulae ###

The official repo doesn't accept all formulae for one reason or another. You can find some of these in (https://github.com/adamv/homebrew-alt

### brew doctor ###

I had some problems with the PHP installation and ran `brew doctor` to check for problems.

1. Added `/usr/local/sbin` and `/usr/local/bin` to path before system bin directory
2. Uninstalled XQuarz to remove config scripts

[^1]: Homebrew will attempt to automatically derive the formula name and version, if it fails, you'll have to make your own template. I suggest copying wget's.
[^2]: This is done automatically when you install formula. It is useful for DIY installation, or in cases where you want to swap out different versions of the same package that you have installed at the same time.
[^3]: This is generally not needed. However, it can be useful if you are doing DIY installations.
[^4]: To delete only a specific out of date version, just go to the folder in the Cellar and `rm -rf` it, or drag it to the trash in Finder.

### Can't update ###

I had some troubles updating my homebrew via `brew update`. It always produced an error message about some untracked data.

I fixed this by

	cd /usr/local
	git reset --hard

adding

	[branch "master"]
		remote = origin
		merge = refs/heads/master
	[remote "origin"]
	    url = http://github.com/mxcl/homebrew.git
	    fetch = refs/heads/*:refs/remotes/origin/*

to the `.git/config` file

	git pull

### Merge conflicts during update ###

Somehow my tree and remote diverged and `brew update` threw conflict errors at me. I reset the upstream via

	cd $(brew --prefix)
	git remote add origin git://github.com/mxcl/homebrew.git
	git fetch origin
	git reset --hard origin/master

### Get revison of homebrew ###

You can get the version of homebrew via

	brew --version

To get the revision just

	brew log -1 --format="%H"
