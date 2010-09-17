# <span class="caps">OCR</span> #

## Sources ##

http://aldeby.org/blog/index.php/how-to-professionally-scan-and-ocr-with-open-source-tools.html:http://aldeby.org/blog/index.php/how-to-professionally-scan-and-ocr-with-open-source-tools.html  
http://gl.ib.ly/archives/35-Doing-<span
class="caps">OCR</span>-on-linuxMac.html:http://gl.ib.ly/archives/35-Doing-<span
class="caps">OCR</span>-on-linuxMac.html

## Notes ##

300 dpi

    $ convert -colorspace RGB -density 300 name.pdf name.tiff

8 bit

    $ convert -colorspace RGB -depth 8 name.pdf name.tiff

Convert to multiple pages

    convert multipage.tif single%d.tif

## <span class="caps">FAQ</span>/Problems ##

### `sh: gs: command not found` ###

If you get a message like

    sh: gs: command not found
    convert: Postscript delegate failed `name.pdf': No such file or directory @ pdf.c/ReadPDFImage/634.
    convert: missing an image filename `name.tiff' @ convert.c/ConvertImageCommand/2838.

Ghostscript isn&#8217;t (properly) installed. You can to so by

    $ sudo port install ghostscript

### Tesseract ###

Tesseract only supports `.tif` files; rename `.tiff` to `.tif`
