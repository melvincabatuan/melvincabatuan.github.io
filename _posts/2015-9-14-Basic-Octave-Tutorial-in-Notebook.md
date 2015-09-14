---
layout: post
title: Basic Octave Tutorial in Notebook
---



# IPython magic mode


```python
%load_ext oct2py.ipython
```

    The oct2py.ipython extension is already loaded. To reload it, use:
      %reload_ext oct2py.ipython



```python
%octave ver
```


    ----------------------------------------------------------------------
    GNU Octave Version 3.8.2
    GNU Octave License: GNU General Public License
    Operating System: Linux 3.10.0-229.11.1.el7.x86_64 #1 SMP Thu Aug 6 01:06:18 UTC 2015 x86_64
    ----------------------------------------------------------------------
    no packages installed.



```python
x = %octave [1 2; 3 4];
x
```


    





    array([[ 1.,  2.],
           [ 3.,  4.]])




```python
y = %octave ones(3,3);
y
```


    





    array([[ 1.,  1.,  1.],
           [ 1.,  1.,  1.],
           [ 1.,  1.,  1.]])



## Plotting


```python
%octave available_graphics_toolkits()
```


    ans = 
    {
      [1,1] = fltk
      [1,2] = gnuplot
    }





    [u'fltk', u'gnuplot']



Plot output is automatically captured and displayed, and using the -f flag you may choose its format (currently, png and svg are supported).


```python
%%octave -f svg
t = linspace(-10,10,1000);
plot(t,sin(t))
```


    



![_config.yml]({{ site.baseurl }}/images/output_8_1.svg)



```python
%%octave -f png

p = [12 -2.5 -8 -0.1 8];
x = 0:0.01:1;

polyout(p, 'x')
plot(x, polyval(p, x));
```


    12*x^4 - 2.5*x^3 - 8*x^2 - 0.1*x^1 + 8



![_config.yml]({{ site.baseurl }}/images/output_9_1.png)


The plot size is adjusted using the -s flag:


```python
%%octave -s 500,500

# butterworth filter, order 2, cutoff pi/2 radians
b = [0.292893218813452  0.585786437626905  0.292893218813452];
a = [1  0  0.171572875253810];
freqz(b, a, 32);
```


    



![_config.yml]({{ site.baseurl }}/images/output_11_1.png)



```python
%%octave -s 600,200 -f png

subplot(121);
[x, y] = meshgrid(0:0.1:3);
r = sin(x - 0.5).^2 + cos(y - 0.5).^2;
surf(x, y, r);

subplot(122);
sombrero()
```


    



![_config.yml]({{ site.baseurl }}/images/output_12_1.png)


# Importing Octave in Python


```python
from oct2py import octave
```

Samples taken from Andrew Ng's Machine Learning Octave Tutorials

## Basic Operations


```python
5+6
```




    11




```python
6-2
```




    4




```python
5*8
```




    40




```python
1/2
```




    0




```python
1.0/2
```




    0.5




```python
2^6  # xor operation in Python
```




    4




```python
%octave 2^6
```


    ans =  64





    64.0




```python
%%octave   # make the cell octave

1 == 2   % false
1 ~= 2   % true
1 && 0   % false
1 || 0   % true
```


    ans = 0
    ans =  1
    ans = 0
    ans =  1



```python
%%octave

or(1,0)
and(1,0)
xor(1,0)
```


    ans =  1
    ans = 0
    ans =  1


## Variable assignment


```python
%%octave
a = 3;
b = 'hello';
c = 3 >= 1;
a
b
c
```


    a =  3
    b = hello
    c =  1


### Take note of the cell scope


```python
a
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-63-60b725f10c9c> in <module>()
    ----> 1 a
    

    NameError: name 'a' is not defined


## Formatted Display


```python
%%octave
a = pi;
printf('2 decimals: %0.2f\n',a)
printf('6 decimals: %0.6f\n',a)
disp('format long:')
format long
a
disp('format short:')
format short
a
```


    2 decimals: 3.14
    6 decimals: 3.141593
    format long:
    a =  3.14159265358979
    format short:
    a =  3.1416


## Vectors and Matrices


```python
%%octave
A = [1 2; 3 4; 5 6]
cols = columns(A)
rows = rows(A)
v = [1 2 3]
v = [1; 2; 3]
```


    A =
    
            1        2
            3        4
            5        6
    
    cols =  2
    rows =  3
    v =
    
            1        2        3
    
    v =
    
            1
            2
            3



```python
%%octave

v = [1:0.1:2]
v = 1:6
```


    v =
    
      1.00000  1.10000  1.20000  1.30000  1.40000  1.50000  1.60000  1.70000  1.80000  1.90000  2.00000
    
    v =
    
       1   2   3   4   5   6



```python
%%octave

C = 2*ones(2,3)
w = ones(1,3)
w = zeros(1,3)
```


    C =
    
            2        2        2
            2        2        2
    
    w =
    
       1   1   1
    
    w =
    
            0        0        0



```python
%%octave

w = rand(1,3)   % drawn from uniform distribution
w = randn(1,3)  % drawn from normal distribution (mean = 0; var =1)

```


    w =
    
      0.11624  0.50021  0.42565
    
    w =
    
      -0.60292  1.13782  -0.52628



```python
%%octave -f png

w = -6 + sqrt(10)*(randn(1,10000));    % (mean = 1; var = 2)
hist(w)
```


    



![_config.yml]({{ site.baseurl }}/images/output_37_1.png)



```python
%%octave

I = eye(4)
help eye
```


    I =
    
    Diagonal Matrix
    
            1        0        0        0
            0        1        0        0
            0        0        1        0
            0        0        0        1
    
    'eye' is a built-in function from the file libinterp/corefcn/data.cc
    
     -- Built-in Function: eye (N)
     -- Built-in Function: eye (M, N)
     -- Built-in Function: eye ([M N])
     -- Built-in Function: eye (..., CLASS)
         Return an identity matrix.  If invoked with a single scalar
         argument N, return a square NxN identity matrix.  If supplied two
         scalar arguments (M, N), 'eye' takes them to be the number of rows
         and columns.  If given a vector with two elements, 'eye' uses the
         values of the elements as the number of rows and columns,
         respectively.  For example:
    
              eye (3)
               =>  1  0  0
                   0  1  0
                   0  0  1
    
         The following expressions all produce the same result:
    
              eye (2)
              ==
              eye (2, 2)
              ==
              eye (size ([1, 2; 3, 4])
    
         The optional argument CLASS, allows 'eye' to return an array of the
         specified type, like
    
              val = zeros (n,m, "uint8")
    
         Calling 'eye' with no arguments is equivalent to calling it with an
         argument of 1.  Any negative dimensions are treated as zero.  These
         odd definitions are for compatibility with MATLAB.
    
         See also: speye, ones, zeros.
    
    
    Additional help for built-in functions and operators is
    available in the online version of the manual.  Use the command
    'doc <topic>' to search the manual index.
    
    Help and information about Octave is also available on the WWW
    at http://www.octave.org and via the help@octave.org
    mailing list.



```python

```
