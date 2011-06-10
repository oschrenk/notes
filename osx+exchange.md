# Exchange on OSX #

As Exchange support is growing for OSX, I still had problems getting access in an Exchange in a Exchange 2003 environment - even the Exchange IMAP implementation, which I suspected to work with the standard IMAP implementation, didn't work.

## DavMail ##

In the end "DavMail":http://davmail.sourceforge.net/ came to the rescue. It's an application that serves as a proxy to the Outlook Web Access and exposes the services offered on that page as default Mail, LDAP, CalDav services on localhost.

You can just download the application and start it. It now runs in the menubar. For me there was no configuration necessary, except pasting the OWA URL. Your mileage may vary depending on your (network) configuration.

Now you can just setup your mailbox, CalDav server as if it were on a remote host, just use localhost and the appropriate port (normally just use an additional 1 in the beginning) to use the services.

The only problem I had was advising mail to use password authentication when using SMTP.