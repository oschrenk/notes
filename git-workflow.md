# Git Workflows #

There are two popular models of collaborative development on GitHub:

1. The **Fork + Pull Model** lets anyone fork an existing repository and push changes to their personal fork without requiring access be granted to the source repository. The changes must then be pulled into the source repository by the project maintainer. This model reduces the amount of friction for new contributors and is popular with open source projects because it allows people to work independently without upfront coordination.

2. The **Shared Repository Model** is more prevalent with small teams and organizations collaborating on private projects. Everyone is granted push access to a single shared repository and topic branches are used to isolate changes.

Pull requests are especially useful in the Fork + Pull Model because they provide a way to notify project maintainers about changes in your fork. However, they’re also useful in the Shared Repository Model where they’re used to initiate code review and general discussion about a set of changes before being merged into a mainline branch.

## Fork + Pull ##

If you work on open source code, try to follow these rules.

1. Fork the repository
2. Clone to your computer( `$ git clone git@github.com:you/project.git` )
3. Don't forget to `cd` into the repo
4. Set up remote upstream (`$ git remote add upstream git://github.com/<origin>/project.git`)
5. Create a branch for new issue/feature (`$ git checkout -b 100-new-feature`, if you don't have a bug report no worries just skip the number)
6. Develop on your issue/feature branch

[The project developers do their thing and develop on their own]

7. Commit changes to branch. (`$ git add . ; git commit -m 'commit message'`)
8. Fetch upstream (`$ git fetch upstream`)
9. Update local master (`$ git checkout master; git pull upstream master`)
10. Repeat steps 5-8 till development is complete
11. Rebase issue branch (`$ git checkout 100-new-feature; git rebase master`)
12. Push branch to GitHub (`$ git push origin 100-new-feature`)
13. Issue pull request

[#Kramer:2011]

## Rebase Your Development Branch on the Latest Upstream ##

To keep your development branch up to date, rebase your changes on top of the current state of the upstream master. Check out [What's git-rebase?](#gitrebase1) to learn more about rebasing.

If you've set up an upstream branch as detailed above, and a development branch called 100-retweet-bugfix, you'd update upstream, update your local master, and rebase your branch from it like so:

	$ git fetch upstream
	$ git checkout master
	$ git rebase upstream/master
	$ git checkout 100-retweet-bugfix
	[make sure all is committed as necessary in branch]
	$ git rebase master

You may need to resolve conflicts that occur when a file on the development trunk and one of your files have both been changed. Edit each file to resolve the differences, then commit the fixes to your development server repo and test. Each file will need to be "added" before running a "commit."

Conflicts are clearly marked in the code files. Make sure to take time in determining what version of the conflict you want to keep and what you want to discard.

	$ git add <filename>
	$ git commit

To push the updates to your GitHub repo, replace 100-retweet-bugfix with your branch name and run:

	$ git push origin 100-retweet-bugfix

### What's git-rebase? ###

Using `git-rebase` helps create clean commit trees and makes keeping your code up-to-date with the current state of the upstream master easy. Here's how it works.

Let's say you're working on Issue #212 a new plugin in your own branch and you start with something like this:

          1---2---3 #212-my-new-plugin
         /
    A---B #master

You keep coding for a few days and then pull the latest upstream stuff and you end up like this:

          1---2---3 #212-my-new-plugin
         /
    A---B--C--D--E--F #master

So all these new things (C,D,..F) have happened since you started. Normally you would just keep going (let's say you're not finished with the plugin yet) and then deal with a merge later on, which becomes a commit, which get moved upstream and ends up grafted on the tree forever.

A cleaner way to do this is to use rebase to essentially rewrite your commits as if you had started at point F instead of point B. So just do:

	git rebase master 212-my-new-plugin

git will rewrite your commits like this:

                      1---2---3 #212-my-new-plugin
                     /
    A---B--C--D--E--F #master

It's as if you had just started your branch. One immediate advantage you get is that you can test your branch now to see if C, D, E, or F had any impact on your code (you don't need to wait until you're finished with your plugin and merge to find this out). And, since you can keep doing this over and over again as you develop your plugin, at the end your merge will just be a fast-forward (in other words no merge at all).

So when you're ready to send the new plugin upstream, you do one last rebase, test, and then merge (which is really no merge at all) and send out your pull request. Then in most cases, Gina has a simple fast forward on her end (or at worst a very small rebase or merge) and over time that adds up to a simpler tree.

More info on the git man page here:
[Git rebase: man page](http://schacon.github.com/git/git-rebase.html)

[#Kramer:2011]: [Jakob Kramer](https://github.com/gandaro) [The Diaspora Git Workflow](https://github.com/diaspora/diaspora/wiki/Git-Workflow)
