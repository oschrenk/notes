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
- Limit lines to 80 characters. 
	> Yes, screens have gotten much bigger over the last few years, but your brain hasn't. Use the additional room for split screen, your editor supports that, right?
	[#Geissendörfer:2011]
- api > framework
- dependency injection > dependency injection container

### Logging ###

- first class citizen
- always use [iso 8601](http://en.wikipedia.org/wiki/ISO_8601) for date formatting
- *Be professional*. Write in professional english.
- *Be concise*.
- *Be consistent*. Think about the message. Think about it hard. The message should never be changed in the future. It helps if you want to search for it in the future. 

[#Batchelder:2003]

### Comments ###

1. Primary Rule

> Comments are for things that **cannot** be expressed in code.
 
The code must be readable. It must clearly express its the developers intents. A good example is a comment to the paper that introduced a specific but complex algorithm.

2. Single Truth Rule

> Comments which restate code must be deleted.

Any restatement of the code is unlikely to be maintained over time. If the comment is maintained, it just adds to the cost, if not maintained they at best waste your time at worst cause confusion and introduce bugs.

3. Single Truth Rule

> If the comment says what the code could say, then the code must change to make the comment redundant.

or example, a comment explaining that the variable `x` represents the principal amount of a loan violates the single truth rule. The variable ought to be named `loanPrincipal`.

[#Ottinger:2009]

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

[#Tower:2011]

#### Commit Messages ####

Git format preferred

- one line summary (max 74 chars)
- then empty line
- then multiple paragraphs explaining the patch in detail (if needed)
- don't describe the code, describe the intent and the approach
- keep the log in a present tense (to be consistent with generated messages from cimmands like `git merge`)

[source](http://who-t.blogspot.com/2009/12/on-commit-messages.html)

Example

	header line: explaining the commit in one line

	Body of commit message is a few lines of text, explaining things
	in more detail, possibly giving some background about the issue
	being fixed, etc etc.

	The body of the commit message can be several paragrahps, and
	please do proper word-wrap and keep columns shorter than about
	74 characters or so. That way "git log" will show things
	nicely even when it's indented.

	Reported-by: whoever-reported-it
	Signed-off-by: Your Name <youremail@yourhost.com>

### Code Review ###

Each code that goes into production should be checked by a peer

- having a **second set of eyes** look over code before it gets checked in catches bugs.
- **good kind of peer pressure** knowing that a coworker, whose opionon matters to you will result in neater, better documented, and better organized code.
- **spreading knowledge** in larger groups each person is responsible for a core component. With code review at least two people are familiar with the code

Keep in mind

>  As a reviewer, your job isn't to make sure that the code is what you would have written - because it won't be. Your job as a reviewer of a piece of code is to make sure that the code as written by its author is correct. When this rule gets broken, you end up with hard feelings and frustration all around - which isn't a good thing.

> The second major pitfall of review is that people feel obligated to say something. You know that the author spent a lot of time and effort working on the code - shouldn't you say something?

> There is never anything wrong with just saying "Yup, looks good". If you constantly go hunting to try to find something to criticize, then all that you accomplish is to wreck your own credibility

[#MarkCC:2011]

## Office ##

### Desks ###

- each to his own but provide room, stationary and helpers (like cord organizers) for keeping the workplace cleam
- have power easy accessible
- have easy accessible ethernet ports available on or near the desk

### Technology ###

- fileserver
- map/reduce cluster
- easy way to spin up virtual machines 

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

## Bibliography ##

[#Geissendörfer:2011]: Felix Geissendörfer [Node.js Style Guide](http://nodeguide.com/style.html#line-length) 2011

[#Batchelder:2003]: Ned Batchelder. [Logging Style Guide](http://nedbatchelder.com/text/log-style-guide.html) 2006

[#Tower:2011]: [Tower](http://www.git-tower.de). [Git Cheat Sheet](http://www.git-tower.com/files/cheatsheet/Git_Cheat_Sheet_grey.pdf) from the guys who made [tower](http://www.git-tower.de) 2011

[#MarkCC:2011]: MarkCC [Things Everyone Should Do: Code Review](http://scientopia.org/blogs/goodmath/2011/07/06/things-everyone-should-do-code-review/) from [scientopia.org/](http://scientopia.org/) 2011

[#Ottinger:2009]: Tim Ottinger, [Rules for Commenting](http://agileinaflash.blogspot.com/2009/04/rules-for-commenting.html) 2009