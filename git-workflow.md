# Workflow building blocks

1. Collaboration model
2. Branching model
3. Common Practices
4. Continuous Integration
5. Less friction and more automation

## Collaboration model

**Anarchy**

- Fully decentralized Anarchy
- everybody has local copy, everybody pushes to everybody

**Gatekeeper**

- blessed repository with Gatekeeper
- clone repository to remote fork
- send notification to original owner
- pull request

**Dictator and Lieutenants**

- for big projects
- submodules have their own lieutenant
- Lieutenants guards the king or dictator
- king has network of trusted people

**Centralized**

- shared common repository
- normally supports repo or even branch permissions
- centralized access allows fine grained ACLs
- has benefit to integrate with issue tracking and CI systems

## Branching model

1. Product releases
2. Continuous delivery

### Product releases

- for long term support
- one central repository

- create a branch for a new feature
- create a branch for a bugfix
- create a stable long standing branch
- master is alpha/RC
- pull requests, low friction code review, before merge

### Continuous delivery

- maintaining only one version in production

- master is in production
- master can receive hot-fixes
- staging/develop is the next version
- feature branches are started from staging/develop
- with branch name like: username/ISSUE-KEY-summary

## Common Practices ##

### Pull request

- it's a notification or even discussion about code
- low friction code review

### Single Repository vs. Forks ###

Pros of a single repo

- complete visibility
- no need to maintain developer forks

Pros for a fork

- everyone has their own remote repository
- manage codebase maturity
- keep central repository clean
- good for cross department and 3rd party collaboration
- allows for developer to developer interaction

## Continuous Integration

- there will be an explosion of branches
- as team grows performance of build system will degrade
- building everything is expensive
- only build stable and master
- manually trigger feature branch builds

## Less

- code quality via pre-commit hooks (style checks, ...)
- branch from green builds, checkout hook to run tests

## FAQ ##

- pull requests are not a part of git
- pull requests can happen from branches
- a website usually demands for a continuous delivery model







