# Ghost #

[Ghost](http://ghost.org) is a blogging platform based on node.js.

## Preparing the Server ##

On a fresh Ubuntu Server 13.10 installation.

Install dependencies

	sudo apt-get update && time sudo apt-get dist-upgrade
	sudo apt-get install git
	sudo apt-get install unzip
	sudo add-apt-repository ppa:chris-lea/node.js
	sudo apt-get update
	sudo apt-get install nodejs

## Using a stable release ##

Install Ghost

	cd ~
	wget https://ghost.org/zip/ghost-0.3.3.zip
	unzip -d ghost ghost-0.3.3.zip
	cd ~/ghost
	npm install

## Building Nightly on development machine ##

Additional requirements

- node 0.10.x
- ruby and the gems `sass` and `bourbon` - you can use bundle install to install the gems
- for running functional tests: `phantomjs 1.9.*`` and `casperjs 1.1.*´
- for building docs: python and pygments

If you're on a Mac

	brew update
	brew install node
	brew install phantomjs
	brew install casperjs --devel

Build the project

	git clone git@github.com:TryGhost/Ghost
	cd Ghost
	git submodule update --init
	npm install -g grunt-cli
	npm install

Create the Bourbon directory and compile SASS and Handlebar templates

	grunt init

Start the server

	npm start

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
