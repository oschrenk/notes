# Company #

## Code ##

Code is king. 

- everybody and not only developers have access to source repositories
- everybody can see the code from other developers and projects
- everybody can fork the source code and file pull requests
- code is user driven not project driven (source control system should support that (see [github](http://github.com/) )

### Style ###

- utf8, unix line encoding
- agree on company style
- enforce style
	- with shared IDE/editor preferences
	- with tools like [FindBugs](http://findbugs.sourceforge.net/), [PMD](http://pmd.sourceforge.net/), [JSLint](http://www.jslint.com/), [JSHint](http://www.jshint.com/) and the likes
	- pre-commit hooks
- Limit lines to 80 characters. 
	> Yes, screens have gotten much bigger over the last few years, but your brain hasn't. Use the additional room for split screen, your editor supports that, right?
	[#Geissendörfer:2011]
- api > framework
- dependency injection > dependency injection container

### Logging ###

- first class citizen
- always use [iso 8601](http://en.wikipedia.org/wiki/ISO_8601) for date formatting

- *Be professional*. Write in professional English.
- *Be concise*.
- *Be consistent*. Think about the message. Think about it hard. The message should never be changed in the future. It helps if you want to search for it in the future. 

[#Batchelder:2003]

- no debugging logs in production
- do not log locally
- monitor your logs

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

#### Pre-Commit ####

[Ben Summers, 2012, Pre-Commit Checklist](http://bens.me.uk/2012/pre-commit-checklist)

- **Tests**
	- How is this tested?
	- Can I simplify the tests, and by doing so, simplify the code?
	- Is the code in the test clean and easy to read, suggesting that the new code - has a sensible and unambiguous API?
	- Are all the edge cases tested?
- **Correctness**
	- Have I verified that the code meets the requirements?
	- What edge cases have I forgotten?
	- Have I included only the necessary files and removed redundant files? Are the ignore files up-to-date?
	- Are the comments around the code still accurate?
	- Have I removed all the debugging code?
	- Are there any glaring performance issues?
- **Neatness**
	- Have I followed the applicable style guide?
	- Is everything an actual change to logic, with no “whitespace only” changes?
	- Is the diff clear about what’s changed?
	- Does the code read nicely, explaining the intent in the way it’s written?
	- Could I remove any comments, either because they’re unnecessary or by - 	writing the code in a more literate style?
	- Does the commit cover too many changes in one go, and could it be split up?
	- Does the proposed commit message explain what was changed, and why?
- **Complexity**
	- Is there a simpler way to write this code?
	- Is interdependent functionality contained within one logical module, with a clear boundary?
- **Security**
	- Is any untrusted information used without sanitising or checking for - validity?
	- If information is disclosed or actions are taken, are there checks that the - user is authorised?
	- Is it hard to use this code in a way which is insecure?
	- Is it possible to abuse this code in any way?
	- Could this code be open sourced?

#### Commits ####

Git centric approach:

- **commit related changes** a commit should be a wrapper for related changes. Fixing two different bugs should produce  separate commits. Small commits make it easier for other developers to understand the changes and roll them back if something went wrong
- **seperate formatting changes from code changes** don't change whitespace/formatting with the code
- **commit often** Committing often keeps your commits small and, again, helps you commit only related changes. Moreover, it allows you to share your code more frequently with others. That way it is easier for everyone to integrate changes regularly and avoid having merge conflicts. Having few large commits and sharing them rarely, in contrast, makes it hard to solve conflicts.
- **don't commit half done work** You should only commit code when it's completed. This doesn‘t mean you have to complete a whole, large feature before committing. Quite the contrary: split the feature's implementation into logical chunks and remember to commit early and often. But don't commit just to have something in the repository before leaving the office at the end of the day.
- **test code before you commit** Resist the temptation to commit some- thing that you _think_ is completed. Test it thoroughly to make sure it really is completed and has no side effects (as far as one can tell). While committing half- baked things in your local repository only requires you to forgive yourself, having your code tested is even more important when it comes to pushing/sharing your code with others.
- **version control is not a backup system** When doing version control, you should pay attention to committing semantically
- **use branches** Branches are the perfect tool to help you avoid mixing up different lines of development. You should use branches extensively in your development workflows: for new features, bug fixes, ideas.
- **agree on a workflow** Git lets you pick from a lot of different workflows: long-running branches, topic branches, merge or rebase, git-flow... Which one you choose depends on a couple of factors: your project, your overall development and deployment workflows and (maybe most importantly) on your and your teammates' personal preferences. However you choose to work, just make sure to agree on a common workflow that everyone follows. 

[#Tower:2011]

#### Commit Messages ####

Git format preferred

- one line summary (max 74 chars)
- then empty line
- then multiple paragraphs explaining the patch in detail (if needed)
- don't describe the code, describe the intent and the approach
- write in imperative, as if you were commanding (eg. write fix instead of fixed)
- keep the log in a present tense (to be consistent with generated messages from commands like `git merge`)

- [On Commit Messages](http://who-t.blogspot.com/2009/12/on-commit-messages.html)
- [Writing good commit messages](https://github.com/erlang/otp/wiki/Writing-good-commit-messages)

Example

	header line: explaining the commit in one line

	Body of commit message is a few lines of text, explaining things
	in more detail, possibly giving some background about the issue
	being fixed, etc etc.

	The body of the commit message can be several paragraphs, and
	please do proper word-wrap and keep columns shorter than about
	74 characters or so. That way "git log" will show things
	nicely even when it's indented.

	Reported-by: whoever-reported-it
	Signed-off-by: Your Name <youremail@yourhost.com>

Why write a good commit message? What are the benefits?

- speed up the reviewing process
- help us write good release notes
- help future maintainers explain your intentions about code changes

##### On Past/Present tense / imperative mood of commit messages #####

There are many threads on stackexchange. A very good answer discussing the why and when can be found [here](http://stackoverflow.com/questions/3580013/should-i-use-past-or-present-tense-in-git-commit-messages#8059167)

In summary: Your project should almost always use the past tense.

**Arguments against present tense**

- Most version control scenarios are centralized. Commits are a journal of work being done/finished.
- For non-distributed projects, 99.99% of the time a person will be reading a commit message is for reading history - history is read in the past tense

**Arguments for present tense** / imperative mood

- writing in the present tense tells someone what applying the commit will do, rather than what you did.
- Consistency. That's how it is in many projects (including git itself). Also git tools that generate commits (like git merge or git revert) do it.
- It's usually shorter
- You can name commits more consistently with titles of tickets in your issue/feature tracker (which don't use past tense, although sometimes future)

**My conclusion**

- Use the style of the project if working on OpenSource
- Use **present tense**. I like the idea of describing the intended change of a commit. It fits my way of thinking since I started working with Git. I also think that using DVCS is the way to go.

### Code Review ###

Each code that goes into production should be checked by a peer

- having a **second set of eyes** look over code before it gets checked in catches bugs.
- **good kind of peer pressure** knowing that a coworker, whose opinion matters to you will result in neater, better documented, and better organized code.
- **spreading knowledge** in larger groups each person is responsible for a core component. With code review at least two people are familiar with the code

Keep in mind

>  As a reviewer, your job isn't to make sure that the code is what you would have written - because it won't be. Your job as a reviewer of a piece of code is to make sure that the code as written by its author is correct. When this rule gets broken, you end up with hard feelings and frustration all around - which isn't a good thing.

> The second major pitfall of review is that people feel obligated to say something. You know that the author spent a lot of time and effort working on the code - shouldn't you say something?

> There is never anything wrong with just saying "Yup, looks good". If you constantly go hunting to try to find something to criticize, then all that you accomplish is to wreck your own credibility

[#MarkCC:2011]

## Culture ##

- get people out of the office out into the open
- flat hierarchy
- behave like an open source project
- optimize for happiness (everything follows from that)
- meetings are waste (calculate time/money wasted)
- async communication (important if remote (locality, time)) via email, pull requests, messages, ...
- no set work hours (I work when I'm inspired - software development is creative process)
- **the best motivating factor is trust**. Team unity is of ultimate importance in accomplishing your goals. **Rule cultures are bred of distrust**, and sticks and prods to enforce rules will only further erode trust from your team.

## Policies ##

### Bring Your Own Device ###

[Bring Your Own Device](http://en.wikipedia.org/wiki/Bring_your_own_device) (BYOD) describes the recent trend of employees bringing personally-owned mobile devices to their place of work, and using those devices to access privileged company resources such as email, file servers and databases as well as their personal application and data.

#### Advantages ####

- company saves might on devices
- employees might take better care of their own devices
- employees are able to decide which technologies to use

#### Disadvantages ####

- company data might not be secure
- employee is responsible for repairs

### Mobile Device Management ###

[Mobile Device Management](http://en.wikipedia.org/wiki/Mobile_Device_Management) (MDM) software secures, monitors, manages and supports mobile devices deployed across mobile operators, service providers and enterprises. MDM functionality typically includes over-the-air distribution of applications, data and configuration settings for all types of mobile devices.

## Infrastructure ##

- fileserver
- map/reduce cluster
- easy way to spin up virtual machines 

## Office ##

### Desks ###

- each to his own but provide room, stationary and helpers (like cord organizers) for keeping the workplace clean
- have power easy accessible
- have easy accessible ethernet ports available on or near the desk

## Organization ##

Only use open source software. Exceptions are allowed when it comes to OS (OS X) and specialized software (Photoshop et al). 

- Project Management: 
	- [Redmine](http://www.redmine.org/)
	- [Sprintly](https://sprint.ly/)
- Sourcecode: [Git](http://git-scm.com/), [Gitolite](https://github.com/sitaramc/gitolite)
- Messaging: Email, MicroMessaging: [status.net](http://status.net/)

## Projects ##

- everything should be in version control
- documents aren't code and should live in their own source code repository 
- if they should be a direct dependency create a super project and add project as submodules
- continuous integration
- make status visible (e.g.. status monitoring, whiteboard). make it a game.
- automate everything
- configuration should be under version control
- configuration files should be textual with ability to comment (comments communicate intend, which is not possible with GUI control)

### Versioning ###

Use [Semantic Versioning](http://semver.org/)

### Managment ###

- Project Managment software and version control system should be tightly integrated. 
	- `Refs #54` Add commit message as comment to the issue (with commit id)
	- `Fixes #2752` Add commit message as comment to the issue (with commit id) and set status to `fixed`
	- `Closes #42` Add commit message as comment to the issue (with commit id) and set status to `closed`
	- can be realized via hooks
- Projects are driven by people. Use people centric software e.g. Github which uses people and not projects as first class citizen in their urls.

## People ##

### Individuality ###

- software developer tend to be introverts

### Happiness ###

- everybody gets their choice of laptop, mouse, keyboard, monitor, OS (with strong bias towards Unix based OS though)
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

### Teams ###

- put teams in same space to support direct communication

### Community ###

- have hackathons with the local developers
- organize talks and presentations

### Customer ###

- send customer emails based on certain rules
	- "Automatically email users after 7 days if they haven't added a teammate or created a project"
	- "On day 15 tell all iPhone users about the iPhone app, if they haven't already installed it"
- send emails based on time (after 1d, 1w, 1m)
- send emails based on activity

## Bibliography ##

[#Geissendörfer:2011]: Felix Geissendörfer [Node.js Style Guide](http://nodeguide.com/style.html#line-length) 2011

[#Batchelder:2003]: Ned Batchelder. [Logging Style Guide](http://nedbatchelder.com/text/log-style-guide.html) 2006

[#Tower:2011]: [Tower](http://www.git-tower.de). [Git Cheat Sheet](http://www.git-tower.com/files/cheatsheet/Git_Cheat_Sheet_grey.pdf) from the guys who made [tower](http://www.git-tower.de) 2011

[#MarkCC:2011]: MarkCC [Things Everyone Should Do: Code Review](http://scientopia.org/blogs/goodmath/2011/07/06/things-everyone-should-do-code-review/) from [scientopia.org/](http://scientopia.org/) 2011

[#Ottinger:2009]: Tim Ottinger, [Rules for Commenting](http://agileinaflash.blogspot.com/2009/04/rules-for-commenting.html) 2009