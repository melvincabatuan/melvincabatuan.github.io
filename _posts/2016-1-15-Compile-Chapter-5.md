---
layout: post
title: Compile OpenCV3 Blueprints Chapter 5
---

*excerpts from opencv3 blueprints*

>![_config.yml]({{ site.baseurl}}/images/opencv3_blueprints.jpg)

- Clone OpenCV3 Blueprints source:

```
git clone https://github.com/OpenCVBlueprints/OpenCVBlueprints.git
```

- Go to Chapter 5 root directory:

```
cd /path/to/your/clone/OpenCVBlueprints/chapter_5/
``` 

- Navigate the source code:

```
cd source_code/
```

E.x.

>![_config.yml]({{ site.baseurl}}/images/OpenCVBlueprints_ch_5.png)

- Sample build: (object annotation)

```
cd object_annotation/
cmake .
make -j4
```

>![_config.yml]({{ site.baseurl}}/images/OpenCVBlueprints_ch_5_2.png)

Now, you are ready to annotate!
