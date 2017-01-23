# cURL #

A good resource for testing HTTP with CURL can be found [here](http://curl.haxx.se/docs/httpscripting.html)

## GET headers ##

```
# -s - Avoid showing progress bar
# -D - - Dump headers to a file, but - sends it to stdout
# -o /dev/null - Ignore response body
curl -sD - -o /dev/null http://example.com
```

## POST a file ##

Given this form

    <form method="POST" enctype='multipart/form-data' action="upload.cgi">
    	<input type=file name=upload>
    	<input type=submit name=press value="OK">
    </form>

you can POST it with

    curl -F upload=@localfilename -F press=OK [URL]$1
