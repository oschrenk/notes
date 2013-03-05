# Regular Expressions #

- [debuggex](http://www.debuggex.com/) Visual help for debugging regular expressions

## Reference ##

| Patttern | Description |
| :---- | :---- |
| `[abx-z]`		| One character of: a, b, or the range x-z
| `[^abx-z]` 	| One character except: a, b, or the range x-z
| `a|b` 		| a or b
| `a?` 			| Zero or one a's
| `a*` 			| Zero or more a's
| `a+` 			| One or more a's
| `a{4}` 		| Exactly 4 a's
| `a{4,8}` 		| Between (inclusive) 4 and 8 a's
| `a{9,}` 		| 9 or more a's
| `(...)` 		| A capturing group
| `(?:...)` 	| A non-capturing group
| `(?=...)` 	| A lookahead
| `(?!...)` 	| A negative lookahead

| Patttern | Description |
| :---- | :---- |
| `^` 			| Beginning of the string
| `$` 			| End of the string
| `\d` 			| A digit (same as [0-9])
| `\D` 			| A non-digit (same as [^0-9])
| `\w` 			| A word character (same as [_a-zA-Z0-9])
| `\W` 			|  A non-word character (same as [^_a-zA-Z0-9])
| `\s` 			| A whitespace character
| `\S` 			| A non-whitespace character
| `\b` 			| A word boundary
| `\B` 			| A non-word boundary
| `\n` 			| A newline
| `\t` 			| A tab
| `\cY` 		| The control character with the hex code Y
| `\xYY` 		| The character with the hex code YY
| `\uYYYY` 		| The character with the hex code YYYY
| `.` 			| Any character
| `\Y` 			| The Y'th captured group
