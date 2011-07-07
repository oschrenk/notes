# Company #

## Code ##

Code is king. 

- everybody and not only developers have access to source repositories
- everybody can see the code from other developers and projects
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
- *Be concise*.
- *Be consistent*. Think about the message. Think about it hard. The message should never be changed in the future. It helps if you want to search for it in the future. 

[Ned Batchelder, Logging Style Guide](http://nedbatchelder.com/text/log-style-guide.html)

### Version Control ###

Git centric approach:

- **commit related changes** a commit should be a wrapper for related changes. Fixing two different bugs should produce  seperate commits. Small commits make it easier for other developers to understand the changes and roll them back if something went wrong
- **seperate formatting changes from code changes** don't change whitespace/formatting with the code
- **commit often** Committing often keeps your commits small and, again, helps you commit only related changes. Moreover, it allows you to share your code more frequently with others. That way it‘s easier for everyone to integrate changes regularly and avoid having merge conflicts. Having few large commits and sharing them rarely, in con- trast, makes it hard to solve conflicts.
- **don't commit half done work** You should only commit code when it‘s completed. This doesn‘t mean you have to complete a whole, large feature before committing. Quite the contrary: split the feature‘s implementation into logical chunks and remember to commit early and often. But don‘t commit just to have something in the repository before leaving the office at the end of the day.
- **test code before you commit** Resist the temptation to commit some- thing that you «think» is completed. Test it thoroughly to make sure it really is completed and has no side effects (as far as one can tell). While committing half- baked things in your local repository only requires you to forgive yourself, having your code tested is even more important when it comes to pushing/sharing your code with others.
- **version control is not a backup system** When doing version control, you should pay attention to committing semantically
- **use branches** Branches are the perfect tool to help you avoid mixing up different lines of development. You should use branches extensively in your development workflows: for new features, bug fixes, ideas.
- **agree on a workflow** Git lets you pick from a lot of different workflows: long-running branches, topic branches, merge or rebase, git-flow... Which one you choose depends on a couple of factors: your project, your overall development and deployment workflows and (maybe most important- ly) on your and your teammates‘ personal preferences. However you choose to work, just make sure to agree on a com- mon workflow that everyone follows. 

Taken from the Git Cheat Sheet from the guys who made [tower](http://www.git-tower.de)

### Commit Messages ###

Git format preferred

- one line summary (50 to max 78 chars)
- then empty line
- then multiple paragraphs explaining the patch in detail (if needed)
- don't describe the code, describe the intent and the approach
- keep the log in a present tense (to be consistent with generated messages from cimmands like `git merge`)

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

## Teams ##

- put teams in same space to support direct communication

## Community ##

- have hackathons with the local developers
- organize talks and presentations
