
## A Keras Tutorial on Image Classification With Little Data

Objective: To be able to develop a model for distinguishing cats and dogs

Source: "Building powerful image classification models using very little data" from blog.keras.io.

Step 1: Download data at: https://www.kaggle.com/c/dogs-vs-cats/data

Step 2: Setup overview
- created a data/ folder
- created train/ and validation/ subfolders inside data/
- created cats/ and dogs/ subfolders inside train/ and validation/
- put the cat pictures index 0-999 in data/train/cats
- put the cat pictures index 1000-1400 in data/validation/cats
- put the dogs pictures index 12500-13499 in data/train/dogs
- put the dog pictures index 13500-13900 in data/validation/dogs

Step 3: Organize imports


```python
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D
from keras.layers import Activation, Dropout, Flatten, Dense
from keras import backend as K
import numpy as np

import matplotlib.pyplot as plt
%matplotlib inline
```

    Using TensorFlow backend.
    

Step 4: Set constants and path 


```python
img_width, img_height = 150, 150

train_data_dir = 'data/train'
validation_data_dir = 'data/validation'
nb_train_samples = 2000
nb_validation_samples = 800
epochs = 50
batch_size = 16
```

Step 5: Manage channel ordering


```python
if K.image_data_format() == 'channels_first':
    input_shape = (3, img_width, img_height)
else:
    input_shape = (img_width, img_height, 3)
```

Step 6: Define the model


```python
model = Sequential()
model.add(Conv2D(32, (3, 3), input_shape=input_shape))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Flatten())
model.add(Dense(64))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(1))
model.add(Activation('sigmoid'))
model.summary()
```

    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv2d_4 (Conv2D)            (None, 148, 148, 32)      896       
    _________________________________________________________________
    activation_6 (Activation)    (None, 148, 148, 32)      0         
    _________________________________________________________________
    max_pooling2d_4 (MaxPooling2 (None, 74, 74, 32)        0         
    _________________________________________________________________
    conv2d_5 (Conv2D)            (None, 72, 72, 32)        9248      
    _________________________________________________________________
    activation_7 (Activation)    (None, 72, 72, 32)        0         
    _________________________________________________________________
    max_pooling2d_5 (MaxPooling2 (None, 36, 36, 32)        0         
    _________________________________________________________________
    conv2d_6 (Conv2D)            (None, 34, 34, 64)        18496     
    _________________________________________________________________
    activation_8 (Activation)    (None, 34, 34, 64)        0         
    _________________________________________________________________
    max_pooling2d_6 (MaxPooling2 (None, 17, 17, 64)        0         
    _________________________________________________________________
    flatten_2 (Flatten)          (None, 18496)             0         
    _________________________________________________________________
    dense_3 (Dense)              (None, 64)                1183808   
    _________________________________________________________________
    activation_9 (Activation)    (None, 64)                0         
    _________________________________________________________________
    dropout_2 (Dropout)          (None, 64)                0         
    _________________________________________________________________
    dense_4 (Dense)              (None, 1)                 65        
    _________________________________________________________________
    activation_10 (Activation)   (None, 1)                 0         
    =================================================================
    Total params: 1,212,513
    Trainable params: 1,212,513
    Non-trainable params: 0
    _________________________________________________________________
    

Step 6: Compile the model


```python
model.compile(loss='binary_crossentropy',
              optimizer='rmsprop',
              metrics=['accuracy'])
```

Step 7: Define Image Data Generator for train and test


```python
train_datagen = ImageDataGenerator(
    rescale=1. / 255,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True)

test_datagen = ImageDataGenerator(rescale=1. / 255)

train_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(img_width, img_height),
    batch_size=batch_size,
    class_mode='binary')

validation_generator = test_datagen.flow_from_directory(
    validation_data_dir,
    target_size=(img_width, img_height),
    batch_size=batch_size,
    class_mode='binary')
```

    Found 2000 images belonging to 2 classes.
    Found 802 images belonging to 2 classes.
    

Step 8: Train


```python
history = model.fit_generator(
    train_generator,
    steps_per_epoch=nb_train_samples // batch_size,
    epochs=epochs,
    validation_data=validation_generator,
    validation_steps=nb_validation_samples // batch_size)
```

    Epoch 1/50
    125/125 [==============================] - 13s - loss: 0.3901 - acc: 0.8465 - val_loss: 0.5646 - val_acc: 0.6756
    Epoch 2/50
    125/125 [==============================] - 12s - loss: 0.3951 - acc: 0.8365 - val_loss: 0.5364 - val_acc: 0.7379
    Epoch 3/50
    125/125 [==============================] - 12s - loss: 0.3866 - acc: 0.8380 - val_loss: 0.6592 - val_acc: 0.7621
    Epoch 4/50
    125/125 [==============================] - 12s - loss: 0.3760 - acc: 0.8345 - val_loss: 0.5647 - val_acc: 0.7150
    Epoch 5/50
    125/125 [==============================] - 11s - loss: 0.4309 - acc: 0.8245 - val_loss: 0.5077 - val_acc: 0.7595
    Epoch 6/50
    125/125 [==============================] - 12s - loss: 0.3827 - acc: 0.8435 - val_loss: 0.5747 - val_acc: 0.7913
    Epoch 7/50
    125/125 [==============================] - 11s - loss: 0.4313 - acc: 0.8220 - val_loss: 0.8847 - val_acc: 0.7595
    Epoch 8/50
    125/125 [==============================] - 11s - loss: 0.3869 - acc: 0.8430 - val_loss: 0.4801 - val_acc: 0.7850
    Epoch 9/50
    125/125 [==============================] - 11s - loss: 0.4079 - acc: 0.8200 - val_loss: 0.5802 - val_acc: 0.7417
    Epoch 10/50
    125/125 [==============================] - 11s - loss: 0.4008 - acc: 0.8290 - val_loss: 0.9261 - val_acc: 0.7557
    Epoch 11/50
    125/125 [==============================] - 12s - loss: 0.3884 - acc: 0.8270 - val_loss: 0.7055 - val_acc: 0.7634
    Epoch 12/50
    125/125 [==============================] - 11s - loss: 0.4116 - acc: 0.8365 - val_loss: 0.4997 - val_acc: 0.7799
    Epoch 13/50
    125/125 [==============================] - 11s - loss: 0.3940 - acc: 0.8425 - val_loss: 0.5346 - val_acc: 0.8015
    Epoch 14/50
    125/125 [==============================] - 11s - loss: 0.3919 - acc: 0.8460 - val_loss: 0.5285 - val_acc: 0.8092
    Epoch 15/50
    125/125 [==============================] - 12s - loss: 0.3834 - acc: 0.8415 - val_loss: 0.5388 - val_acc: 0.7964
    Epoch 16/50
    125/125 [==============================] - 12s - loss: 0.3723 - acc: 0.8435 - val_loss: 0.4938 - val_acc: 0.7977
    Epoch 17/50
    125/125 [==============================] - 12s - loss: 0.3743 - acc: 0.8480 - val_loss: 0.6918 - val_acc: 0.7723
    Epoch 18/50
    125/125 [==============================] - 12s - loss: 0.4039 - acc: 0.8345 - val_loss: 0.4744 - val_acc: 0.7913
    Epoch 19/50
    125/125 [==============================] - 12s - loss: 0.3791 - acc: 0.8555 - val_loss: 0.5435 - val_acc: 0.7812
    Epoch 20/50
    125/125 [==============================] - 11s - loss: 0.3856 - acc: 0.8350 - val_loss: 0.5031 - val_acc: 0.7723
    Epoch 21/50
    125/125 [==============================] - 12s - loss: 0.4111 - acc: 0.8390 - val_loss: 0.5261 - val_acc: 0.7405
    Epoch 22/50
    125/125 [==============================] - 11s - loss: 0.3774 - acc: 0.8425 - val_loss: 0.5223 - val_acc: 0.7786
    Epoch 23/50
    125/125 [==============================] - 12s - loss: 0.3913 - acc: 0.8400 - val_loss: 0.5599 - val_acc: 0.8041
    Epoch 24/50
    125/125 [==============================] - 12s - loss: 0.3829 - acc: 0.8440 - val_loss: 0.6514 - val_acc: 0.8168
    Epoch 25/50
    125/125 [==============================] - 11s - loss: 0.3965 - acc: 0.8365 - val_loss: 0.5637 - val_acc: 0.7939
    Epoch 26/50
    125/125 [==============================] - 12s - loss: 0.3928 - acc: 0.8375 - val_loss: 0.5057 - val_acc: 0.7710
    Epoch 27/50
    125/125 [==============================] - 12s - loss: 0.3992 - acc: 0.8340 - val_loss: 0.4931 - val_acc: 0.7990
    Epoch 28/50
    125/125 [==============================] - 12s - loss: 0.4001 - acc: 0.8370 - val_loss: 0.6067 - val_acc: 0.7939
    Epoch 29/50
    125/125 [==============================] - 12s - loss: 0.3811 - acc: 0.8455 - val_loss: 0.5053 - val_acc: 0.8003
    Epoch 30/50
    125/125 [==============================] - 12s - loss: 0.3829 - acc: 0.8405 - val_loss: 0.4840 - val_acc: 0.7799
    Epoch 31/50
    125/125 [==============================] - 12s - loss: 0.3881 - acc: 0.8475 - val_loss: 0.5546 - val_acc: 0.7799
    Epoch 32/50
    125/125 [==============================] - 12s - loss: 0.3767 - acc: 0.8510 - val_loss: 0.5566 - val_acc: 0.7901
    Epoch 33/50
    125/125 [==============================] - 12s - loss: 0.3717 - acc: 0.8425 - val_loss: 0.5731 - val_acc: 0.8066
    Epoch 34/50
    125/125 [==============================] - 12s - loss: 0.3896 - acc: 0.8375 - val_loss: 0.4956 - val_acc: 0.7977
    Epoch 35/50
    125/125 [==============================] - 11s - loss: 0.3985 - acc: 0.8365 - val_loss: 0.5035 - val_acc: 0.7608
    Epoch 36/50
    125/125 [==============================] - 12s - loss: 0.3733 - acc: 0.8430 - val_loss: 0.5348 - val_acc: 0.7926
    Epoch 37/50
    125/125 [==============================] - 12s - loss: 0.3794 - acc: 0.8270 - val_loss: 0.4949 - val_acc: 0.7735
    Epoch 38/50
    125/125 [==============================] - 11s - loss: 0.3830 - acc: 0.8390 - val_loss: 0.5987 - val_acc: 0.7888
    Epoch 39/50
    125/125 [==============================] - 11s - loss: 0.3957 - acc: 0.8425 - val_loss: 0.5248 - val_acc: 0.8130
    Epoch 40/50
    125/125 [==============================] - 12s - loss: 0.3808 - acc: 0.8410 - val_loss: 0.5187 - val_acc: 0.7634
    Epoch 41/50
    125/125 [==============================] - 11s - loss: 0.3741 - acc: 0.8435 - val_loss: 0.5924 - val_acc: 0.8015
    Epoch 42/50
    125/125 [==============================] - 12s - loss: 0.3817 - acc: 0.8425 - val_loss: 0.5502 - val_acc: 0.7824
    Epoch 43/50
    125/125 [==============================] - 12s - loss: 0.3775 - acc: 0.8385 - val_loss: 0.5800 - val_acc: 0.7710
    Epoch 44/50
    125/125 [==============================] - 12s - loss: 0.3770 - acc: 0.8475 - val_loss: 0.7753 - val_acc: 0.7595
    Epoch 45/50
    125/125 [==============================] - 12s - loss: 0.3827 - acc: 0.8410 - val_loss: 0.6078 - val_acc: 0.7888
    Epoch 46/50
    125/125 [==============================] - 12s - loss: 0.3764 - acc: 0.8425 - val_loss: 0.5217 - val_acc: 0.7875
    Epoch 47/50
    125/125 [==============================] - 11s - loss: 0.4025 - acc: 0.8395 - val_loss: 0.5281 - val_acc: 0.7634
    Epoch 48/50
    125/125 [==============================] - 12s - loss: 0.3460 - acc: 0.8475 - val_loss: 0.7290 - val_acc: 0.8079
    Epoch 49/50
    125/125 [==============================] - 12s - loss: 0.3800 - acc: 0.8475 - val_loss: 0.7074 - val_acc: 0.7964
    Epoch 50/50
    125/125 [==============================] - 12s - loss: 0.4052 - acc: 0.8390 - val_loss: 0.5712 - val_acc: 0.7672
    

Step 9: Plot the accuracy from history


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

    dict_keys(['acc', 'loss', 'val_loss', 'val_acc'])
    


![_config.yml]({{ site.baseurl}}/images/littledata_output_20_1.png)


Step 10: Sample prediction

![](https://github.com/melvincabatuan/melvincabatuan.github.io/blob/master/images/cat.jpg)


```python
from keras.preprocessing import image

test_image = image.load_img('cat.jpg', target_size = (img_width, img_height))
test_image = image.img_to_array(test_image)
test_image = np.expand_dims(test_image, axis = 0)

result = model.predict(test_image)
train_generator.class_indices
if result[0][0] == 1:
    prediction = 'dog'
else:
    prediction = 'cat'
print(prediction)
```

    cat
    


	
![](https://github.com/melvincabatuan/melvincabatuan.github.io/blob/master/images/dog.jpg)	
	
	
```python
test_image = image.load_img('dog.jpg', target_size = (img_width, img_height))
test_image = image.img_to_array(test_image)
test_image = np.expand_dims(test_image, axis = 0)

result = model.predict(test_image)
train_generator.class_indices
if result[0][0] == 1:
    prediction = 'dog'
else:
    prediction = 'cat'
print(prediction)
```

    dog
    
- mkc
