---
layout: post
title: libdeep xor
---



Here is a simple and portable deep learning C library [libdeep](https://github.com/bashrc/libdeep) by Bob Mottram.

To get a general feel of this library, lets try its xor example 

E.g.

<code data-gist-id="b1223c5a701352e02ea2"></code>

Simply run `make` or 

    gcc -Wall -ansi -pedantic -o xor xor.c -ldeep -lm -fopenmp

Run the executable for training the xor network 

    ./xor

Note: The data are as follows 

<code data-gist-id="33f26e006c585ad1d0bf"></code>

After a few seconds it will finish the training with the following output
