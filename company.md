# Company #

## Organisation ##

Only use open source software. Exceptions are allowed when it comes to OS (OS X) and specialized software (Photoshop et al). 

- Project Managment: [Redmine](http://www.redmine.org/)
- Sourcecode: [Git](http://git-scm.com/), [Gitolite](https://github.com/sitaramc/gitolite)
- Messaging: Email, MicroMessaging: [status.net](http://status.net/)

## Code ##

Code is king. 

- everybody and not only developers have access to source repositories
- everybody can watch other developers and projects
- everybody can fork the source code and file pull reqests

### Style ###

- utf8, unix line encoding
- agree on company style
- enforce style, via IDE, via commit hooks

- api > framework
- dependency injection > dependency injection container

### Commits ###

- SCM is not a backup system: NO end-of-day commits!
- Commit changes and not single files!
- One change per commit!
- Dont' change Whitespace/Formatting with the code!

### Commit Messages ###

- git format preferred
- one line summary (50 to max 78 chars)
- then empty line
- then multiple paragraphs explaining the patch in detail (if needed)
- don't describe the code, describe the intent and the approach. And keep the log in a present tense.

[source](http://who-t.blogspot.com/2009/12/on-commit-messages.html)

## Projects ##

- everything should be in version control
- documents aren't code and should live in their own source code repository 
- if they should be a direct dependency create a super project and add project as submodules
- continous integration
- make status visible (eg. status monitoring, whiteboard). make it a game.
- automate everything

### Versioning ###

Use [Semantic Versioning](http://semver.org/)

## People ##

- everybody gets their own laptop, mouse, keyboard, monitor, os (only unix based though)
- create open space
- offer books, place to read
- release partys
- release cookies

### New Hires ###

- check what harware he wants/needs before he starts 
- assign a mentor for the first day 
- mentor should be project lead/member of the project the new hire should work in
- email/scm/... should be setup when he sits down
- give him something to do that can go live today
- whole company or a least project team has meal together on first day with new hire
- feedback chat after 1d, 1w, 1m

## Teams ##

- put teams in same space to support direct communication

## Technology ##

- fileserver
- map/reduce cluster

## Communiy ##

- have hackathons with the local developers
- organize talks and presentations