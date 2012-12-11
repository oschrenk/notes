# Version Control #

## Merging and Rebasing ##

**Merging** brings two lines of development together while preserving the ancestry of each commit history.

**Rebasing** unifies the lines of development by re-writing changes from the source branch so that they appear as children of the destination branch – effectively pretending that those commits were written on top of the destination branch all along.

Merging keeps the separate lines of development explicitly, while rebasing always ends up with a single linear path of development for both branches. But this rebase requires the commits on the source branch to be re-written, which changes their content and their SHAs. This has important ramifications which we’ll talk about below.

## Workflows ##

### Feature branches ###

You deliberately create a separate branch for a feature you’re developing, and for the sake of this example, you are the only person working on that feature branch. This feature branch may take a while to complete, and you’ll only want to re-integrate it into other lines of development once you’re done.

The final merge: When building a feature on a separate branch, you’re usually going to want to keep these commits together in order to illustrate that they are part of a cohesive line of development. Retaining this context allows you to identify the feature development easily, and potentially use it as a unit later, such as merging it again into a different branch, submitting it as a pull request to a different repository, and so on. Therefore, you’re going to want to merge rather than rebase when you complete your final re-integration, since merging gives you a single defined integration point for that feature branch and allows easy identification of the commits that it comprised.

Keeping the feature branch up to date: While you’re developing your feature branch, you may want to periodically keep it in sync with the branch which it will eventually be merged back into. For example, you may want to test that your new feature remains compatible with the evolving codebase well before you perform that final merge. 		