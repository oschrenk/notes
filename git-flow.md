# Git Flow #

Vincent Driessen published his ideas of a [branching model](http://nvie.com/posts/a-successful-git-branching-model/) on his blog.

## Idea ##

One central repo with two main branches with _infinite_ lifetime:
- `orgin/master`, where the `HEAD`  always reflects _production-ready_ state.
- `origin/develop`, where the `HEAD` always reflects the latest development state.

Therefore a merge from `develop` to `master` is a release.

Support branches with _finite_ lifetime:
- feature
- release
- hotfix

### Feature branch ###

- May branch off from: `develop`
- Must merge back into: `develop`
- Branch naming convention: anything except `master`, `develop`, `release-*`, or `hotfix-*`

Used to develop new features in an upcoming release. They exist as long as the feature is in development. Normally exists only in developer repository and not in `origin`.

To start a feature

	git checkout -b myfeature develop

To finish a feature

	git checkout develop
	git merge --no-ff myfeature
	git branch -d myfeature
	git push origin develop

### Release branch ###

- May branch off from: `develop`
- Must merge back into: `develop` and `master`
- Branch naming convention: `release-*`

Used for preparation of a release. Minuscule changes and change metadata (e.g.. version number). Create a release when `develop` is ready for new release. Only now version number changes, what version number must be decided now.

To start a release

	git checkout -b release-1.2 develop
	./bump-version.sh 1.2
	git commit -a -m "Bumped version number to 1.2"

To finish a release

	git checkout master
	git merge --no-ff release-1.2
	git tag -a 1.2
	git checkout develop
	git merge --no-ff release-1.2
	git branch -d release-1.2

### Hotfix branch ###

- May branch off from: `master`
- Must merge back into: `develop` and `master`
- Branch naming convention: `hotfix-*`

Used for critical fixes. The idea is that the work of team members (on `develop`) can continue, while a fix is prepared.

To start a hotfix

	git checkout -b hotfix-1.2.1 master
	./bump-version.sh 1.2.1
	git commit -a -m "Bumped version number to 1.2.1"
	# Fix the bug
	git commit -m "Fixed severe production problem"

To finish a hotfix

	git checkout master
	git merge --no-ff hotfix-1.2.1
	git tag -a 1.2.1
	# include fix in develop
	git checkout develop
	git merge --no-ff hotfix-1.2.1
	git branch -d hotfix-1.2.1

Exception to the rule here is that, when a release branch currently exists, the hotfix changes need to be merged into that release branch, instead of develop

## Commandline ##

Vincent also developed a [git extension](https://github.com/nvie/gitflow) to help the process.

Initialize git repo

	git flow init

Start/Finish feature

	git flow feature start <name>
	# hack hack hack
	git flow feature finish <name>

Start/Finish release

	git flow release start <name>
	# hack bump
	git flow release finish <name>
