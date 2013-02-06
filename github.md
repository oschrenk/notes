# Github #

## Setup ##

Github needs you to generate a SSH key to communicate with the server. As always the Github team made it easy and posted a [tutorial](http://help.github.com/mac-key-setup/). In short:

    $ ssh-keygen -t rsa -C "mail@host.com"

Choose a passphrase, copy it to the clipboard.

    $ cat ~/.ssh/id_rsa.pub | pbcopy

Open the _SSH Public Keys_ tab of your [account](https://github.com/account) and paste it.

## Github User pages ##

If you publish a repository to github named `username.github.com`, it will automatically be used as your github pages website accessible via [username.github.com](http://username.github.com).

It supports static HTMl content, but also supports [Jekyll](http://github.com/mojombo/jekyll/), a static site generator. For more info you can read up on it on [pages.github.com](http://pages.github.com/)

Be advised that only a push from your local repository does trigger a publish of your homepage, editing the pages via the web interface does not trigger the publishing service.

## Push repository to Github ##

Set up the repository on github using [github.com/repositories/new](http://github.com/repositories/new)

Github tells you what to do next, which is [setting up a local repository](#new-project), making your commits, configure your local repository and push it.

    $ git remote add origin git@github.com:username/reponame.git
    $ git push origin master

## Specify GitHub as a remote repository ##

    $ cd ~/path/to/repos/repo
    $ vi .git/config

You should see something like

    [core]
    	repositoryformatversion = 0
    	filemode = true
    	bare = false
    	logallrefupdates = true
    	ignorecase = true

We want to add github as a remote repository, so we add

    [remote "github"]
    	url = git@github.com:git_username/projectname.git
    	fetch =  +refs/heads/:refs/remotes/origin/
    	push = refs/heads/master:refs/heads/master

The name `github` can be anything, but it should be helpful if intend to use different remote repositories in the future

Above code can also be added by a command

    git remote add github git@github.com:git_username/projectname.git

Do some hacking and commit the changes. To push your changes to github call

    $ git push github master

Replace `github` with the name you used.
