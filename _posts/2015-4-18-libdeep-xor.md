---
layout: post
title: libdeep xor
---



Caffe image dataset requires `train.txt` and `val.txt` which are text listing all the files and their labels.

E.g.

<code data-gist-id="b72ef6456438fc48f7b8"></code> 

## Extracting the frames of a video clip can correspond to one label. Thus, the following command can be utilized: 

    ./opencv_frame_extraction -fn="/path/to/your/video.mp4" -label="3"

## The command will extract the video frames and provide the text listings with the label provided (default is 0): 

![_config.yml]({{ site.baseurl}}/images/frame_extraction3.png)

## Source code:

<code data-gist-id="e2636d0fdddac14061da"></code>

## Github:

[FrameExtractionSampleforCaffe](https://github.com/melvincabatuan/FrameExtractionSampleforCaffe)
