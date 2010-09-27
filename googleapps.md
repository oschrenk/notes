# Google Apps #

## Signing Up ##

https://www.google.com/a/cpanel/domain/new

offers you two options, to use an existing domain or to buy a domain name for 10$ a year.

As I already own the domain i just enter the domain name and select the `Administrator: I own or control this domain` box. 

You have to fill out some infos about you (forename, surname, email address, phone number ) and the organizsation you represent.

After you register you have to verify the domain. There are several possibilities

* through Upload of html file http://www.google.com/support/a/bin/static.py?page=guide.cs&hl=en&guide=22229&topic=22391
*  through CNAME entry http://www.google.com/support/a/bin/answer.py?answer=47283

## Set up users ##

I only did that via the web user interface - one by one. I just copied the initial passwords into some text file and informed each user of their new username/pasword.

## Access to your services ##

You can always access your account with the URLs below:

* Control Panel: http://www.google.com/a/your-domain.com
* Mail: http://mail.google.com/a/your-domain.com
* Calendar: http://calendar.google.com/a/your-domain.com
* Docs: http://docs.google.com/a/your-domain.com
* Sites: http://sites.google.com/a/your-domain.com

In each case, replace "your-domain.com" with your actual domain name. 

You can also create custom convenience urls which redirect to a service. 

For example you can redirect http://mail.domain.com redirects to https://mail.google.com/a/domain.com. To do that you have to control the DNS Settings of your domain.

First you have to inform Google Apps of the domains. You can do that at https://www.google.com/a/cpanel/acme.de/CustomUrl.

If you have registered the domain with one of googles partners you are done, otherwise you have o manually enter the data.

Unfortunately each domain provider handles these things differently, so you have to do some digging yourself. But you have to create a `CNAME` entry with the value `ghs.google.com.` (there is a dot at the end).

**WARNING** I know that http://www.google.com/support/a/bin/static.py?page=guide.cs&guide=22229&topic=22391#149095 says differently, but it only worked for me with the dot at the end. Some other users also report the same http://www.im-web-gefunden.de/2007/05/14/cname-im-nameserver-richtig-konfiguriert-die-sache-mit-dem-punkt/

As I only use the main four services (for now) I just used:

| Name 		| Type 	| Value 			| 
----------- | :---: | ----------------: |
mail 		| CNAME	| ghs.google.com.	|
calendar	| CNAME	| ghs.google.com.	|
sites		| CNAME	| ghs.google.com.	|
docs		| CNAME	| ghs.google.com.	|
[CNAME entries]

## Activating Email Services ##

I only investigated on how to switch services and not how to do phased deployments. You will need to inform the users of the change, asking them to backup their data just in case and plan some time on where the changes will be made.

You have to change the DNS settings of your domain.

| Name 		| Type 	| Value 					| Priority	|
----------- | :---: | ------------------------: | :-------: |
			| MX	| ASPMX.L.GOOGLE.COM.		| 10		|
			| MX	| ALT1.ASPMX.L.GOOGLE.COM.	| 20		|
			| MX	| ALT2.ASPMX.L.GOOGLE.COM.	| 20		|
			| MX	| ASPMX2.GOOGLEMAIL.COM.	| 30		|
			| MX	| ASPMX3.GOOGLEMAIL.COM.	| 30		|
			| MX	| ASPMX4.GOOGLEMAIL.COM.	| 30		|
			| MX	| ASPMX5.GOOGLEMAIL.COM.	| 30		|
[MX entries]

## Settings ##

### Domain Settings ###

https://www.google.com/a/cpanel/acme.com/DomainSettings

* Set the Default Language, Time Zone
* Set Control Panel to current version

## Services ##

### Email ###

#### Set up language ####

#### Set up Imap ####

#### Forwarding Emails ####

#### Adding Email alias ####

#### Send as... ####

Mail > Settings > Accounts Send Mails as
Mail > Einstellungen > Konten > Email senden als > Weitere E-Mail-Adressse hinzufügen

#### Internal mailing list ####

https://www.google.com/a/cpanel/acme.com/CreateGroup

Group name *
: Descriptive Name, for example `Acme Inc.`
Group email address * 
: Identifier, for example `internal` (@acme.com)

Set the `Access Level` to `Team` (should be preselected) and check the box `Add all users within Acme Inc. to this group`

### Calendar ###

#### Change Language ####

#### Change Time Format/Week Start Time 
