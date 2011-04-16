# Hub #

[Hub](https://github.com/defunkt/hub) is a command line tool which adds GitHub knowledge to `git`

## Install ##

	brew install hub
	
## Configuration ##

You must ste github user and token in your `.gitconfig`

	git config --global github.user tekkub
	git config --global github.token 0123456789abcdef0123456789abcdef
	
You can find your token on the [account page](https://github.com/account)
	
### Usage ###

	hub init
	hub create
	git add .
	git c
	git push origin master