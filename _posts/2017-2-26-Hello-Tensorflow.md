## Running the 'Hello World!' routine to check environment settings: 


```python
import tensorflow as tf
hello = tf.constant('Hello World!')
sess = tf.Session()
print(sess.run(hello))
```

    b'Hello World!'


Note: The 'b' prefix is to indicate byte strings instead of unicode.
