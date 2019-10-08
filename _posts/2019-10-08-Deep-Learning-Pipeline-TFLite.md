
```python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

keras = tf.keras
tf.__version__
```




    '1.14.0'



## 1. Get Dataset


```python
(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_data()
print("x_train shape:", x_train.shape, "y_train shape:", y_train.shape)
print("x_test shape:", x_test.shape, "y_test shape:", y_test.shape)
```

    x_train shape: (60000, 28, 28) y_train shape: (60000,)
    x_test shape: (10000, 28, 28) y_test shape: (10000,)



```python
plt.subplot(1, 4, 1)
plt.imshow(x_train[0])        # first train
plt.subplot(1, 4, 2)
plt.imshow(x_train[59999])    # last train
plt.subplot(1, 4, 3)
plt.imshow(x_test[0])         # first test
plt.subplot(1, 4, 4)
plt.imshow(x_test[9999])      # last test
```




    <matplotlib.image.AxesImage at 0x2db881816d8>




![_config.yml]({{ site.baseurl}}/images/2019108_output_3_1.png)


## 2. Get Dataset


```python
num_classes = 10

x_train = x_train.astype('float32')
x_test = x_test.astype('float32')

# Normalize the input data
x_train = x_train.astype('float32') / 255
x_test = x_test.astype('float32') / 255

# Reshape input data from (28, 28) to (28, 28, 1)
w, h = 28, 28
x_train = x_train.reshape(x_train.shape[0], w, h, 1)
x_test = x_test.reshape(x_test.shape[0], w, h, 1)

# One-hot encode the labels
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

print("x_train shape:", x_train.shape, "y_train shape:", y_train.shape)
print("x_test shape:", x_test.shape, "y_test shape:", y_test.shape)
```

    x_train shape: (60000, 28, 28, 1) y_train shape: (60000, 10)
    x_test shape: (10000, 28, 28, 1) y_test shape: (10000, 10)


## 3. Create model


```python
def create_model():
    model = keras.models.Sequential([

    keras.layers.Conv2D(filters=32, kernel_size=3, padding='same', activation='relu', input_shape=(28,28,1)),
    keras.layers.MaxPooling2D(pool_size=2),
    keras.layers.Dropout(0.3),

    keras.layers.Conv2D(filters=64, kernel_size=3, padding='same', activation='relu'),
    keras.layers.MaxPooling2D(pool_size=2),
    keras.layers.Dropout(0.3),

    keras.layers.Flatten(),
    keras.layers.Dense(128, activation='relu'),
    keras.layers.Dropout(0.5),
    keras.layers.Dense(10, activation='softmax')

    ])

    model.compile(loss=keras.losses.categorical_crossentropy,
         optimizer=keras.optimizers.Adam(),
         metrics=['accuracy'])

    return model
```


```python
model = create_model()
model.summary()
```

    WARNING: Logging before flag parsing goes to stderr.
    W1008 19:20:59.557817  3592 deprecation.py:506] From C:\Program Files\Anaconda3\envs\latest\lib\site-packages\tensorflow\python\ops\init_ops.py:1251: calling VarianceScaling.__init__ (from tensorflow.python.ops.init_ops) with dtype is deprecated and will be removed in a future version.
    Instructions for updating:
    Call initializer instance with the dtype argument instead of passing it to the constructor


    Model: "sequential"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #
    =================================================================
    conv2d (Conv2D)              (None, 28, 28, 32)        320
    _________________________________________________________________
    max_pooling2d (MaxPooling2D) (None, 14, 14, 32)        0
    _________________________________________________________________
    dropout (Dropout)            (None, 14, 14, 32)        0
    _________________________________________________________________
    conv2d_1 (Conv2D)            (None, 14, 14, 64)        18496
    _________________________________________________________________
    max_pooling2d_1 (MaxPooling2 (None, 7, 7, 64)          0
    _________________________________________________________________
    dropout_1 (Dropout)          (None, 7, 7, 64)          0
    _________________________________________________________________
    flatten (Flatten)            (None, 3136)              0
    _________________________________________________________________
    dense (Dense)                (None, 128)               401536
    _________________________________________________________________
    dropout_2 (Dropout)          (None, 128)               0
    _________________________________________________________________
    dense_1 (Dense)              (None, 10)                1290
    =================================================================
    Total params: 421,642
    Trainable params: 421,642
    Non-trainable params: 0
    _________________________________________________________________


## 4. Train Model


```python
%%time
history = model.fit(
        x_train,
        y_train,
        batch_size=64,
        epochs=5,
        validation_data=(x_test, y_test))
```

    Train on 60000 samples, validate on 10000 samples
    Epoch 1/5
    60000/60000 [==============================] - 32s 540us/sample - loss: 0.2854 - acc: 0.9109 - val_loss: 0.0581 - val_acc: 0.9815
    Epoch 2/5
    60000/60000 [==============================] - 32s 537us/sample - loss: 0.1052 - acc: 0.9684 - val_loss: 0.0401 - val_acc: 0.9869
    Epoch 3/5
    60000/60000 [==============================] - 32s 536us/sample - loss: 0.0810 - acc: 0.9756 - val_loss: 0.0347 - val_acc: 0.9886
    Epoch 4/5
    60000/60000 [==============================] - 33s 542us/sample - loss: 0.0695 - acc: 0.9791 - val_loss: 0.0274 - val_acc: 0.9913
    Epoch 5/5
    60000/60000 [==============================] - 32s 533us/sample - loss: 0.0618 - acc: 0.9813 - val_loss: 0.0254 - val_acc: 0.9917
    Wall time: 2min 41s


## 5. Evaluate Model


```python
score = model.evaluate(x_test, y_test, verbose=1)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

    10000/10000 [==============================] - 1s 141us/sample - loss: 0.0254 - acc: 0.9917

    Test score: 0.025416590155386803
    Test accuracy: 0.9917



```python
print(history.history.keys())
plt.plot(history.history['acc'])
plt.plot(history.history['val_acc'])
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```

    dict_keys(['loss', 'acc', 'val_loss', 'val_acc'])



![_config.yml]({{ site.baseurl}}/images/2019108_output_13_1.png)



```python
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/2019108_output_14_0.png)


## 6. Save Model


```python
keras_model = "keras_model.h5"
keras.models.save_model(model, keras_model)
```

## 7. Convert Keras model to TFLite model for Android


```python
from datetime import date

converter = tf.lite.TFLiteConverter.from_keras_model_file(keras_model)
tflite_model = converter.convert()
tflite_model_file_name = "mnist_" + tf.__version__ + "_" + str(date.today()) + ".tflite"
open(tflite_model_file_name, "wb").write(tflite_model)
```

    W1008 19:32:14.385704  3592 deprecation.py:506] From C:\Program Files\Anaconda3\envs\latest\lib\site-packages\tensorflow\python\ops\init_ops.py:97: calling GlorotUniform.__init__ (from tensorflow.python.ops.init_ops) with dtype is deprecated and will be removed in a future version.
    Instructions for updating:
    Call initializer instance with the dtype argument instead of passing it to the constructor
    W1008 19:32:14.387699  3592 deprecation.py:506] From C:\Program Files\Anaconda3\envs\latest\lib\site-packages\tensorflow\python\ops\init_ops.py:97: calling Zeros.__init__ (from tensorflow.python.ops.init_ops) with dtype is deprecated and will be removed in a future version.
    Instructions for updating:
    Call initializer instance with the dtype argument instead of passing it to the constructor
    W1008 19:32:14.908329  3592 deprecation.py:323] From C:\Program Files\Anaconda3\envs\latest\lib\site-packages\tensorflow\lite\python\util.py:238: convert_variables_to_constants (from tensorflow.python.framework.graph_util_impl) is deprecated and will be removed in a future version.
    Instructions for updating:
    Use `tf.compat.v1.graph_util.convert_variables_to_constants`
    W1008 19:32:14.909357  3592 deprecation.py:323] From C:\Program Files\Anaconda3\envs\latest\lib\site-packages\tensorflow\python\framework\graph_util_impl.py:270: extract_sub_graph (from tensorflow.python.framework.graph_util_impl) is deprecated and will be removed in a future version.
    Instructions for updating:
    Use `tf.compat.v1.graph_util.extract_sub_graph`





    1688684




```raw
 -mkc
```
