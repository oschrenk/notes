git commit -vxp

# enable git-completion.bash

# use git-prompt.sh

# git grep -e stupid --and -e boos

# git stash --include-untracked




# whoops I forgot this
git commit --amend

git show :/stupid

// back to last rbancjh
git co -

git branch --merged
git branch --no-merged

# gives branc
git branch --cotnains <sha> h

# copy file at branch without swithcing branch
git checkout <branch> -- path/to/file


# comits that aren't in B
git log branchA ^branch B


# git status -sb


# good for prose
git diff HEAD^ --word-diff

# remember recorded resolution
git config --global rerere.enabled true

http://git-scm.com/2010/03/08/rerere.html


# I dont quite understand reset(s) I guess
git reset --soft HEAD^


# A script needs to know the root of project
cd $(git rev-parse --show-toplevel)

# So here’s how you add to your repository all unknown files, but respecting the .gitignore and not raising any alarms:
git ls-files --other --exclude-standard | xargs git add



http://durdn.com/blog/2012/11/22/must-have-git-aliases-advanced-examples/
http://durdn.com/blog/2012/12/05/git-12-curated-git-tips-and-workflows/


http://durdn.com/blog/2011/07/06/using-git-with-subversion-tips-that-will-make-your-life-easier/
