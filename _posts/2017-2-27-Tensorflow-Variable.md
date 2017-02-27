

```python
import tensorflow as tf
session = tf.InteractiveSession()
```


```python
A = tf.Variable(tf.ones([3,3]), name = "A")
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print(sess.run(A))
```

    [[ 1.  1.  1.]
     [ 1.  1.  1.]
     [ 1.  1.  1.]]



```python
B = tf.Variable(A.initialized_value()*10, name="B")
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print(sess.run(B))
```

    [[ 10.  10.  10.]
     [ 10.  10.  10.]
     [ 10.  10.  10.]]



```python
C = tf.Variable(B.initialized_value() + 2, name="C")
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print(sess.run(C))
```

    [[ 12.  12.  12.]
     [ 12.  12.  12.]
     [ 12.  12.  12.]]



```python
%pwd
```




    '/home/cobalt'




```python

```
