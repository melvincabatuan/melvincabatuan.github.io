---
layout: post
title: Image Segmentation
---

> "*The problems of image segmentation and grouping remain great challenges for computer vision. Since the time of the Gestalt movement in psychology, it has been known that perceptual grouping plays a powerful role in human visual perception*."  
                                       Pedro Felzenszwalb
## Input:

>![_config.yml]({{ site.baseurl}}/images/coins.png)


## Simple Image Segmentation with OpenGM

<code data-gist-id="7bf023ec33056d804e09"></code>

## Output:

$ ./imageSegSimple coins.pgm out.pgm 20.0 30 90

>![_config.yml]({{ site.baseurl}}/images/out.png)
