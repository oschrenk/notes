# C #

## Operators ##

### Memory Access ###

| Operator | Meaning | Example | Description |
| :---- | :---- | :---- | :---- |
| `&`	| Adress of | `&x`| Get the pointer to `x` |
| `*`	| Pointer operator | `*p` | Object to which `p` points to |
| `[]`	| Array Element | `x[i]` | `*(x+i)`: in Array `x` the element with index `i` |
| `.`	| Select component | `s.x` | Component `x` of struct `s` |
| `->`	| Select component | `p->x` | Component `x` of struct, to which `p` points to |

- Use `->` to dereference the pointer on it's left hand side and access the member on it's right hand side.
- Use `.` to access the member of it's right hand side of the variable on it's left hand side.

## Style ##

*header guards*  Header guards are little pieces of code that protect the contents of a header file from being included more than once. They are implemented through the use of preprocessor directives.

	#ifndef UNIQUE_NAME_H
	#define UNIQUE_NAME_H

	// ... code

	#endif

## Testing ##

- [Check](http://check.sourceforge.net/)
