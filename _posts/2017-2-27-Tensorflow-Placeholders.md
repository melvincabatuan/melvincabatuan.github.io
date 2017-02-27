

```python
import tensorflow as tf
session = tf.InteractiveSession()
```

## Initializing a fixed-element placeholder:


```python
a = tf.placeholder("float32", 4)
b = a * 10
with tf.Session() as sess:
    print(sess.run(b, feed_dict={a:[1., 2., 3., 4.]}))
```

    [ 10.  20.  30.  40.]


## Initializing an n-element placeholder:


```python
a = tf.placeholder("float32", None)
b = a * 10
with tf.Session() as sess:
    print(sess.run(b, feed_dict={a:[1., 2., 3., 4., 5., 6., 7., 8., 9., 10.]}))
```

    [  10.   20.   30.   40.   50.   60.   70.   80.   90.  100.]


## Multi-dimensional placeholder:


```python
a = tf.placeholder("float32", [None, 4])
b = a * 10
with tf.Session() as sess:
    x_data = [[1., 2., 3., 4.],[5., 6., 7., 8.], [9., 10., 11., 12.]]
    print(sess.run(b, feed_dict={a: x_data}))
```

    [[  10.   20.   30.   40.]
     [  50.   60.   70.   80.]
     [  90.  100.  110.  120.]]


## Handle an Image


```python
import matplotlib.image as mpimg
import matplotlib.pyplot as plt

src = mpimg.imread("/mnt/c/Users/cobalt/Pictures/Dovahkin.jpg")
plt.imshow(src)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/2017227_output_8_0.png)


## Crop image:


```python
img = tf.placeholder("uint8", [None, None, 3])
slice = tf.slice(img, [0, 0, 0], [300, -1, -1])

with tf.Session() as session:
        result = session.run(slice, feed_dict={img: src})
        print(result.shape)

plt.imshow(result)
plt.show()
```

    (300, 500, 3)



![_config.yml]({{ site.baseurl}}/images/2017227_output_10_1.png)



```python
%pwd
```




    '/home/cobalt'


