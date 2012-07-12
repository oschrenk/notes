# YAML #

**YAML** ( /ˈjæməl/, rhymes with camel) is a human-readable data serialization format. YAML is a recursive acronym for "YAML Ain't Markup Language".

## Concepts ##

- **Structure is shown through indentation** (one or more spaces).
- **Sequence items are denoted by a dash**
- **key value pairs** within a map are separated by a colon
- **stream optimized**. Multiple documents can exist in a single file/stream and are separated by `---`. An optional `...` can be used at the end of a file (useful for signaling an end in streamed communications without closing the pipe).

## Syntax ##

- YAML streams are encoded using the set of printable Unicode characters, either in `UTF-8` or `UTF-16`.
- Whitespace indentation is used to denote structure; however `tab` characters are never allowed as indentation.
- Comments begin with the number sign `#`, can start anywhere on a line, and continue until the end of the line.
- List members are denoted by a leading hyphen `-` with one member per line, or enclosed in square brackets `[ ]` and separated by comma space `, `.
- Associative arrays are represented using the colon space `:  ` in the form key: value, either one per line or enclosed in curly braces `{   }` and separated by comma space `, `.
- An associative array key may be prefixed with a question mark `?` to allow for liberal multi-word keys to be represented unambiguously.
- Strings (scalars) are ordinarily unquoted, but may be enclosed in double-quotes `"`, or single-quotes `'`.
- Within double-quotes, special characters may be represented with C-style escape sequences starting with a backslash `\`. According to the documentation the only octal escape supported is `\0`.
- Block scalars are delimited with indentation with optional modifiers to - Multiple documents within a single stream are separated by three hyphens `---`.
- three periods `... `` optionally end a file within a stream.
- Repeated nodes are initially denoted by an ampersand `&` and thereafter referenced with an asterisk `*`.
- Nodes may be labeled with a type or tag using the exclamation point `!` followed by a string which can be expanded into a URI.
- YAML documents in a stream may be preceded by directives composed of a percent sign `%` followed by a name and space delimited parameters. Two directives are defined in YAML 1.1:
	- The `%YAML` directive is used to identify the version of YAML in a given document.
	- The `%TAG` directive is used as a shortcut for URI prefixes. These shortcuts may then be used in node type tags.
- YAML requires that colons and commas used as list separators be followed by a space so that scalar values containing embedded punctuation (such as 5,280 or http://www.wikipedia.org) can generally be represented without needing to be enclosed in quotes.

Two additional sigil characters are reserved in YAML for possible future standardisation: the at sign `@` and accent grave `.

## Example ##
	
	--- !clarkevans.com/^invoice
	invoice: 34843
	date   : 2001-01-23
	bill-to: &id001
	    given  : Chris
	    family : Dumars
	    address:
	        lines: |
	            458 Walkman Dr.
	            Suite #292
	        city    : Royal Oak
	        state   : MI
	        postal  : 48046
	ship-to: *id001
	product:
	    - sku         : BL394D
	      quantity    : 4
	      description : Basketball
	      price       : 450.00
	    - sku         : BL4438H
	      quantity    : 1
	      description : Super Hoop
	      price       : 2392.00
	tax  : 251.42
	total: 4443.52
	comments: >
	    Late afternoon is best.
	    Backup contact is Nancy
	    Billsmer @ 338-4338.

## Resourcen ##

- [YAML 1.2 Specification](http://www.yaml.org/spec/1.2/spec.html)
- [Wikepedia, YAML](http://en.wikipedia.org/wiki/Yaml#Syntax)

### Parsers ###

- [js-yaml](https://github.com/nodeca/js-yaml) node.js parser by Nodeca.
- [js-yaml](https://github.com/visionmedia/js-yaml) node.js parser by Visionmedia.
