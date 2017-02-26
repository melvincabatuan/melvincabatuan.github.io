## Tensors are multilinear maps from vector spaces to the real numbers.

E.g.

Scalar:  $\left( f : \mathbb{R}\rightarrow \mathbb{R},  f\left(e\_1\right) = c \right)$  

Vector:  $\left( f : \mathbb{R}^n \rightarrow \mathbb{R},  f\left(e\_i\right) = v\_i \right)$  

Matrix:  $\left( f : \mathbb{R}^n \times \mathbb{R}^m \rightarrow \mathbb{R},  f\left(e\_i, e\_j\right) = A\_{ij} \right)$


## Importing Tensorflow for the demo


```python
import tensorflow as tf 
```

## Running an Interactive session 


```python
session = tf.InteractiveSession()
```

## Tensors in Tensorflow

### A. Scalar $\left( f : \mathbb{R}\rightarrow \mathbb{R},  f(e_1) = c \right)$  


```python
a = tf.constant(3)
b = tf.constant(4)
c = a + b
c.eval()
```




    7



### B. Vector $\left( f :\: \mathbb{R}^n \rightarrow \mathbb{R}, \: f(e\_i) = v\_i \right)$ 


```python
v1 = tf.constant([[1., 2., 3.]]) 
v2 = tf.constant([[3., 4., 5.]])
v3 = tf.add(v1, v2)
v3.eval()
```




    array([[ 4.,  6.,  8.]], dtype=float32)



### C. Matrix  $\left( f :\: \mathbb{R}^n \times \mathbb{R}^m \rightarrow \mathbb{R}, \: f(e\_i, e\_j) = A\_{ij} \right)$


```python
A = tf.constant([[1.,2.,3.],[4.,5.,6.],[7.,8.,9.]])
B = tf.ones([3,3])
C = tf.fill([3,3], 10.)
D = tf.add(A,B)
E = tf.add(D,C)
E.eval()
```




    array([[ 12.,  13.,  14.],
           [ 15.,  16.,  17.],
           [ 18.,  19.,  20.]], dtype=float32)




```python
%pwd
```




    '/home/cobalt'


