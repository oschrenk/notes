# SVN Developer Handbuch

## SVN installieren

	mac ports
	subversion-javahlbindings

## Subversive

Subversive ist das offizielle Projekt der Eclipse Foundation, dass die Eclipse Plattform um einen Subversion Team Provider erweitert. Es ist nicht Teil der Standarddistribution, so dass es nachinstalliert werden muss. 

Leider ist die Installation ein wenig anstrengender als sie sein sollte, da Subversive in 2 Teile aufgesplittert ist, dem Plug-In und dem Connector. Während das Plug-In auf der Seite des Eclipse Projektes gehostet wird, ist der Connector auf der Seite von Polarion[1].

Die Downloads bzw. Links zu der Update Seite des Plug-Ins sind hier:

[http://www.eclipse.org/subversive/downloads.php](http://www.eclipse.org/subversive/downloads.php)

Die Update Seite des Plug-Ins:

[http://download.eclipse.org/technology/subversive/0.7/update-site](http://download.eclipse.org/technology/subversive/0.7/update-site/)


Den Connector kann man hier herunterladen:

[http://www.polarion.org/projects/subversive/download/eclipse/2.0/ganymede-site/](http://www.polarion.org/projects/subversive/download/eclipse/2.0/ganymede-site/)

Folgende Pakete müssen installiert werden:

	Polarion Update Site
	> Subversive SVN Connectors
		> Subversive SVN Connectors
		> SVNKit 1.2.0 Implementation (Optional)
		
oder
		
		> Native JavaHL 1.5 Implementation (Optional)

		Subversive Update Site
	> Subversive SVN Team Provider (Incubation)
		> Subversive SVN Team Provider (Incubation)


Problem mit Subversive. Native JAVA Libraries sollen nicht da sein, trotz
	
	sudo port install subversion-javahlbindings

Probeweise

	ln -s /opt/local/share/java/svn-javahl.jar /Library/Java/Extensions/

Auch gerne mal einfach auswählen und probieren. Trotz der Anzeige das sie nicht geladen sind hat es geklappt.

[http://www.nabble.com/JavaHL-%28JNI%29-Not-Available---On-Mac-OS-X-td19399014.html]()


#SVN Server einrichten

#Passwortschutz

#SSL

##Einrichten von SSL

##Nutzen von SSL geschützten Repositories

Es empfiehlt sich das CA Zertifikat als vertrauenswürdig einzustufen. man besorgt sich also das CA Zertifikat vom admin und legt es irgendwo ab, wo man es wieder findet.

Die Datei

	$ ~/subversion/servers


editieren.

Und die (normalerweise) auskommentierte Zeile `ssl-authority-files` suchen. Kommentar entfernen und Pfad zu Zertifikat hinzufügen

Die Datei wird auch von *Subversive* ausgelesen.

# Grundbegriffe und Eigenschaften

Billige Kopien
: Jede Datei im Subversion existiert nur einmal. Ansonsten werden nur Referenzen bzw. Änderungen gespeichert. Jede Revision ist eine billige Kopie.

### SVN

#### Eclipse SVN Client

Mir ist danach Eclipse erst einmal abgestürzt - Eclipse und die lieben Plugins... irgendwann finde ich nochmals eine Möglichkeit sie ordentlich zu managen.

Fehlermeldung:

	Location Information has been specified incorrectly.
	svn: PROPFIND of '/personal': 405 Method not allowd (http://localhost)
	Keep location anyway?

* Userrechte?
* Firewall (Little Snitch?)
* Falsche Pfadangabe? Sicher das `http://localhost/svn/REPO` angegeben wurde?

Prüfen ob repo über Browser erreichbar

In `/etc/apache2/other/svn.conf`:

	LoadModule dav_svn_module /usr/libexec/apache2/mod_dav_svn.so
	<Location /svn>
	  DAV svn
	  SVNParentPath /usr/local/svn
	AuthType Basic
	AuthName "SVN"
	AuthUserFile /etc/apache2/auth/svn
	Require valid-user
	</Location>


	sudo mkdir /etc/apache2/auth
	sudo htpasswd -cm /etc/apache2/auth/svn USERNAME
	Passwort doppelt eingeben

	sudo apachectl graceful

	$ sudo mkdir /usr/local/svn
	$ sudo mkdir /usr/local/REPOSNAME
	$ sudo svnadmin create /usr/local/svn/REPOSNAME
	$ sudo chown -R www:www /usr/local/svn/REPOSNAME


Ich habe Rechtsklick auf repo -> Informationen Schloss aufgemacht
dem User `_www` Rechte auch auf Unterobjekte gegebenen


Bei Eclipse

* Projekt anlegen > `Rechtsklick > Share Project`
* SVN auswählen > `Next > Create a new repository location > http://localhost/REPO`

#### Lokales SVN Repository aufsetzen


##### Modify Apache’s httpd.confOn OS X #

The Apache configuration file httpd.conf is located in `/etc/apache2/httpd.conf `. This file is only writeable for the root user. Therefore you have to edit it using 

	sudo vi /etc/apache2/httpd.conf 
	
Add the following line in the section containing the `LoadModule` directives

	LoadModule dav_svn_module libexec/apache2/mod_dav_svn.so 

Following the conventions of the OS X Apache installation you should add the following lines close to the end of the `httpd.conf` file:

	Include /private/etc/apache2/extra/httpd-svn.conf

That tells Apache to read the subversion configuration from the file located at `/private/etc/apache2/extra/httpd-svn.conf`

Save the modified file and exit vi. Create `httpd-svn.conf`. Fire up vi creating a new file with
	
	sudo vi /etc/apache2/extra/httpd-svn.conf 

and add the following lines

	<Location /repo> DAV svn SVNPath <PATH_TO_REPO></Location>
 
Now you can access the repository using the URL `http://localhost/repo`

##  Appendix

### Permission denied

Gerade wenn man Eclipse nutzt kann es zu einigen Problemen kommen wenn es um SVN geht. Etwa wenn man Projekte migriert. Dann kann es auch gerne mal dazu kommen, dass svn cleanup nicht mehr funktioniert, da Dateirechte nicht mehr stimmen. Es kann sogar passieren das das Immutable Flag gesetzt wird, was dafür sorgt dass nichtmal root etwas ändern kann.

 	$ cd .svn
	$ chflags -R nouchg .
	$ chmod -R 755 .

kann da Abhilfe schaffen

### No external editor

Q: Why does subversion need an external editor?

	$ svn commit
	 svn: Commit failed (details follow):
	svn: Could not use external editor to fetch log message; consider setting the $SVN_EDITOR environment variable or using the --message (-m) or --file (-F) options
	 svn: None of the environment variables SVN_EDITOR, VISUAL or EDITOR is set, and no 'editor-cmd' run-time configuration option was found

A: It is customary and good practice to include a comment with each commit. This can be done on the command line with the -m message option or using an editor of your choice. To use and external editor you have to:

    * set the SVN_EDITOR shell variable (e.g., on bash export SVN_EDITOR=xemacs or on [t]csh setevn SVN_EDITOR xemacs)
    * set the VISUAL or EDITOR shell variable 

	It is not advised to set disable comments in the client configuration.
	 svn: None of the environment variables SVN_EDITOR, VISUAL or EDITOR is set
	« on: February 11, 2008, 05:34:44 AM »
	
This error can be fixed by using below commands

	$EDITOR=vi
	$export EDITOR