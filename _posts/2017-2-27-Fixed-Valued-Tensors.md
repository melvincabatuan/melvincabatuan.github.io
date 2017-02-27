
```python
import tensorflow as tf
session = tf.InteractiveSession()
```

### A. Constant


```python
A = tf.constant([[1,2,3],[3,4,5],[6,7,8]])
A.eval()
```




    array([[1, 2, 3],
           [3, 4, 5],
           [6, 7, 8]], dtype=int32)



### B. Fill


```python
B = tf.fill([3,3], 10)
B.eval()
```




    array([[10, 10, 10],
           [10, 10, 10],
           [10, 10, 10]], dtype=int32)



### C. Zeros


```python
C = tf.zeros([3,3])
C.eval()
```




    array([[ 0.,  0.,  0.],
           [ 0.,  0.,  0.],
           [ 0.,  0.,  0.]], dtype=float32)



### D. Ones


```python
D = tf.ones([3,3])
D.eval()
```




    array([[ 1.,  1.,  1.],
           [ 1.,  1.,  1.],
           [ 1.,  1.,  1.]], dtype=float32)



### E. Zeros-like and Ones-like


```python
E = tf.zeros_like(A)
E.eval()
```




    array([[0, 0, 0],
           [0, 0, 0],
           [0, 0, 0]], dtype=int32)




```python
F = tf.ones_like(A)
F.eval()
```




    array([[1, 1, 1],
           [1, 1, 1],
           [1, 1, 1]], dtype=int32)


