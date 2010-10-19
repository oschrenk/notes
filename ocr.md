# OCR #

## Sources ##

[Professionally scan with Open Source tools](http://aldeby.org/blog/index.php/how-to-professionally-scan-and-ocr-with-open-source-tools.html)
[OCR on Linux & Mac](http://gl.ib.ly/archives/35-Doing-OCR-on-linuxMac.html:http://gl.ib.ly/archives/35-Doing-OCR-on-linuxMac.html)

## Notes ##

300 dpi

    $ convert -colorspace RGB -density 300 name.pdf name.tiff

8 bit

    $ convert -colorspace RGB -depth 8 name.pdf name.tiff

Convert to multiple pages

    convert multipage.tif single%d.tif

## FAQ/Problems ##

### `sh: gs: command not found` ###

If you get a message like

    sh: gs: command not found
    convert: Postscript delegate failed `name.pdf': No such file or directory @ pdf.c/ReadPDFImage/634.
    convert: missing an image filename `name.tiff' @ convert.c/ConvertImageCommand/2838.

Ghostscript isn't (properly) installed. You can to so by

    $ sudo port install ghostscript

### Tesseract ###

Tesseract only supports `.tif` files; rename `.tiff` to `.tif`
