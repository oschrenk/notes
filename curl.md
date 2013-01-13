# cURL #

A good resource for testing HTTP with CURL can be found [here](http://curl.haxx.se/docs/httpscripting.html)

## POST a file ##

Given this form

    <form method="POST" enctype='multipart/form-data' action="upload.cgi">
    	<input type=file name=upload>
    	<input type=submit name=press value="OK">
    </form>

you can POST it with

    curl -F upload=@localfilename -F press=OK [URL]$1
