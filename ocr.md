# OCR #

## Solutions ##

### Open Source ###

- [GOCR](http://jocr.sourceforge.net/) `brew install gocr`
- [tesseract](http://code.google.com/p/tesseract-ocr/) `brew install tesseract`
- [orcad](http://www.gnu.org/software/ocrad/) `brew install ocrad`

### Closed Source ###

[OmniPage Capture Software Development Kit (SDK)](http://www.nuance.com/for-business/by-product/omnipage/csdk/index.htm)

## Notes ##

300 dpi

    $ convert -colorspace RGB -density 300 name.pdf name.tiff

8 bit

    $ convert -colorspace RGB -depth 8 name.pdf name.tiff

Convert to multiple pages

    convert multipage.tif single%d.tif

## Troubleshooting ##

### `sh: gs: command not found` ###

If you get a message like

    sh: gs: command not found
    convert: Postscript delegate failed `name.pdf': No such file or directory @ pdf.c/ReadPDFImage/634.
    convert: missing an image filename `name.tiff' @ convert.c/ConvertImageCommand/2838.

Ghostscript isn't (properly) installed. You can to so by

    $ brew install ghostscript

## Sources ##

- [Professionally scan with Open Source tools](http://aldeby.org/blog/index.php/how-to-professionally-scan-and-ocr-with-open-source-tools.html)
- [OCR on Linux & Mac](http://gl.ib.ly/archives/35-Doing-OCR-on-linuxMac.html:http://gl.ib.ly/archives/35-Doing-OCR-on-linuxMac.html)
