# Exchange



Can be accessed in multiple ways depending on your Exchange Version and your installation.



Exchange Server can be exposed via OutlookWebAccess and accessed via WebDav. Newer Versions (Exchange 2007+) also exposes itself as a WebService.



## Email



http://tools.ietf.org/html/rfc5322



- `MIME` Extends format of Email such as Non-text, multiple parts.

- `Content-ID` Header. Is a unique identifier. Mainly used for multi-part messages, allowing to reference each part.

- `Content-Type`. Indicates Internet Media Type e.g.. `plain/text`

- `Content-Disposition`. Can define presentation style. For example whether to handle part as `inline` or as `attachment` or defining name of the file, the creation date and modification date

- `Content-Transfer-Encoding`. 7bit, quoted-printable, base64, 8bit, binary. Legacy use normally dictates to use base64 or quoted-printable



### Multipart



A MIME multipart message contains a boundary in the "Content-Type: " header; this boundary, which must not occur in any of the parts, is placed between the parts, and at the beginning and end of the body of the message, as follows:



	MIME-Version: 1.0

	Content-Type: multipart/mixed; boundary="frontier"



	This is a message with multiple parts in MIME format.

	--frontier

	Content-Type: text/plain



	This is the body of the message.

	--frontier

	Content-Type: application/octet-stream

	Content-Transfer-Encoding: base64



	PGh0bWw+CiAgPGhlYWQ+CiAgPC9oZWFkPgogIDxib2R5PgogICAgPHA+VGhpcyBpcyB0aGUg

	Ym9keSBvZiB0aGUgbWVzc2FnZS48L3A+CiAgPC9ib2R5Pgo8L2h0bWw+Cg==

	--frontier--





#### Multipart/mixed



[rfc2046](http://www.ietf.org/rfc/rfc2046.txt)



Multipart/mixed is used for sending files with different "Content-Type" headers inline (or as attachments). If sending pictures or other easily readable files, most mail clients will display them inline (unless otherwise specified with the "Content-disposition" header). Otherwise it will offer them as attachments. The default content-type for each part is "text/plain".



#### Multipart/related



[rfc2387](http://www.ietf.org/rfc/rfc2387.txt)



The `Multipart/Related` media type is intended for compound objects consisting of several inter-related body parts. For a `Multipart/Related` object, proper display cannot be achieved by individually displaying the constituent body parts.



Exchange handles multipart/related specially - i.e. it considers all attachment parts inside multipart/related as inline. Such attachments are normally hidden from the attachment list and supposed to be accessible from the body itself, like inline images. Some clients, like OWA, can determine whether attachments are really inline by analyzing a message body - if they don't find any reference to such attachment in a body they fix it by displaying it in attachment list. Other clients like Outlook will trust how attachments are marked by Exchange and hide them.



Correct use of multipart/related



	Multipart/mixed

		Multipart/related

			Text/html - message body

			inline attachments referenced from the body

		Any normal attachments, like application/msword



Normal attachments should appear outside of multipart/related.



## WebDav



- [rfc2518](http://www.ietf.org/rfc/rfc2518.txt)

- [OWA via WebDav](http://msdn.microsoft.com/en-us/library/aa486282(v=EXCHG.65).aspx)



## Solutions ##



### fetchExc ###



[FetchExc](http://www.saunalahti.fi/juhrauti/index.html) is java utility which retrieves mail from your MS Exchange (2000/2003) inbox and forwards it to SMTP server of your choice or mbox type file. FetchExc uses webDAV to retrieve mail either over http or https



### DavMail ###



[DavMail](http://davmail.sourceforge.net/) is a POP/IMAP/SMTP/Caldav/LDAP exchange gateway allowing users to use any mail/calendar client (e.g. Thunderbird with Lightning or Apple iCal) with an Exchange server, even from the internet or behind a firewall through Outlook Web Access



