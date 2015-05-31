---
layout: post
title: Create Image Dataset from Video Frames
---




## This task can be accomplished by either of the following methods:

1. ffmpeg
2. opencv (with text file labels)

# 1. ffmpeg

    ffmpeg -i /path/to/your/video.mp4 -r 1/1 $frame%04d.jpg     

E.g.

<code data-gist-id="0e25b678a9bc06735255"></code>

![_config.yml]({{ site.baseurl}}/images/frame_extraction.png)


# 2. opencv

    ./opencv_frame_extraction -fn="/path/to/your/video.mp4"

E.g.

<code data-gist-id="f80a60ce50c5bd8b1383"></code> 

![_config.yml]({{ site.baseurl}}/images/frame_extraction2.png)

## Source code:

<code data-gist-id="ae74d7c871ec45022bf2"></code>

## Github:

[FrameExtractionSample](https://github.com/melvincabatuan/FrameExtractionDemo2)
