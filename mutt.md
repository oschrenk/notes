# mutt

## Installation

```
brew install sqlite offline-imap urlview msmtp contacts notmuch
```

As of the day writing this `homebrew` didn't support the `sidebar-patch` for `mutt`.

You have to add it yourself

```
brew edit mutt
```

Scroll down to a section of commands that all start with "option", sort of like:

```
option "with-debug", "Build with debug option enabled"
option "with-trash-patch", "Apply trash folder patch"
option "with-s-lang", "Build against slang instead of ncurses"
option "with-ignore-thread-patch", "Apply ignore-thread patch"
option "with-pgp-verbose-mime-patch", "Apply PGP verbose mime patch"
option "with-confirm-attachment-patch", "Apply confirm attachment patch"
```

Add this line to the bottom of the options:

```
option "with-sidebar-patch", "Apply sidebar patch"
```

Scroll down further to the section with all the patches, e.g.

```
patch do
  url "http://patch-tracker.debian.org/patch/series/dl/mutt/1.5.21-6.2+deb7u1/features/trash-folder"
  sha1 "6c8ce66021d89a063e67975a3730215c20cf2859"
end if build.with? "trash-patch"
```

And add this block:

```
patch do
  url "https://raw.github.com/nedos/mutt-sidebar-patch/7ba0d8db829fe54c4940a7471ac2ebc2283ecb15/mutt-sidebar.patch"
  sha1 "1e151d4ff3ce83d635cf794acf0c781e1b748ff1"
end if build.with? "sidebar-patch"
```

Then install `mutt` with

TODO explain patches

```
brew install mutt --with-sidebbar-patch --with-confirm-attachment-patch --with-s-lang
```

## Configuration

### `offline-imap`

```
touch ~/.offlineimaprc
```


## Resources

Start with

http://stevelosh.com/blog/2012/10/the-homely-mutt/#mutt

Move on to

http://zanshin.net/2015/01/19/teaching-a-homely-mutt-new-tricks/

http://linsec.ca/Using_mutt_on_OS_X
https://github.com/zanshin/dotfiles/tree/master/mutt
http://jonmorehouse.tumblr.com/post/100650456885/yosemite-mutt
http://www.maclife.com/article/columns/terminal_101_using_mutt_email_client
http://garbage.world/posts/email/
http://baptiste-wicht.com/posts/2014/07/a-mutt-journey-download-mails-with-offlineimap.html
https://wiki.archlinux.org/index.php/OfflineIMAP
