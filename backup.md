# Backup unter OSX #

In den letzten Tagen, oder eher Wochen hat mich die Paranoia gepackt, zumindest was die Sicherheit meiner Daten angeht.

Angefangen hat alles damit, dass sich nach der Reise durch solch eineUnmenge an Daten, Videos und Bilder angesammelt haben, die einfach nicht auf meine Festplatte passen wollten. Also schnell über eine externe Festplatte nachgedacht um mein Datenhort zu vergrößern. Aber wenn ich schon mal auslagern will, warum dann nur meine Bilder, Videos und vielleicht meine Musikbibliothek? Ein ordentlicher Server muss her, um auch endlich mal einen eigenen SVN Server aufsetzen zu können und meine Projekte und Dokumente zu sichern.

Dann trifft es mich plötzlich – was wenn der Server ausfällt, was wenn meine jetzige Festplatte ausfällt? Eine ordentliche Backup-Lösung muss endlich her.

## Was muss gesichert werden ##

Hier differenzieren sich die Anforderungen. Ich empfehle hier jeden mal seine Festplatte zu ordnen und zumindest logisch zu kategorisieren. Mein Datenbestand lässt sich grob in folgende Kategorien aufteilen:

### Email ###

Ich besitze 6 Email-Postfächer die ich mehr oder weniger täglich nutze. Daneben habe ich noch ein paar weitere die aber oft nur als Spam-Adresse missbraucht werden.

Folgende drei nutze ich eigentlich täglich und je nach Kontext. Da hier relativ viele Emails einlaufen habe ich bei allen Accounts IMAP eingerichtet. Dies hat zwei Vorteile: Ich hab nicht nur einen Posteingang, sondern kann direkt auf dem Server Ordnerstrukturen anlegen, die mir eine Sortierung meiner Emails erlaubt. Außerdem liegen alle meine Emails immer online verfügbar und nutzen nicht das veraltete Postfach-Prinzip wie POP &#8211; einmal abgeholt, weg.

*   GMX: Mein Privater Account. Ich habe hier den TopMail Account. Der kostet mich zwar 3€ im Monat gewährt mir aber IMAP Zugriff, 5GB     Speicherplatz und 50 SMS kostenlos.
*   Universität: Mein Account für die Uni.
*   Arbeit: Mein Account für die Arbeit

Für ein privates Projekt verwalte ich noch einige Email Konten, die
aber leider (wegen veralteter Server) POP3 unterstützen:

*   Karmariders Privat
*   Karmariders Team

Dann besitze ich noch natürlich bei allen großen Drei ein Account um
entsprechende Services nutzen zu können:

*   Google Mail für Google Mail, Google Maps etc.
*   Yahoo für Flickr
*   Windows Live

Im Abschluss kann ich nur jedem empfehlen sich seine Email Konto mit IMAP einzurichten. Dadurch verliert man einige Sorgen über verloren gegangene E-Mails. Für den Einsteiger sich hier Google Mail, der hier klar im Wettbewerbsvorteil ist und als einziger IMAP Unterstützung umsonst anbietet.

### Dokumente ###

So gut wie alle meine Daten kommen in diesen Ordner.

*   Korrespondenz: Briefe, Berichte
*   Projekte: Blogs,
*   Privat: Finanzen, Kopien meiner wichtigsten Unterlagen
*   Universität: Alles für meine Uni

#### Development ####

Als Softwareentwickler verdient alles um diesen Bereich einen
gesonderten Bereich in meinem User Verzeichnis.

*   apps: Hier liegen verschiedene Eclipse Installationen (Standard, Aptana, Flex)
*   docs: Dokumentationen für Java, PHP, ...
*   lib: Hauptsächlich Java Bibliotheken (vielleicht sollte ich mir mal Maven einrichten ;)
*   projects: Meine Projekte. Hier liegen immer nur die an denen ich aktuell arbeite. Da alle Projekte zusätzlich in einem lokalen SVN Repository liegen, bewege, lösche und kopiere ich hier ohne große Sorgen. Die Sorgen mache ich mir nur um mein Repository.
*   tools: Android SDK, Maven Installation

#### Medien ####

Unter Medien fasse ich Daten, die der Unterhaltung dienen

*   Filme
*   Musik
*   Fotos

#### Ressourcen ####

Unter Ressourcen fasse ich alle Arten von Dateien, auf die ich nur bei
Bedarf zurückgreifen muss.

*   Grafiken: Da ich früher, heute eher weniger, Ambitionen als Mediengestalter hatte liegen hier eine Vielzahl von verschiedenen Bildern. Sie sind nach Art und Typ sortiert.
*   Installation: Hier sind nur einige Installationspakete für diverse Programme (Mac Developer Tools, Eclipse, iWork), die mir meist einfach zu groß sind um sie herunter zu laden. Neben den Programmen finden sich hier auch Kopien aller Widgets und Erweiterungen (Skripte, AddOns für Firefox), damit ich die wichtigsten Programme (Eclipse, Firefox) auch offline und ohne Zugriff auf das Backup wiederherstellen kann.
*   Schriftarten: Wie gesagt ich hatte Ambitionen als Mediengestalter: Irgendwie kann ich mich nicht von der Sammlung trennen
*   Texte: Meine PDF Sammlung. Von Anleitungen meiner Hardware, über Anleitungen von Brettspielen und Rollenspielen, habe ich hier auch den Verbundsfahrplan und die Bus- und Bahnpläne meiner Nachbarstädte. Gerade letztere ziehe    ich doch häufiger zu Rate.

## Backup Anbieter und Programme ##

Es gibt verschiedene Möglichkeiten um seine Daten zu speichern. Auf einem externen Medium (externe Festplatte, DVD, CD) und Backuplösungen über das Internet. Bei beiden Varianten habe ich natürlich immer die Wahl zwischen manuellen und automatisierten Backup.

Ich kann hier nur klar das automatisierte Backup auf einer Mischung von Online und Offline Medien empfehlen.

### Services ###

#### Dropbox ####

Dropbox hat 2GB mit automatischer inkrementeller Synchronisation und Zugriff auf bis zu 30 Tage alte Dateien

Die Pro Version kostet 50GB 99$ (70€) im Jahr

#### GMX ####

GMX hat 5GB (4GB frei) als WebDav Folder

#### Fruux ####

Fruux syncs Ical, Adressbuch
ziemlich nutzlos da Plaxo das schon hat und zusätzlich ein Webinterface bietet

#### Wuala ####

Wuala hat 1GB aber zusätzliche Applikation (15€ für 10GB/Jahr)
eigene Anwendung

#### Plaxo ####

Plaxo synchronisiert iCal, Adressbuch

#### Mobile Me ####

MobileMe bietet:

*   20GB
*   Backup
*   synchronisiert Mail, Adressen, Ical aber 80€
*   komplett integrierte Lösung

#### Evernote ####

Die Freie Version bietet 40MB monatlichen Upload (für Reine Texte langt das allemal)
Ansonsten 5$ pro Monat bzw 45$ (32€) pro Jahr, für 500MB Upload pro Monat auch für andere Dateien außer Text und
SSL

#### Sugarsync ####

SugarSync 25$ (18€) pro Jahr für 10GB Sync Lösung für IPod und Mac
30GB 50$ (36€) pro Jahr

#### Flickr ####

Flickr bietet für 25$ (18€) pro Jahr unendlich Speicher für Bilder

#### Hosted Server ####

*   kann eigenen SVN Server einrichten
*   kann Tomcat laufen lassen
*   kostet sehr viel Geld

*   Server mit Leuten Teilen
*   billiger

#### Heimserver ####

*   langsame Verbindung zum/weg vom Server
*   gut für Testzwecke
*   aber günstig (Stromkosten)
