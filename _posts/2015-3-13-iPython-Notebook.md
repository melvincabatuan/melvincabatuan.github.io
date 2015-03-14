---
layout: post
title: iPython Notebook
---

> "*Simplicity is the final achievement. After one has played a vast quantity of notes and more notes, it is simplicity that emerges as the crowning reward of art.*"  
> Frédéric Chopin 


-------------------------------------------------------------

# <pre>   <span class="hgviolet"> Introduction </span>

<p style="text-align: center;"><i class="icon-github icon-2x">
[melvincabatuan](https://github.com/melvincabatuan)</i></p>

-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


# <span> Hello World! </span>


    message = 'Hello World!'
    !echo $message

    Hello World!



    print("Hello World!")

    Hello World!


-------------------------------------------------------------
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span class="hgviolet"> Displaying Images </span>


    from IPython.display import Image
    Image(url='https://emptyencore.files.wordpress.com/2011/05/the-witcher-2-19121.jpg')
          




<img src="https://emptyencore.files.wordpress.com/2011/05/the-witcher-2-19121.jpg"/>



-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span>  Displaying Equations </span>

The *compound interest* formula states that:

$$ R = Pe^{rt} $$

where:

- $P$ is the principal (initial investment).
- $r$ is the annual interest rate, as a decimal.
- $t$ is the time in years.
- $e$ is the base of natural logarithms.
- $R$ is the total return after $t$ years (including principal)

-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span> Cross Product Formula </span>


<p><span class="math">\[\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} &amp; \mathbf{j} &amp; \mathbf{k} \\
\frac{\partial X}{\partial u} &amp;  \frac{\partial Y}{\partial u} &amp; 0 \\
\frac{\partial X}{\partial v} &amp;  \frac{\partial Y}{\partial v} &amp; 0
\end{vmatrix}\]</span></p>


-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span> Maxwell's Equations </span>

\begin{align}
\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} & = \frac{4\pi}{c}\vec{\mathbf{j}} \\\\   \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\\\
\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\\\
\nabla \cdot \vec{\mathbf{B}} & = 0 
\end{align}

-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span>  List Files using ls </span>


    ls

    [0m[01;32mAnaconda-2.1.0-Linux-x86.sh[0m*       HelloNotebook.ipynb  NotebookEssentials.ipynb  [01;31mRISE-master.zip[0m          Styling_notebooks.ipynb
    Details                            Introduction         NotebookSetup.ipynb       SlideSample.ipynb
    [01;34mEvolution-of-Swarming-Experiment[0m/  [01;34mnode-v0.12.0[0m/        [01;34mRISE-master[0m/              SlideSample.slides.html


-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span> Import Audio </span>


    from IPython.display import Audio
    Audio(url="http://www.nch.com.au/acm/8k16bitpcm.wav")





                <audio controls="controls" >
                    <source src="http://www.nch.com.au/acm/8k16bitpcm.wav" type="audio/x-wav" />
                    Your browser does not support the audio element.
                </audio>
              



-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span> Adding Video </span>


    from IPython.display import YouTubeVideo
    YouTubeVideo('j9jbdgZidu8')





        <iframe
            width="400"
            height="300"
            src="https://www.youtube.com/embed/j9jbdgZidu8"
            frameborder="0"
            allowfullscreen
        ></iframe>
        



-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span> Embed External Websites </span>


    from IPython.display import IFrame
    IFrame('http://www.dlsu.edu.ph/', width='100%', height=540)





        <iframe
            width="100%"
            height="540"
            src="http://www.dlsu.edu.ph/"
            frameborder="0"
            allowfullscreen
        ></iframe>
        



-------------------------------------------------------------

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# <span> That's it for now! </span>
