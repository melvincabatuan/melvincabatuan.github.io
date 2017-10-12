
## A Keras Tutorial on Image Classification With Little Data

Objective: To be able to develop a model for distinguishing cats and dogs using transfer of learning

Source: "Building powerful image classification models using very little data" from blog.keras.io.

Step 1: Download data at: https://www.kaggle.com/c/dogs-vs-cats/data

Step 2: Setup overview
- created a data/ folder
- created train/ and validation/ subfolders inside data/
- created cats/ and dogs/ subfolders inside train/ and validation/
- put the cat pictures index 0-999 in data/train/cats
- put the cat pictures index 1000-1400 in data/validation/cats
- put the dogs pictures index 0-999 in data/train/dogs
- put the dog pictures index 1000-1400 in data/validation/dogs

Step 3: Organize imports


```python
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Dropout, Flatten, Dense
from keras import applications
from keras import backend as K
import numpy as np

import matplotlib.pyplot as plt
%matplotlib inline
```

Step 4: Set constants and path 


```python
img_width, img_height = 150, 150

top_model_weights_path = 'bottleneck_fc_model.h5'
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

Step 6: Build the model

![](https://blog.keras.io/img/imgclf/vgg16_modified.png)

```python
model = applications.VGG16(include_top=False, weights='imagenet')
model.summary()
```

    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    input_1 (InputLayer)         (None, None, None, 3)     0         
    _________________________________________________________________
    block1_conv1 (Conv2D)        (None, None, None, 64)    1792      
    _________________________________________________________________
    block1_conv2 (Conv2D)        (None, None, None, 64)    36928     
    _________________________________________________________________
    block1_pool (MaxPooling2D)   (None, None, None, 64)    0         
    _________________________________________________________________
    block2_conv1 (Conv2D)        (None, None, None, 128)   73856     
    _________________________________________________________________
    block2_conv2 (Conv2D)        (None, None, None, 128)   147584    
    _________________________________________________________________
    block2_pool (MaxPooling2D)   (None, None, None, 128)   0         
    _________________________________________________________________
    block3_conv1 (Conv2D)        (None, None, None, 256)   295168    
    _________________________________________________________________
    block3_conv2 (Conv2D)        (None, None, None, 256)   590080    
    _________________________________________________________________
    block3_conv3 (Conv2D)        (None, None, None, 256)   590080    
    _________________________________________________________________
    block3_pool (MaxPooling2D)   (None, None, None, 256)   0         
    _________________________________________________________________
    block4_conv1 (Conv2D)        (None, None, None, 512)   1180160   
    _________________________________________________________________
    block4_conv2 (Conv2D)        (None, None, None, 512)   2359808   
    _________________________________________________________________
    block4_conv3 (Conv2D)        (None, None, None, 512)   2359808   
    _________________________________________________________________
    block4_pool (MaxPooling2D)   (None, None, None, 512)   0         
    _________________________________________________________________
    block5_conv1 (Conv2D)        (None, None, None, 512)   2359808   
    _________________________________________________________________
    block5_conv2 (Conv2D)        (None, None, None, 512)   2359808   
    _________________________________________________________________
    block5_conv3 (Conv2D)        (None, None, None, 512)   2359808   
    _________________________________________________________________
    block5_pool (MaxPooling2D)   (None, None, None, 512)   0         
    =================================================================
    Total params: 14,714,688
    Trainable params: 14,714,688
    Non-trainable params: 0
    _________________________________________________________________
    

Step 7: Define Image Data Generator and save features


```python
datagen = ImageDataGenerator(rescale=1. / 255)
    
generator = datagen.flow_from_directory(
        train_data_dir,
        target_size=(img_width, img_height),
        batch_size=batch_size,
        class_mode=None,
        shuffle=False)

bottleneck_features_train = model.predict_generator(
        generator, nb_train_samples // batch_size)
np.save(open('bottleneck_features_train.npy', 'wb'),
            bottleneck_features_train)

generator = datagen.flow_from_directory(
        validation_data_dir,
        target_size=(img_width, img_height),
        batch_size=batch_size,
        class_mode=None,
        shuffle=False)

bottleneck_features_validation = model.predict_generator(
        generator, nb_validation_samples // batch_size)
np.save(open('bottleneck_features_validation.npy', 'wb'), bottleneck_features_validation)
```

    Found 2000 images belonging to 2 classes.
    Found 802 images belonging to 2 classes.
    

Step 8: Train


```python
train_data = np.load(open('bottleneck_features_train.npy','rb'))
train_labels = np.array([0] * (nb_train_samples // 2) + [1] * (nb_train_samples // 2))

validation_data = np.load(open('bottleneck_features_validation.npy','rb'))
validation_labels = np.array([0] * (nb_validation_samples // 2) + [1] * (nb_validation_samples // 2))
```


```python
model = Sequential()
model.add(Flatten(input_shape=train_data.shape[1:]))
model.add(Dense(256, activation='relu'))
model.add(Dropout(0.5))
model.add(Dense(1, activation='sigmoid'))
model.summary()

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
```

    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    flatten_6 (Flatten)          (None, 8192)              0         
    _________________________________________________________________
    dense_11 (Dense)             (None, 256)               2097408   
    _________________________________________________________________
    dropout_6 (Dropout)          (None, 256)               0         
    _________________________________________________________________
    dense_12 (Dense)             (None, 1)                 257       
    =================================================================
    Total params: 2,097,665
    Trainable params: 2,097,665
    Non-trainable params: 0
    _________________________________________________________________
    


```python
history = model.fit(train_data, train_labels,
             epochs=epochs,
             batch_size=batch_size,
             validation_data=(validation_data, validation_labels))
```

    Train on 2000 samples, validate on 800 samples
    Epoch 1/50
    2000/2000 [==============================] - 0s - loss: 0.4358 - acc: 0.8090 - val_loss: 0.3306 - val_acc: 0.8500
    Epoch 2/50
    2000/2000 [==============================] - 0s - loss: 0.2662 - acc: 0.8800 - val_loss: 0.2878 - val_acc: 0.8888
    Epoch 3/50
    2000/2000 [==============================] - 0s - loss: 0.2077 - acc: 0.9140 - val_loss: 0.2773 - val_acc: 0.8888
    Epoch 4/50
    2000/2000 [==============================] - 0s - loss: 0.1801 - acc: 0.9280 - val_loss: 0.2431 - val_acc: 0.9012
    Epoch 5/50
    2000/2000 [==============================] - 0s - loss: 0.1488 - acc: 0.9450 - val_loss: 0.2446 - val_acc: 0.9025
    Epoch 6/50
    2000/2000 [==============================] - 0s - loss: 0.1393 - acc: 0.9465 - val_loss: 0.2814 - val_acc: 0.8975
    Epoch 7/50
    2000/2000 [==============================] - 0s - loss: 0.1077 - acc: 0.9565 - val_loss: 0.3106 - val_acc: 0.8900
    Epoch 8/50
    2000/2000 [==============================] - 0s - loss: 0.0765 - acc: 0.9730 - val_loss: 0.2872 - val_acc: 0.9000
    Epoch 9/50
    2000/2000 [==============================] - 0s - loss: 0.0665 - acc: 0.9775 - val_loss: 0.3136 - val_acc: 0.8875
    Epoch 10/50
    2000/2000 [==============================] - 0s - loss: 0.0720 - acc: 0.9720 - val_loss: 0.3013 - val_acc: 0.9000
    Epoch 11/50
    2000/2000 [==============================] - 0s - loss: 0.0697 - acc: 0.9740 - val_loss: 0.4359 - val_acc: 0.8700
    Epoch 12/50
    2000/2000 [==============================] - 0s - loss: 0.0585 - acc: 0.9785 - val_loss: 0.3115 - val_acc: 0.9025
    Epoch 13/50
    2000/2000 [==============================] - 0s - loss: 0.0386 - acc: 0.9875 - val_loss: 0.4747 - val_acc: 0.8762
    Epoch 14/50
    2000/2000 [==============================] - 0s - loss: 0.0571 - acc: 0.9770 - val_loss: 0.3429 - val_acc: 0.8950
    Epoch 15/50
    2000/2000 [==============================] - 0s - loss: 0.0293 - acc: 0.9925 - val_loss: 0.3814 - val_acc: 0.9075
    Epoch 16/50
    2000/2000 [==============================] - 0s - loss: 0.0227 - acc: 0.9920 - val_loss: 0.3657 - val_acc: 0.9062
    Epoch 17/50
    2000/2000 [==============================] - 0s - loss: 0.0308 - acc: 0.9910 - val_loss: 0.4062 - val_acc: 0.8975
    Epoch 18/50
    2000/2000 [==============================] - 0s - loss: 0.0142 - acc: 0.9980 - val_loss: 0.4110 - val_acc: 0.9038
    Epoch 19/50
    2000/2000 [==============================] - 0s - loss: 0.0355 - acc: 0.9885 - val_loss: 0.4744 - val_acc: 0.8712
    Epoch 20/50
    2000/2000 [==============================] - 0s - loss: 0.0574 - acc: 0.9765 - val_loss: 0.3826 - val_acc: 0.8975
    Epoch 21/50
    2000/2000 [==============================] - 0s - loss: 0.0352 - acc: 0.9885 - val_loss: 0.4523 - val_acc: 0.8950
    Epoch 22/50
    2000/2000 [==============================] - 0s - loss: 0.0424 - acc: 0.9850 - val_loss: 0.4419 - val_acc: 0.8888
    Epoch 23/50
    2000/2000 [==============================] - 0s - loss: 0.0371 - acc: 0.9880 - val_loss: 0.4625 - val_acc: 0.8875
    Epoch 24/50
    2000/2000 [==============================] - 0s - loss: 0.0156 - acc: 0.9970 - val_loss: 0.5719 - val_acc: 0.8875
    Epoch 25/50
    2000/2000 [==============================] - 0s - loss: 0.0167 - acc: 0.9930 - val_loss: 0.4637 - val_acc: 0.9012
    Epoch 26/50
    2000/2000 [==============================] - 0s - loss: 0.0279 - acc: 0.9890 - val_loss: 0.4782 - val_acc: 0.9025
    Epoch 27/50
    2000/2000 [==============================] - 0s - loss: 0.0377 - acc: 0.9875 - val_loss: 0.4123 - val_acc: 0.8950
    Epoch 28/50
    2000/2000 [==============================] - 0s - loss: 0.0210 - acc: 0.9935 - val_loss: 0.5044 - val_acc: 0.9038
    Epoch 29/50
    2000/2000 [==============================] - 0s - loss: 0.0192 - acc: 0.9930 - val_loss: 0.5502 - val_acc: 0.8938
    Epoch 30/50
    2000/2000 [==============================] - 0s - loss: 0.0265 - acc: 0.9905 - val_loss: 0.6185 - val_acc: 0.8800
    Epoch 31/50
    2000/2000 [==============================] - 0s - loss: 0.0304 - acc: 0.9880 - val_loss: 0.4137 - val_acc: 0.8975
    Epoch 32/50
    2000/2000 [==============================] - 0s - loss: 0.0273 - acc: 0.9910 - val_loss: 0.5884 - val_acc: 0.8725
    Epoch 33/50
    2000/2000 [==============================] - 0s - loss: 0.0135 - acc: 0.9955 - val_loss: 0.4928 - val_acc: 0.9100
    Epoch 34/50
    2000/2000 [==============================] - 0s - loss: 0.0136 - acc: 0.9945 - val_loss: 0.4501 - val_acc: 0.9012
    Epoch 35/50
    2000/2000 [==============================] - 0s - loss: 0.0111 - acc: 0.9970 - val_loss: 0.5428 - val_acc: 0.9025
    Epoch 36/50
    2000/2000 [==============================] - 0s - loss: 0.0132 - acc: 0.9960 - val_loss: 0.7411 - val_acc: 0.8838
    Epoch 37/50
    2000/2000 [==============================] - 0s - loss: 0.0226 - acc: 0.9905 - val_loss: 0.5674 - val_acc: 0.8825
    Epoch 38/50
    2000/2000 [==============================] - 0s - loss: 0.0122 - acc: 0.9965 - val_loss: 0.6416 - val_acc: 0.8912
    Epoch 39/50
    2000/2000 [==============================] - 0s - loss: 0.0493 - acc: 0.9825 - val_loss: 0.4783 - val_acc: 0.8962
    Epoch 40/50
    2000/2000 [==============================] - 0s - loss: 0.0315 - acc: 0.9885 - val_loss: 0.4995 - val_acc: 0.8912
    Epoch 41/50
    2000/2000 [==============================] - 0s - loss: 0.0244 - acc: 0.9915 - val_loss: 0.6473 - val_acc: 0.8788
    Epoch 42/50
    2000/2000 [==============================] - 0s - loss: 0.0197 - acc: 0.9925 - val_loss: 0.4846 - val_acc: 0.8975
    Epoch 43/50
    2000/2000 [==============================] - 0s - loss: 0.0244 - acc: 0.9920 - val_loss: 0.4959 - val_acc: 0.8975
    Epoch 44/50
    2000/2000 [==============================] - 0s - loss: 0.0162 - acc: 0.9930 - val_loss: 0.6148 - val_acc: 0.8988
    Epoch 45/50
    2000/2000 [==============================] - 0s - loss: 0.0283 - acc: 0.9895 - val_loss: 0.5494 - val_acc: 0.8850
    Epoch 46/50
    2000/2000 [==============================] - 0s - loss: 0.0212 - acc: 0.9910 - val_loss: 0.5134 - val_acc: 0.8962
    Epoch 47/50
    2000/2000 [==============================] - 0s - loss: 0.0085 - acc: 0.9970 - val_loss: 0.6296 - val_acc: 0.8988
    Epoch 48/50
    2000/2000 [==============================] - 0s - loss: 0.0658 - acc: 0.9740 - val_loss: 0.4389 - val_acc: 0.9075
    Epoch 49/50
    2000/2000 [==============================] - 0s - loss: 0.0171 - acc: 0.9950 - val_loss: 0.5721 - val_acc: 0.9000
    Epoch 50/50
    2000/2000 [==============================] - 0s - loss: 0.0117 - acc: 0.9945 - val_loss: 0.5057 - val_acc: 0.9012
    

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

    dict_keys(['loss', 'acc', 'val_loss', 'val_acc'])
    


![_config.yml]({{ site.baseurl}}/images/littledata_vgg_output_20_1.png)

- mkc