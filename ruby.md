# Ruby #

Install rubies with [rbenv](rbenv.md)

## RubyGems ##

- Install a gem	`gem install name-of-gem`
- Updating your gems `sudo gem update`
- Update RubyGems `sudo gem update --system`

## Troubleshooting ##

### Invalid gemspec in [<path>]: invalid date format in specification ###

	vi <path>

Change `s.date = %q{2011-05-21 00:00:00.000000000Z}` to `s.date = %q{2011-05-21}`
