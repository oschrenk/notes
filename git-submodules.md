# Git submodules #

Sources:

- [GitSubmoduleTutorial](http://git.wiki.kernel.org/index.php/GitSubmoduleTutorial)
- [Git Submodules: Adding, Using, Removing, Updating](http://chrisjean.com/2009/04/20/git-submodules-adding-using-removing-and-updating/)

## Adding submodules ##

	 git submodule add git@github.com:user/repo.git path

## Pulling submodules ##

If a project uses submodules and a developer clones that repository it looks like a normal checkout. But a closer look shows that the directories exist but are in fact empty.

Use

    $ git submodule init

to add the submodule repository URLs to `.git/config`. Then run

    $ git submodule update

to clone the repositories.

If you want to make changes within the submodule you have to check out a branch

    $ cd subproject
    $ git checkout master

## Making changes in a submodule ##

You can work quite normally in a checked out submodule. Consider the following example:

    $ pwd
    /tmp/git/super/subproject
    $ cd subproject
    $ echo "a" >> file.txt
    $ commit -a -m file.txt
    [master]: created 92c430: "Added A"
    1 files changed, 1 insertions(+), 0 deletions(-)
    [/tmp/git/super/subproject(master)]$ cd ..

We are now again in the root directory of our super project. You can check the status of your submodules via

	$ git submodule status +\92c43001654dca53ba570827d8d3e7d465abbae5 ProjectA (heads/master)

As you can see there is a `+` sign in front of the SHA hash indicating that the HEAD of your submodule has moved away from the version the superproject used to reference to it in its index.

You can check which hash the super project uses via

    $ git ls-files --stage | grep subproject
    160000 cf8c0db9da2aef3d9da40a733c6e641facfd831a 0	subproject

Running `git status` will show the subproject directory as modified

    $ git status | grep subproject
    #	modified:   subproject

## Pushing changes of submodule ##

Always push changes of the submodule before you push the superproject.

    git push

## Updating the superproject ##

After (!) you pushed all submodules you have changed (do not do a `git submodule update` until all submodules have published their changes) you can update your submodule references.

    $ pwd
    /tmp/git/super
    $ git add subproject # DON'T USE THE FORWARD SLASH AT THE END
    $ git commit -m "Updated submodule" subproject
    $ git push

**BEWARE**

! If you use `git add subproject/` instead of `git add subproject` Git will think you want to delete the submodule and want to add all the files in the submodule directory.

## Removing a submodule ##

1. Remove the submodule’s entry in the `.gitmodules` file

There should be an entry like:

	[submodule "lib/billboard"]
    path = lib/billboard
    url = git@mygithost:billboard

2. Remove the submodule’s entry in the `.git/config`

There should be an entry like:

	[submodule "billboard"]
	url = git@mygithost:billboard

3. Remove the path created for the submodule

Simply run

	git rm --cached [plugin path] # without trailing slash

Do **not** put a trailing slash as the command will fail.

## Troubleshooting ##

*   If you make changes within a submodule and then run `git submodule update` the changes you made will be silently overwritten.
*   Always publish the submodule before you publish the change in the superproject. If you do others won't be able to clone your repository.
