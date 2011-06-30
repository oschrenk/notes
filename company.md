# Company #

## Code ##

Code is king. 

- everybody and not only developers have access to source repositories
- everybody can watch other developers and projects
- everybody can fork the source code and file pull requests

### Style ###

- utf8, unix line encoding
- agree on company style
- enforce style, via IDE, via commit hooks

- api > framework
- dependency injection > dependency injection container

### Logging ###

- first class citizen
- always use [iso 8601](http://en.wikipedia.org/wiki/ISO_8601) for date formatting
- *Be professional*. Write in professional english.
- *Concise*.
- *Be consistent*. Think about the message. Think about it hard. The message should never be changed in the future. It helps if you want to search for it in the future. 

[Ned Batchelder, Logging Style Guide](http://nedbatchelder.com/text/log-style-guide.html)

### Commits ###

- SCM is not a backup system: NO end-of-day commits!
- Commit changes and not single files!
- One change per commit!
- Don't change whitespace/formatting with the code!

### Commit Messages ###

- git format preferred
- one line summary (50 to max 78 chars)
- then empty line
- then multiple paragraphs explaining the patch in detail (if needed)
- don't describe the code, describe the intent and the approach. And keep the log in a present tense.

[source](http://who-t.blogspot.com/2009/12/on-commit-messages.html)

## Office ##

### Desks ###

- have power outlets on top of the desks
- have easy accessible ethernet ports available on or near the desk

### Technology ###

- fileserver
- map/reduce cluster

## Organization ##

Only use open source software. Exceptions are allowed when it comes to OS (OS X) and specialized software (Photoshop et al). 

- Project Management: [Redmine](http://www.redmine.org/)
- Sourcecode: [Git](http://git-scm.com/), [Gitolite](https://github.com/sitaramc/gitolite)
- Messaging: Email, MicroMessaging: [status.net](http://status.net/)

## Projects ##

- everything should be in version control
- documents aren't code and should live in their own source code repository 
- if they should be a direct dependency create a super project and add project as submodules
- continuos integration
- make status visible (e.g.. status monitoring, whiteboard). make it a game.
- automate everything
- configuration should be under version control
- configuration files should be textual with ability to comment (comments communicate intend, which is not possible with GUI control)

### Versioning ###

Use [Semantic Versioning](http://semver.org/)

## Teams ##

- put teams in same space to support direct communication

## People ##

- everybody gets their own laptop, mouse, keyboard, monitor, os (only unix based though)
- create open space
- offer books, place to read
- release parties
- release cookies

### New Hires ###

- check what hardware he wants/needs before he starts 
- assign a mentor for the first day 
- mentor should be project lead/member of the project the new hire should work in
- email/scm/... should be setup when he sits down
- give him something to do that can go live today
- whole company or a least project team has meal together on first day with new hire
- feedback chat after 1d, 1w, 1m

## Community ##

- have hackathons with the local developers
- organize talks and presentations
