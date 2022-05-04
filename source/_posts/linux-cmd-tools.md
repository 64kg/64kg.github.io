---
title: Linux command line tools
date: 2021-10-23 20:31:00
updated: 2022-02-15 19:41:00
categories:
- Tech
tags:
- Linux
- Software
---

A note on usage of several linux CLI tools. These tools includes pdf tools, image tools, etc.

<!-- more -->

# PDF tools

## poppler-utils

Install by `apt install poppler-utils`.

Poppler is a PDF rendering library based on Xpdf PDF viewer.
This package contains command line utilities (based on Poppler) for getting information of PDF documents, convert them to other formats, or manipulate them:

- pdfdetach -- lists or extracts embedded files (attachments)
- pdffonts -- font analyzer
- pdfimages -- image extractor
- pdfinfo -- document information
- pdfseparate -- page extraction tool
- pdfsig -- verifies digital signatures
- pdftocairo -- PDF to PNG/JPEG/PDF/PS/EPS/SVG converter using Cairo
- pdftohtml -- PDF to HTML converter
- pdftoppm -- PDF to PPM/PNG/JPEG image converter
- pdftops -- PDF to PostScript (PS) converter
- pdftotext -- text extraction
- pdfunite -- document merging tool

## pdftk

Install by `apt install pdftk`.

Dump meta data:
```bash
pdftk src.pdf dump_data output meta.txt
```

Update meta data:
```bash
pdftk src.pdf update_info meta.txt output dest.pdf
```

## Example: add missing bookmarks

Suppose "src.pdf" (*a editable pdf*) is a book that has table of contents, but has no bookmarks (so that you can do quick jumping).

First, extract the text version of table of contents (say, page 6 to page 12) out as "toc.txt":
```bash
pdftotext -f 6 -l 12 src.pdf toc.txt
```

Second, parse "toc.txt" by hand (which is the most time consuming part) to produce "meta-bookmarks.txt". The format of "meta-bookmarks.txt" is something like:
```
BookmarkBegin
BookmarkTitle: Title One
BookmarkLevel: 1
BookmarkPageNumber: 1
BookmarkBegin
BookmarkTitle: Title Two
BookmarkLevel: 1
BookmarkPageNumber: 2
...
```

Finally insert the above meta data into the pdf, and save as "dest.pdf":
```bash
pdftk src.pdf dump_data output meta.txt
cat meta.txt meta-bookmarks.txt > meta-update.txt
pdftk src.pdf update_info meta-update.txt output dest.pdf
```

# Image tools

## imagemagick

Install by `apt install imagemagick`.

### Reduce image file size

One obvious way is resize the dimension
```bash
# fit into a 800px square, DPI is mantained
convert -resize 800x800 in.png out.png
# halve the size
convert -resize 50%x50% in.png out.png
```

To reduce file size without changing dimension, it depends on the format of image. PNG is not a lossy format, so you have to reduce colors or depth:
```bash
# reduce colors to 254
convert -colors 254 in.png out.png
# reduce depth to 5
convert -depth 5 in.png out.png
```
JPEG has a settable "quality" factor, thus you can convert any image to JPEG before resizing.
```bash
convert -quality 50% in.any out.jpg
```