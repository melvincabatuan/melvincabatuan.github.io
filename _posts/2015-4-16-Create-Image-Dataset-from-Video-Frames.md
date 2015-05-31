---
layout: post
title: Create Image Dataset from Video Frames
---




## This task can be accomplished by either of the following methods:

1. ffmpeg
2. opencv (with text file labels)

# ffmpeg

    ffmpeg -i /path/to/your/video.mp4 -r 1/1 $frame%04d.jpg     

E.g.

<code data-gist-id="0e25b678a9bc06735255"></code>

![_config.yml]({{ site.baseurl}}/images/frame_extraction.png)


# opencv


E.g.

<code data-gist-id="7422790021035015eb0c"></code> 
