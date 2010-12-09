# Git Hooks #

[Manual](http://www.kernel.org/pub/software/scm/git/docs/githooks.html)

Hooks are little scripts you can place in` $GIT_DIR/hooks` directory to trigger action at certain points. When git init is run, a handful of example hooks are copied into the `hooks` directory of the new repository, but by default they are all disabled. To enable a hook, rename it by removing its `.sample` suffix.

It is also a requirement for a given hook to be executable. However - in a freshly initialized repository - the `.sample` files are executable by default.

Most of the hooks fall into one of two categories

1. `pre` hooks will be executed before an action (eg. a commit). It can therefore be used to deny or accept an action. Return values unequal to zero deny an action.
2. `post`hooks will be executed after an action. It can be used for triggering the dispatch of emails or other notifications. The exit codes are ignored.	

| Hook					| Description 	|
| --------------------- | -----------:-	|
| `applypatch-msg`		|  |
| `commit-msg`			|  |
| `post-applypatch`		|  |
| `post-checkout`		|  |
| `post-commit`			|  |
| `post-merge`			|  |
| `post-receive`		|  |
| `post-rewrite`		|  |
| `post-update`			|  |
| `pre-applypatch`		|  |
| `pre-auto-gc`			|  |
| `pre-commit`			|  |
| `pre-rebase`			|  |
| `pre-receive`			|  |
| `prepare-commit-msg`	|  |
| `update`				|  |
