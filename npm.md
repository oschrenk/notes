# Node Package Manager #

[`npm`](http://github.com/isaacs/npm) is the package manager for [node.js](http://nodejs.org/)

## Installation ##

	curl http://npmjs.org/install.sh | sh

## To _sudo_ or not to _sudo_ ##

Normally you shouldn't use `sudo` for a package manager unless there is a very good reason to - and normally there isn't. [Homebrew](http://mxcl.github.com/homebrew/) (an excellent package manager for OSX) doesn't need it.

Since version 0.3 of though usage of `sudo npm ...` is advised in order to set the correct user id. npm will degrade the user privileges before running build scripts provided by user packages.

Up until now I didn't run into problems without using `sudo` and I don't plan to change my behaviour until I run into problems.

## Usage ##

| Command | Description |
| :---- | :---- |
| `npm install foo` | Installs `foo` package |
| `npm list installed` | List installed packages. |
| `npm ls` | Searches the local system and the remote registry. |
| `npm ls installed` | Shows installed modules. |
| `npm update` | Updates packages. |

## Creating a package ##

Create a file named `package.json` int the root of your package. Here is an example:

	{
	  "name"          : "node-acme",
	  "description"   : "Acme's next great thing.",
	  "homepage"      : "",
	  "keywords"      : ["acme", "roadrunner"],
	  "author"        : "John Doe <john.doe@provider.tld>",
	  "contributors"  : [],
	  "dependencies"  : [],
	  "repository"    : {"type": "git", "url": "git://github.com/acme/project.git"},
	  "main"          : "project.js",
	  "version"       : "0.0.1"
	}

You only need `name`, `version`  and `main` but filling out the other fields will help you and the community in the long run.

For more infos what fields are available use `npm help json`. For instance if you intend to write a command line tool, you should include a `bin` field, so that npm can mark files as executable.

## Publishing ##

1. Create an account with `npm adduser` and supply a username, password and an email address. 
2. Go to the your root directory of your package and run `npm publish`
3. Profit!	
