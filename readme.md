ci2v: Compare Image to Video
============================

ci2v searches videos for frames most similar to an image.


## Requirements
* numpy
* opencv
* scikit-image


## How to use it

ci2v just needs an input image and a target video:

~~~text
$ ci2v --image path/to/image --video path/to/video
~~~


Results are formatted by:

* rank
* frame number
* similarity score between 0 and 1


Sample output:

~~~text
--results:
#1	381	: 0.773894658419

--time taken: 
0:00:12.341023
~~~


ci2v can optionally save matched frames as images:

**example:** save the five best-matching frames from a video

~~~text
$ ci2v --image path/to/image --video path/to/video --number 5 --output matched_frame.jpg
~~~



### Options

You can refine a ci2v search with these options:


### `-i` / `--image`
Path to an image file.

Supported formats: `.bmp` `.jpeg` `.jpg` `.jp2` `.png` `.pbm` `.pgm` `.ppm` `.tiff` `.tif`

### `-v` / `--video`
Path to an `ffmpeg`-compatible video file.

### `-n` / `--number`
Number of best matches to return.

### `-b` / `--break_point` `0 to 1`
Break search when similarity level equals or exceeds this level.

### `-o` / `--output` `filename.ext`
Save matched frames as images

Output images are appended with their rank match number, `<filename>_n.<ext>`.

See `--image` for supported formats.

### `-d` / `--directory`
Search recursively through the specified directory for video files and process each one.



## How it works

ci2v is built around **OpenCV** and **scikit-image**, and uses the latter's [*Structural similarity index*](http://scikit-image.org/docs/dev/auto_examples/plot_ssim.html) function to compare an input image to a video frame.

Because `ssim` can be slow with large images, ci2v rescales images to 10x10px greyscale arrays. This means ci2v can search a video at around 200fps and still find acceptable matches.