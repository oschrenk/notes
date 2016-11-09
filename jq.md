# JQ

## FAQ

### `jq: error (at <stdin>:2695): Cannot index array with string "..."`

You're trying to select a field on an array, upack the array first. For example: Instead of doing `cat images.json | jq '.images'` , do `cat images.json | jq '.[].images`

@json @shell @jq

