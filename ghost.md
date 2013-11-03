# Ghost #

[Ghost](http://ghost.org) is a blogging platform based on node.js.

## Installation ##

On a fresh Ubuntu Server 13.10 installation.

Install dependencies

	sudo apt-get update && time sudo apt-get dist-upgrade
	sudo apt-get install git
	sudo apt-get install unzip
	sudo add-apt-repository ppa:chris-lea/node.js
	sudo apt-get update
	sudo apt-get install nodejs

Install Ghost

	cd
	wget https://ghost.org/zip/ghost-0.3.3.zip
	unzip -d ghost ghost-0.3.3.zip
	cd ~/ghost
	npm install

## Configuration ##

	cd ~/ghost
	sudo vim config.js

- change every instance of `"host:"` to `0.0.0.0`
- change every instance of `"port:"` to `80`

## Running ##

Start Ghost

	cd ~/ghost
	sudo npm start

As we configured it to run on port `80` we need to prefix our start command with `sudo`. Whenever you terminate the process your blog will cease to exist. So it might be useful to control the node.js process.

Install `forever` to run and monitor the node.js instance that will run our  application.

	sudo npm install forever -g

To start

	cd ~/ghost
	NODE_ENV=production sudo forever start index.js

To restart

	cd ~/ghost
	sudo forever restart index.js

## Themes ##

- [Readium](https://github.com/starburst1977/readium)
