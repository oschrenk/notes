# Exchange on OSX #

While native Exchange support is getting better for OSX, you might still have the pleasure to connect to an old Exchange Server that is not supported.

In this case [DavMail](http://davmail.sourceforge.net) can help. It's an application that serves as a proxy to the Outlook Web Access and exposes the services offered on that page as default Mail, LDAP, CalDav services on `localhost`.

You can just download the application and start it. It now runs in the menubar. For me there was no configuration necessary, except pasting the OWA URL. Your mileage may vary depending on your (network) configuration.

Now you can just setup your mailbox, CalDav server as if it were on a remote host, just use `localhost` and the appropriate port (normally just use an additional 1 in the beginning) to use the services.
