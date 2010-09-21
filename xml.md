# XML

## Useful XPATH expressions

*Selecting distinct/unique attributes*

	//path/to/item[not(@attribute=preceding-sibling::*/@attribute)]/@attribute