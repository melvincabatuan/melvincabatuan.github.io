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


## Image Segmentation (with EM Learning) in OpenGM

<code data-gist-id="ff45cf981265a32c750b"></code>

## Output:

$ ./imageSegWithEMLearning coins.pgm out.pgm 3 0.1

>![_config.yml]({{ site.baseurl}}/images/out2.png)


## Output:

$ ./imageSegWithEMLearning coins.pgm out.pgm 2 0.1

>![_config.yml]({{ site.baseurl}}/images/out3.png)
