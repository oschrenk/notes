# Tesseract #

Tesseract is an OCR (Optical Character Recognition) engine.

## Usage ##<>

The easiest way to use it is to convert the source to a Grayscale tiff:
  `convert source.png -type Grayscale terre_input.tif`
then run tesseract:
  `tesseract terre_input.tif output`

## Troubleshooting ##

Tesseract only supports `.tif` files; rename `.tiff` to `.tif`
