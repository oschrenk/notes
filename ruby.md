# Ruby #

## Installation ##

If you use windows download and run the installer from [http://rubyinstaller.org/](http://rubyinstaller.org/)

On OS X you can just run

	brew install ruby

## RubyGems ##

- Install a gem	`gem install name-of-gem`
- Updating your gems `sudo gem update`
- Update RubyGems `sudo gem update --system`

## FAQ/ Problems ##

### Invalid gemspec in [<path>]: invalid date format in specification ###

	vi <path>
	
Change `s.date = %q{2011-05-21 00:00:00.000000000Z}` to `s.date = %q{2011-05-21}`
