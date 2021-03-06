---
layout: post
title: libdeep xor
---



Here is a simple and portable deep learning C library [libdeep](https://github.com/bashrc/libdeep) by Bob Mottram.

To get a general feel of this library, lets try its xor example 

E.g.

<code data-gist-id="b1223c5a701352e02ea2"></code>

Simply run `make` or 


    `gcc -Wall -ansi -pedantic -o xor xor.c -ldeep -lm -fopenmp`

Run the executable for training the xor network 

    `./xor`

The data are as follows 

<code data-gist-id="33f26e006c585ad1d0bf"></code>

After a few seconds it will finish the training with the following output 

```
Loading data set
Number of training examples: 22
Number of labeled training examples: 22
Number of test examples: 6
Number of Inputs: 64
Training Completed
Test data set performance is 99.9%
```

![_config.yml]({{ site.baseurl}}/images/xorTraining.png)

And it will produce the xor networks both in C and Python:

C xor Network 

<code data-gist-id="f25af0f8ed6eb5eeaf8f"></code>

Python xor Network 

<code data-gist-id="3219e6f4f55fd4e5df02"></code>

Testing the C Network 

```
 ./export_xor zero zero
0.0055552125
 ./export_xor zero one
0.9918711185
 ./export_xor one zero
0.9939632416
 ./export_xor one one
0.0139884353
```
