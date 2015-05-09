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

```
brew install mutt --with-sidebbar-patch --with-confirm-attachment-patch --with-s-lang
```



## Configuration

### `offline-imap`

```
touch ~/.offlineimaprc
```

Edit the file and add

```
[general]
ui = TTY.TTYUI
accounts = platzhaltr
pythonfile=~/.mutt/offlineimap.py
fsync = False

[Account platzhaltr]
localrepository = platzhaltr-local
remoterepository = platzhaltr-remote
status_backend = sqlite
postsynchook = notmuch new

[Repository platzhaltr-local]
type = Maildir
localfolders = ~/.mail/platzhaltr
nametrans = lambda folder: {'drafts':  '[Gmail]/Drafts',
                            'sent':    '[Gmail]/Sent Mail',
                            'flagged': '[Gmail]/Starred',
                            'trash':   '[Gmail]/Trash',
                            'archive': '[Gmail]/All Mail',
                            }.get(folder, folder)

[Repository platzhaltr-remote]
maxconnections = 1
type = Gmail
remoteuser = oliver.schrenk@platzhaltr.com
remotepasseval = get_keychain_pass(account="oliver.schrenk@platzhaltr.com", server="imap.gmail.com")
realdelete = no
nametrans = lambda folder: {'[Gmail]/Drafts':    'drafts',
                            '[Gmail]/Sent Mail': 'sent',
                            '[Gmail]/Starred':   'flagged',
                            '[Gmail]/Trash':     'trash',
                            '[Gmail]/All Mail':  'archive',
                            }.get(folder, folder)
```

Then we need to create `.mutt/offlineimap.py`

```
mkdir ~/.mutt/
touch ~/.mutt/offlineimap.py
```

Edit this file

```python
#!/usr/bin/python

import re, subprocess

def get_keychain_pass(account=None, server=None):
    params = {
        'security': '/usr/bin/security',
        'command': 'find-internet-password',
        'account': account,
        'server': server,
        'keychain': '/Users/`whoami`/Library/Keychains/login.keychain',
    }

    command = "sudo -u `whoami` %(security)s -v %(command)s -g -a %(account)s -s %(server)s %(keychain)s" %params
    output = subprocess.check_output(command, shell=True, stderr=subprocess.STDOUT)
    outtext = [l for l in output.splitlines()
                     if l.startswith('password: ')][0]

    return re.match(r'password: "(.*)"', outtext).group(1)
```

Create a directory for `platzhalter`

```
mkdir -p ~/.mail/platzhaltr
```

Create passwords in your keychain

```
security add-generic-password -U -s "imap.gmail.com" -a "oliver.schrenk@platzhaltr.com" -w "password" "/Users/oliver/Library/Keychains/login.keychain"
security add-generic-password -U -s "smtp.gmail.com" -a "oliver.schrenk@platzhaltr.com" -w "password" "/Users/oliver/Library/Keychains/login.keychain"

```

You need certificate. Any dummy will do

http://mercurial.selenic.com/wiki/CACertificates#Mac_OS_X_10.6_and_higher

```
openssl req -new -x509 -extensions v3_ca -keyout /dev/null -out dummycert.pem -days 3650
cp dummycert.pem ~/.mutt/dummycert.pem
```

You need to setup notmuch

```
notmuch setup
```

which creates `~/.notmuch-config`

Create `/User/oliver/.mail`

```
mkdir ~/.mail



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
