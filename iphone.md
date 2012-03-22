# iPhone #

## Measurements ##

The hit-region of a user interface element is recommended to be 44×44 pixels

### Screen size ###

Complete screen
:	480×320 px
Status bar
:	20×320 px
Navigation bar
:	44×320 px
Tab bar
:	49×320 px
Toolbar
:	44×320 px
Visible Keyboard
:	215×320 px

### Icons ###

Home screen icon
:	57×57 px
Spotlight icon
:	29×29 px
Toolbar icon
:	20×20 px
Tab bar icon
:	30×30 px

### Real size ###

On [developer.apple.com/ipod/cases.html](http://developer.apple.com/ipod/cases.html) Apple has released dimensional drawings of all mobile devices.

iPhone 3G case

width
:	62.1 mm
length
:	115.5 mm
height
:	12.3 mm

iPhone 3G screen size

width
:	51.42 mm
length
:	76.38 mm

That means

1 mm
:	6.28436764 px ≈ 6 px
1 cm
:	62.8436764 px ≈ 62 px
1 px
:	0.159125 mm ≈ 0,16 mm
10 px
:	1,59125 mm ≈ 1,6 mm
100 px
:	15,9125 mm ≈ 16 mm

calculated using length as the dimension drawing . If you use it to calculate the width it doesn't fit. The iPod drawings suggest that the active area has an edge of 0.75mm. Something that is missing from the iPhone dimension drawings. Let's consider that info.

iPhone 3G active screen size:

width
:	49.92mm
length
:	74.88mm

which converts to

1 mm
:	6.410256 px ≈ 6 px
1 cm
:	64.10256 px ≈ 64 px
1 px
:	0.156 mm
10 px
:	1,56 mm
100 px
:	15,6 mm

which if you use it to calculate the screen width results in 49.92 mm,
which is absolutely correct.

## Offline applications ##

Webpage icon
:	`<link rel="apple-touch-icon" href="/custom_icon.png"/>` 57×57 pixels, 90 degree corners, no gloss. iPhone adds rounded corners and gloss
Startup image (OS 3.0+)
:	`<link rel="apple-touch-startup-image" href="/startup.png">` must be 320×460 in portrait orientation
Hide Safari user interface components
:	`<meta name="apple-mobile-web-app-capable" content="yes" />`
Changing the status bar appearance
:	`<meta name="apple-mobile-web-app-status-bar-style" content="black" />` only in connection with `apple-mobile-web-app-capable`, changes appearance of status bar

### Safari layout ###

Status bar
:	20 pixels
text field: 60 pixels
Visible area
:	320 × 356 pixels
Button bar
:	44 pixels

### Viewport ###

Default width
:	980 pixels


You can define the viewport in the header:

    <meta name="viewport" content="width=device-width"

You can change the scale with

    <meta name = "viewport" content = "initial-scale = 1.0">

Deactivate scaling with

    <meta name = "viewport" content = "initial-scale = 2.3, user-scalable = no">

For web applications specifically designed for iPhone you might want to use

    <meta name = "viewport" content = "user-scalable=no, width=device-width">

### Caching the web application ###

Can be done by using a 5 manifest file. You include a manifest file with

    <html manifest="demo.manifest">

A sample manifest file:

    CACHE MANIFEST
    # This is a comment.
    # Cache manifest version 0.0.1
    # If you change the version number in this comment,
    # the cache manifest is no longer byte-for-byte
    # identical.
     
    `demoimages/clownfish.jpg`
     
    `NETWORK:
    # All URLs that start with the following lines
    # are whitelisted.
    http://example.com/examplepath/
    http://www.example.org/otherexamplepath/`
     
    `CACHE:
    # Additional items to cache after NETWORK.
    demoimages/stonessmall.jpg`
     
    `FALLBACK:
    # if the first URI doesn't work, use the second one
    demoimages/ images/`

    `You have to follow these rules:`

    `* must be served as @text/cache-manifest@, use a @.htaccess@
    * first line must be @CACHE MANIFEST@.`

#### Updating the cache ####

    CACHE MANIFEST
    # v0.0.1
    images/background.jpg

The comment isn't superfluous. On the contrary. It's very useful. The cache is only updated when the manifest file changes and
not if the referenced file changes. You can change the version number to trigger a cache update.

You can attach event listener to the object and swap the cache automatically,

    var webappCache = window.applicationCache;
    function updateCache() {
    	webappCache.swapCache();
    }
    webappCache.addEventListener("updateready", updateCache, false);

    `The status of the cache can be accessed via JavaScript using`

    var webappCacheStatus = window.applicationCache.status;

There are 6 different values

*   0 (`UNCACHED`) : no cache available
*   1 (`IDLE`) : the used cache is up-to-date
*   2 (`CHECKING`) : change in the manifest file, checking for changes
*   3 (`DOWNLOADING`) : changes have been found and are being added to the cache
*   4 (`UPDATEREADY`) : the new cache is ready to be updated and override your current cache
*   5 (`OBSOLETE`) : the cache is no longer valid; it has been removed

You can also attach an error handler

    var webappCache = window.applicationCache;
    function errorCache() {
    	alert("Cache failed to update");
    }
    webappCache.addEventListener("error", errorCache, false);

### Online/Offline ###

You can check the network status by accessing the boolean value of

    navigator.onLine
