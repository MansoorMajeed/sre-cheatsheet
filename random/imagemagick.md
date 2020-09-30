# ImageMagick Stuff


## Convert a PDF to jpg

```
convert -density 600 doc.pdf doc-%02d.jpg
```

Change density to change quality

## Resize an image (resolution)

```
convert image.png -resize 70% image-resized.png
```

## Compress an image

```
convert input.jpg -quality 50 compressed-output.jpg
```

## Resize and compress an image

```
convert test.jpg -resize 30% -quality 20 test_out.jpg
```


## Convert PDF in a directory to image

```
cd directory
for doc in `ls *.pdf`; do filename=`echo $doc | cut -f1 -d.`; convert -density 300 $doc $filename-%02d.jpg;done
```

## Resize and compress images in a directory

```
cd directory
mkdir resized
for i in `ls *.jpg`;do convert $i -resize 30% -quality 20 resized/$i-resized.jpg
```
