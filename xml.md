# XML

## Pretty

```
# brew install xmlstarlet
xmlstarlet format --indent-spaces 2
```

## Useful XPATH expressions

*Selecting distinct/unique attributes*

	//path/to/item[not(@attribute=preceding-sibling::*/@attribute)]/@attribute
