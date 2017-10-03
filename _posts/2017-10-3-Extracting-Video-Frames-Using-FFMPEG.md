
## Step 1: Install FFMPEG

http://www.wikihow.com/Install-FFmpeg-on-Windows

https://www.ffmpeg.org/

## Step 2: Make sure to add ffmpeg to your PATH environment  
(You can check this by typing ffmpeg in cmd or terminal)


```python
!ffmpeg
```

    ffmpeg version N-87306-g6743351 Copyright (c) 2000-2017 the FFmpeg developers
      built with gcc 7.2.0 (GCC)
      configuration: --enable-gpl --enable-version3 --enable-cuda --enable-cuvid --enable-d3d11va --enable-dxva2 --enable-libmfx --enable-nvenc --enable-avisynth --enable-bzlib --enable-fontconfig --enable-frei0r --enable-gnutls --enable-iconv --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libfreetype --enable-libgme --enable-libgsm --enable-libilbc --enable-libmodplug --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenh264 --enable-libopenjpeg --enable-libopus --enable-librtmp --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvo-amrwbenc --enable-libvorbis --enable-libvpx --enable-libwavpack --enable-libwebp --enable-libx264 --enable-libx265 --enable-libxavs --enable-libxvid --enable-libxml2 --enable-libzimg --enable-lzma --enable-zlib
      libavutil      55. 75.100 / 55. 75.100
      libavcodec     57.106.101 / 57.106.101
      libavformat    57. 82.100 / 57. 82.100
      libavdevice    57.  8.101 / 57.  8.101
      libavfilter     6.105.100 /  6.105.100
      libswscale      4.  7.103 /  4.  7.103
      libswresample   2.  8.100 /  2.  8.100
      libpostproc    54.  6.100 / 54.  6.100
    Hyper fast Audio and Video encoder
    usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...
    
    Use -h to get full help or, even better, run 'man ffmpeg'
    

Note: If "command not found" then you need to add ffmpeg to PATH


```python
!where ffmpeg
```

    C:\Users\ECE\Documents\ffmpeg-20170915-6743351-win64-static\bin\ffmpeg.exe
    


```python
!ls C:\Users\ECE\Documents\ffmpeg-20170915-6743351-win64-static\bin
```

    
    (tensorflow-gpu) C:\Users\ECE\workspace\ExtractingVideoFrames>dir C:\Users\ECE\Documents\ffmpeg-20170915-6743351-win64-static\bin  
     Volume in drive C is Windows
     Volume Serial Number is 4E4C-DF71
    
     Directory of C:\Users\ECE\Documents\ffmpeg-20170915-6743351-win64-static\bin
    
    16/09/2017  07:52 PM    <DIR>          .
    16/09/2017  07:52 PM    <DIR>          ..
    15/09/2017  09:55 AM        42,307,072 ffmpeg.exe
    15/09/2017  09:55 AM        42,189,824 ffplay.exe
    15/09/2017  09:55 AM        42,215,424 ffprobe.exe
                   3 File(s)    126,712,320 bytes
                   2 Dir(s)  34,444,881,920 bytes free
    

## Step 3: Convert video into frame images

https://en.wikibooks.org/wiki/FFMPEG_An_Intermediate_Guide/image_sequence


```python
!ls
```

    
    (tensorflow-gpu) C:\Users\ECE\workspace\ExtractingVideoFrames>dir   
     Volume in drive C is Windows
     Volume Serial Number is 4E4C-DF71
    
     Directory of C:\Users\ECE\workspace\ExtractingVideoFrames
    
    03/10/2017  08:01 AM    <DIR>          .
    03/10/2017  08:01 AM    <DIR>          ..
    03/10/2017  07:32 AM    <DIR>          .ipynb_checkpoints
    03/10/2017  07:43 AM         2,989,023 EverybodysFreeToWearSunscreenBazLuhrmann.3gp
    03/10/2017  08:01 AM             5,329 ExtractingVideoFramesUsingFFMPEG.ipynb
                   2 File(s)      2,994,352 bytes
                   3 Dir(s)  34,448,015,360 bytes free
    


```python
!ffmpeg -i EverybodysFreeToWearSunscreenBazLuhrmann.3gp -r 1/8 frame_%03d.jpg
```

    ffmpeg version N-87306-g6743351 Copyright (c) 2000-2017 the FFmpeg developers
      built with gcc 7.2.0 (GCC)
      configuration: --enable-gpl --enable-version3 --enable-cuda --enable-cuvid --enable-d3d11va --enable-dxva2 --enable-libmfx --enable-nvenc --enable-avisynth --enable-bzlib --enable-fontconfig --enable-frei0r --enable-gnutls --enable-iconv --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libfreetype --enable-libgme --enable-libgsm --enable-libilbc --enable-libmodplug --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenh264 --enable-libopenjpeg --enable-libopus --enable-librtmp --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvo-amrwbenc --enable-libvorbis --enable-libvpx --enable-libwavpack --enable-libwebp --enable-libx264 --enable-libx265 --enable-libxavs --enable-libxvid --enable-libxml2 --enable-libzimg --enable-lzma --enable-zlib
      libavutil      55. 75.100 / 55. 75.100
      libavcodec     57.106.101 / 57.106.101
      libavformat    57. 82.100 / 57. 82.100
      libavdevice    57.  8.101 / 57.  8.101
      libavfilter     6.105.100 /  6.105.100
      libswscale      4.  7.103 /  4.  7.103
      libswresample   2.  8.100 /  2.  8.100
      libpostproc    54.  6.100 / 54.  6.100
    Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'EverybodysFreeToWearSunscreenBazLuhrmann.3gp':
      Metadata:
        major_brand     : 3gp6
        minor_version   : 256
        compatible_brands: isom3gp6
        creation_time   : 2014-05-08T00:06:54.000000Z
      Duration: 00:05:09.24, start: 0.000000, bitrate: 77 kb/s
        Stream #0:0(und): Video: mpeg4 (Simple Profile) (mp4v / 0x7634706D), yuv420p, 176x144 [SAR 1:1 DAR 11:9], 52 kb/s, 8.33 fps, 8.33 tbr, 8333 tbn, 8333 tbc (default)
        Metadata:
          creation_time   : 2014-05-08T00:06:54.000000Z
          handler_name    : IsoMedia File Produced by Google, 5-11-2011
        Stream #0:1(eng): Audio: aac (LC) (mp4a / 0x6134706D), 22050 Hz, mono, fltp, 23 kb/s (default)
        Metadata:
          creation_time   : 2014-05-08T00:06:54.000000Z
          handler_name    : IsoMedia File Produced by Google, 5-11-2011
    Stream mapping:
      Stream #0:0 -> #0:0 (mpeg4 (native) -> mjpeg (native))
    Press [q] to stop, [?] for help
    [swscaler @ 0000000000a2f780] deprecated pixel format used, make sure you did set range correctly
    Output #0, image2, to 'frame_%03d.jpg':
      Metadata:
        major_brand     : 3gp6
        minor_version   : 256
        compatible_brands: isom3gp6
        encoder         : Lavf57.82.100
        Stream #0:0(und): Video: mjpeg, yuvj420p(pc), 176x144 [SAR 1:1 DAR 11:9], q=2-31, 200 kb/s, 0.12 fps, 0.12 tbn, 0.12 tbc (default)
        Metadata:
          creation_time   : 2014-05-08T00:06:54.000000Z
          handler_name    : IsoMedia File Produced by Google, 5-11-2011
          encoder         : Lavc57.106.101 mjpeg
        Side data:
          cpb: bitrate max/min/avg: 0/0/200000 buffer size: 0 vbv_delay: -1
    frame=   40 fps=0.0 q=1.6 Lsize=N/A time=00:05:20.00 bitrate=N/A dup=0 drop=2536 speed= 865x    
    video:357kB audio:0kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
    


```python
import glob
from matplotlib import pyplot as plt
from matplotlib import image as mpimg
%matplotlib inline

images = []
for image_path in glob.glob('*.jpg'):
    images.append(mpimg.imread(image_path))

plt.figure(figsize=(20,50))
columns = 3
for i, image in enumerate(images):
    plt.subplot(len(images) / columns + 1, columns, i + 1)
    plt.imshow(image)
```


![_config.yml]({{ site.baseurl}}/images/sunscreen_output_12_0.png)


## Extra: Although the focus of this post is conversion of video to its frames, you can use FFMPEG for more stuff:

* create a video from a set of images
* video scaling and pixel format conversion
* extracting audio from video
* concatenate multiple videos into one file
* and many more

BONUS: 

```
Everybody's Free (To Wear Sunscreen)

by Baz Luhrmann

Ladies and Gentlemen of the class of '99

Wear Sunscreen

If I could offer you only one tip for the future,
Sunscreen would be it
The long term benefits of sunscreen have been proved by scientists
whereas the rest of my advice has no basis more reliable than my own meandering experience...

I will dispense this advice now...

1. Enjoy the power and beauty of your youth
oh nevermind;
you will not understand the power and beauty of your youth until they have faded
But trust me, in 20 years you'll look back at photos of yourself
and recall in a way you can't grasp now how much possibility lay before
you and how fabulous you really looked...

You are not as fat as you imagine

2. Don't worry about the future; or worry, but know that worrying is as
effective as trying to solve an algebra equation by chewing bubblegum
The real troubles in your life are apt to be things that
never crossed your worried mind
the kind that blindside you at 4pm on some idle Tuesday

3. Do one thing everyday that scares you
Sing
Don't be reckless with other people's hearts
don't put up with people who are reckless with yours
Floss

4. Don't waste your time on jealousy; sometimes you're ahead, sometimes you're behind...
the race is long, and in the end, it's only with yourself

5. Remember the compliments you receive, forget the insults
if you succeed in doing this, tell me how

6. Keep your old love letters, throw away your old bank statements

Stretch

7. Don't feel guilty if you don't know what you want to do with your life...
the most interesting people I know didn't know at 22 what they wanted to do with their lives
some of the most interesting 40 year olds I know still don't

8. Get plenty of calcium
Be kind to your knees, you'll miss them when they're gone

Maybe you'll marry, maybe you won't
maybe you'll have children, maybe you won't
maybe you'll divorce at 40, maybe you'll dance the funky chicken on your 75th wedding
anniversary...
9. what ever you do, don't congratulate yourself too much or berate yourself either
your choices are half chance, so are everybody else's

10. Enjoy your body
use it every way you can...
don't be afraid of it, or what other people think of it
it's the greatest instrument you'll ever own

11. Dance...even if you have nowhere to do it but in your own living room

12. Read the directions, even if you don't follow them

13. Do NOT read beauty magazines, they will only make you feel ugly

14. Get to know your parents, you never know when they'll be gone for good

15. Be nice to your siblings
they are the best link to your past
and the people most likely to stick with you in the future

16. Understand that friends come and go, but for the precious few you should hold on
Work hard to bridge the gaps in geography and lifestyle
because the older you get
the more you need the people you knew when you were young

17. Live in New York City once, but leave before it makes you hard
live in Northern California once, but leave before it makes you soft

18. Travel

19. Accept certain inalienable truths
prices will rise
politicians will philander
you too will get old, and when you do you'll fantasize that when you were young
prices were reasonable
politicians were noble
and children respected their elders

20. Respect your elders

21. Don't expect anyone else to support you
Maybe you have a trust fund
maybe you have a wealthy spouse
but you never know when either one might run out

22. Don't mess too much with your hair
or by the time you're 40, it will look 85

23. Be careful whose advice you buy, but
be patient with those who supply it
Advice is a form of nostalgia
dispensing it is a way of fishing the past from the disposal, wiping it off
painting over the ugly parts and recycling it for more than it's worth
But trust me on the sunscreen 
```
- mkc