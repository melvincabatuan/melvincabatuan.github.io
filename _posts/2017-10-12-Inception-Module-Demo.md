
## Step 1: Organize Imports


```python
from keras import backend as K
from keras.datasets import cifar10
from keras.utils import np_utils
from keras.models import Sequential
from keras.layers.core import Dense, Dropout, Activation, Flatten
from keras.layers.convolutional import Conv2D, MaxPooling2D
from keras.optimizers import SGD, Adam, RMSprop
from keras.models import Model
from keras.layers import Input, concatenate

from keras.preprocessing.image import ImageDataGenerator

import matplotlib.pyplot as plt
%matplotlib inline
```

## Step 2: Set the constants </br>
CIFAR_10 is a set of 60K images 32x32 pixels on 3 channels


```python
IMG_CHANNELS = 3
IMG_ROWS = 32
IMG_COLS = 32
INPUT_SHAPE = (IMG_ROWS, IMG_COLS, IMG_CHANNELS)   # channels last

BATCH_SIZE = 128
NB_EPOCH = 50
NB_CLASSES = 10
VERBOSE = 1
VALIDATION_SPLIT = 0.2
OPTIM = Adam()
```

## Step 3: Load dataset


```python
(X_train, y_train), (X_test, y_test) = cifar10.load_data()
print('X_train shape:', X_train.shape)
print(X_train.shape[0], 'train samples')
print(X_test.shape[0], 'test samples')
```

    X_train shape: (50000, 32, 32, 3)
    50000 train samples
    10000 test samples
    

## Step 4: Convert to categorical (OHE)


```python
Y_train = np_utils.to_categorical(y_train, NB_CLASSES)
Y_test = np_utils.to_categorical(y_test, NB_CLASSES) 
```

## Step 5: Convert to float and normalize


```python
X_train = X_train.astype('float32')
X_test = X_test.astype('float32')
X_train /= 255
X_test /= 255
```

## Step 6: Define the Inception Module


```python
input_img = Input(shape = INPUT_SHAPE)

tower_1 = Conv2D(64, (1,1), padding='same', activation='relu')(input_img)
tower_1 = Conv2D(64, (3,3), padding='same', activation='relu')(tower_1)

tower_2 = Conv2D(64, (1,1), padding='same', activation='relu')(input_img)
tower_2 = Conv2D(64, (5,5), padding='same', activation='relu')(tower_2)

tower_3 = MaxPooling2D((3,3), strides=(1,1), padding='same')(input_img)
tower_3 = Conv2D(64, (1,1), padding='same', activation='relu')(tower_3)

output = concatenate([tower_1, tower_2, tower_3], axis = 3)

output = Flatten()(output)

out    = Dense(10, activation='softmax')(output)

model = Model(inputs = input_img, outputs = out)

model.summary()
```

    ____________________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                     
    ====================================================================================================
    input_3 (InputLayer)             (None, 32, 32, 3)     0                                            
    ____________________________________________________________________________________________________
    conv2d_13 (Conv2D)               (None, 32, 32, 64)    256         input_3[0][0]                    
    ____________________________________________________________________________________________________
    conv2d_15 (Conv2D)               (None, 32, 32, 64)    256         input_3[0][0]                    
    ____________________________________________________________________________________________________
    max_pooling2d_3 (MaxPooling2D)   (None, 32, 32, 3)     0           input_3[0][0]                    
    ____________________________________________________________________________________________________
    conv2d_14 (Conv2D)               (None, 32, 32, 64)    36928       conv2d_13[0][0]                  
    ____________________________________________________________________________________________________
    conv2d_16 (Conv2D)               (None, 32, 32, 64)    102464      conv2d_15[0][0]                  
    ____________________________________________________________________________________________________
    conv2d_17 (Conv2D)               (None, 32, 32, 64)    256         max_pooling2d_3[0][0]            
    ____________________________________________________________________________________________________
    concatenate_2 (Concatenate)      (None, 32, 32, 192)   0           conv2d_14[0][0]                  
                                                                       conv2d_16[0][0]                  
                                                                       conv2d_17[0][0]                  
    ____________________________________________________________________________________________________
    flatten_2 (Flatten)              (None, 196608)        0           concatenate_2[0][0]              
    ____________________________________________________________________________________________________
    dense_2 (Dense)                  (None, 10)            1966090     flatten_2[0][0]                  
    ====================================================================================================
    Total params: 2,106,250
    Trainable params: 2,106,250
    Non-trainable params: 0
    ____________________________________________________________________________________________________
    

## Step 7: Define the ImageDataGenerator object


```python
datagen = ImageDataGenerator(
rotation_range=20,       # 0 to 180
width_shift_range=0.2,   # horizontal-translation
height_shift_range=0.2,  # vertical-translation
zoom_range=0.1,          # random zoom
horizontal_flip=True,
fill_mode='nearest')     # filling in pixels strategy
```

## Step 8: Train the model


```python
model.compile(loss='categorical_crossentropy', optimizer=OPTIM, metrics=['accuracy'])

history = model.fit_generator(datagen.flow(X_train, Y_train,
                                           batch_size=BATCH_SIZE), samples_per_epoch=X_train.shape[0],
                              epochs=NB_EPOCH, verbose=VERBOSE)
 
# history = model.fit(X_train, Y_train, batch_size=BATCH_SIZE,
#	epochs=NB_EPOCH, validation_split=VALIDATION_SPLIT, 
#	verbose=VERBOSE)
```

    C:\Program Files\Anaconda3\envs\tensorflow-gpu\lib\site-packages\ipykernel_launcher.py:5: UserWarning: Update your `fit_generator` call to the Keras 2 API: `fit_generator(<keras.pre..., verbose=1, epochs=50, steps_per_epoch=390)`
      """
    

    Epoch 1/50
    390/390 [==============================] - 17s - loss: 1.8867 - acc: 0.3573    
    Epoch 2/50
    390/390 [==============================] - 15s - loss: 1.5454 - acc: 0.4483    
    Epoch 3/50
    390/390 [==============================] - 15s - loss: 1.4896 - acc: 0.4665    
    Epoch 4/50
    390/390 [==============================] - 15s - loss: 1.4443 - acc: 0.4884    
    Epoch 5/50
    390/390 [==============================] - 15s - loss: 1.4086 - acc: 0.4991    
    Epoch 6/50
    390/390 [==============================] - 15s - loss: 1.3828 - acc: 0.5100    
    Epoch 7/50
    390/390 [==============================] - 15s - loss: 1.3566 - acc: 0.5198    
    Epoch 8/50
    390/390 [==============================] - 15s - loss: 1.3448 - acc: 0.5236    - ETA
    Epoch 9/50
    390/390 [==============================] - 15s - loss: 1.3231 - acc: 0.5341    
    Epoch 10/50
    390/390 [==============================] - 15s - loss: 1.3173 - acc: 0.5346    
    Epoch 11/50
    390/390 [==============================] - 15s - loss: 1.2989 - acc: 0.5422    
    Epoch 12/50
    390/390 [==============================] - 15s - loss: 1.2978 - acc: 0.5425    
    Epoch 13/50
    390/390 [==============================] - 15s - loss: 1.2767 - acc: 0.5469    
    Epoch 14/50
    390/390 [==============================] - 15s - loss: 1.2758 - acc: 0.5507    
    Epoch 15/50
    390/390 [==============================] - 15s - loss: 1.2621 - acc: 0.5562    
    Epoch 16/50
    390/390 [==============================] - 15s - loss: 1.2617 - acc: 0.5555    
    Epoch 17/50
    390/390 [==============================] - 15s - loss: 1.2600 - acc: 0.5581    
    Epoch 18/50
    390/390 [==============================] - 15s - loss: 1.2431 - acc: 0.5615    
    Epoch 19/50
    390/390 [==============================] - 15s - loss: 1.2374 - acc: 0.5662    
    Epoch 20/50
    390/390 [==============================] - 15s - loss: 1.2330 - acc: 0.5644    - 
    Epoch 21/50
    390/390 [==============================] - 15s - loss: 1.2297 - acc: 0.5688    
    Epoch 22/50
    390/390 [==============================] - 15s - loss: 1.2211 - acc: 0.5711    
    Epoch 23/50
    390/390 [==============================] - 15s - loss: 1.2168 - acc: 0.5702    
    Epoch 24/50
    390/390 [==============================] - 15s - loss: 1.2125 - acc: 0.5757    
    Epoch 25/50
    390/390 [==============================] - 15s - loss: 1.2072 - acc: 0.5763    
    Epoch 26/50
    390/390 [==============================] - 15s - loss: 1.2026 - acc: 0.5790    
    Epoch 27/50
    390/390 [==============================] - 15s - loss: 1.1967 - acc: 0.5804    - ETA: 1s - los
    Epoch 28/50
    390/390 [==============================] - 15s - loss: 1.2000 - acc: 0.5780    
    Epoch 29/50
    390/390 [==============================] - 15s - loss: 1.1949 - acc: 0.5778    
    Epoch 30/50
    390/390 [==============================] - 15s - loss: 1.1890 - acc: 0.5836    
    Epoch 31/50
    390/390 [==============================] - 15s - loss: 1.1827 - acc: 0.5861    
    Epoch 32/50
    390/390 [==============================] - 15s - loss: 1.1911 - acc: 0.5827    
    Epoch 33/50
    390/390 [==============================] - 15s - loss: 1.1748 - acc: 0.5866    
    Epoch 34/50
    390/390 [==============================] - 15s - loss: 1.1790 - acc: 0.5855    
    Epoch 35/50
    390/390 [==============================] - 15s - loss: 1.1777 - acc: 0.5853    
    Epoch 36/50
    390/390 [==============================] - 15s - loss: 1.1709 - acc: 0.5859    
    Epoch 37/50
    390/390 [==============================] - 15s - loss: 1.1639 - acc: 0.5909    
    Epoch 38/50
    390/390 [==============================] - 15s - loss: 1.1663 - acc: 0.5904    
    Epoch 39/50
    390/390 [==============================] - 15s - loss: 1.1650 - acc: 0.5895    
    Epoch 40/50
    390/390 [==============================] - 15s - loss: 1.1619 - acc: 0.5916    
    Epoch 41/50
    390/390 [==============================] - 15s - loss: 1.1609 - acc: 0.5922    
    Epoch 42/50
    390/390 [==============================] - 15s - loss: 1.1523 - acc: 0.5914    
    Epoch 43/50
    390/390 [==============================] - 15s - loss: 1.1533 - acc: 0.5941    
    Epoch 44/50
    390/390 [==============================] - 15s - loss: 1.1531 - acc: 0.5954    
    Epoch 45/50
    390/390 [==============================] - 15s - loss: 1.1512 - acc: 0.5969    
    Epoch 46/50
    390/390 [==============================] - 15s - loss: 1.1463 - acc: 0.5981    
    Epoch 47/50
    390/390 [==============================] - 15s - loss: 1.1449 - acc: 0.5981    
    Epoch 48/50
    390/390 [==============================] - 15s - loss: 1.1412 - acc: 0.5974    
    Epoch 49/50
    390/390 [==============================] - 15s - loss: 1.1443 - acc: 0.5962    
    Epoch 50/50
    390/390 [==============================] - 15s - loss: 1.1362 - acc: 0.6008    
    

## Step 9: Test the model


```python
score = model.evaluate(X_test, Y_test,
                     batch_size=BATCH_SIZE, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

     9984/10000 [============================>.] - ETA: 0s
    Test score: 0.982766229439
    Test accuracy: 0.6594
    

## Step 10: Save the model


```python
model_json = model.to_json()
open('cifar10_architecture.json', 'w').write(model_json)
model.save_weights('cifar10_weights.h5', overwrite=True)
```

## Step 11: Plot the accuracy


```python
print(history.history.keys())
plt.plot(history.history['acc'])
# plt.plot(history.history['val_acc'])
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.legend(['train'], loc='upper left')
plt.show()
```

    dict_keys(['acc', 'loss'])
    


![_config.yml]({{ site.baseurl}}/images/inception_output_21_1.png)


## Step 13: Plot loss history


```python
plt.plot(history.history['loss'])
#plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train'], loc='upper left')
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/inception_output_23_0.png)


## Step 13: Test on random images


```python
import numpy as np
 

disp_images = []
for i in np.random.choice(np.arange(0, len(Y_test)), size=(10, )):
        probs = model.predict(X_test[np.newaxis, i])
        print('Probability: %s' % probs)     
        prediction = probs.argmax(axis=1)
        print('Prediction: %s' % probs[0][prediction])  
        image = (X_test[i] * 255).astype("uint8")
        disp_images.append(image)
        print("[INFO] Predicted: {}, Actual: {}".format(prediction[0], np.argmax(Y_test[i])))  
        print('\n')
        
fig = plt.figure(figsize= (20,50))
for k in range(10):
    plt.subplot(1, 10, k+1)      
    plt.gca().axes.get_yaxis().set_visible(False)
    plt.gca().axes.get_xaxis().set_visible(False)
    plt.imshow(disp_images[k])
```

    Probability: [[ 0.0018246   0.00865341  0.12834515  0.39300814  0.11174847  0.15422654
       0.13835931  0.0555375   0.00453933  0.00375758]]
    Prediction: [ 0.39300814]
    [INFO] Predicted: 3, Actual: 6
    
    
    Probability: [[ 0.0139301   0.01829209  0.14747213  0.25828251  0.13413012  0.12674768
       0.11731534  0.10055181  0.06505118  0.01822706]]
    Prediction: [ 0.25828251]
    [INFO] Predicted: 3, Actual: 3
    
    
    Probability: [[  9.71127689e-01   1.15318922e-02   6.54235855e-03   9.54762218e-05
        1.25066828e-04   8.15707608e-06   1.92407082e-04   1.66015543e-05
        7.02026486e-03   3.34009482e-03]]
    Prediction: [ 0.97112769]
    [INFO] Predicted: 0, Actual: 0
    
    
    Probability: [[  1.09853488e-06   6.58453524e-01   2.81217126e-06   2.42703222e-03
        8.60351247e-06   2.24124431e-03   1.20651725e-06   5.15462074e-04
        2.04664376e-03   3.34302455e-01]]
    Prediction: [ 0.65845352]
    [INFO] Predicted: 1, Actual: 2
    
    
    Probability: [[  3.43735563e-03   7.66683042e-01   1.57865725e-04   1.21283787e-03
        1.76112459e-04   1.14664377e-04   5.46371775e-05   1.75605455e-04
        1.18388301e-02   2.16149002e-01]]
    Prediction: [ 0.76668304]
    [INFO] Predicted: 1, Actual: 1
    
    
    Probability: [[  5.36347628e-01   1.83941759e-02   1.76991001e-02   1.30466954e-03
        1.13334050e-02   1.51568645e-04   7.04725826e-05   3.60318981e-02
        7.08951382e-03   3.71577621e-01]]
    Prediction: [ 0.53634763]
    [INFO] Predicted: 0, Actual: 0
    
    
    Probability: [[  1.43321568e-05   1.24129801e-04   4.89310920e-03   2.51284316e-02
        6.90007699e-04   8.22760016e-02   7.46636745e-03   8.72446597e-01
        2.08455876e-05   6.94023212e-03]]
    Prediction: [ 0.8724466]
    [INFO] Predicted: 7, Actual: 5
    
    
    Probability: [[  1.38641079e-03   7.40182804e-05   1.26985431e-01   1.52738225e-02
        3.27417791e-01   1.31353319e-01   3.94519372e-03   3.92506540e-01
        2.02483661e-05   1.03735726e-03]]
    Prediction: [ 0.39250654]
    [INFO] Predicted: 7, Actual: 7
    
    
    Probability: [[  1.23202917e-04   3.44213695e-05   2.46911179e-02   7.08036900e-01
        6.77241432e-03   2.16207653e-01   2.39793770e-02   2.00042427e-02
        4.97957590e-05   1.00937301e-04]]
    Prediction: [ 0.7080369]
    [INFO] Predicted: 3, Actual: 3
    
    
    Probability: [[  3.26205678e-02   6.01756526e-03   1.28423022e-02   8.73213925e-04
        6.77449852e-02   2.05494254e-03   7.52695054e-02   7.61110961e-01
        2.09391947e-05   4.14450802e-02]]
    Prediction: [ 0.76111096]
    [INFO] Predicted: 7, Actual: 9
    
    
    


![_config.yml]({{ site.baseurl}}/images/inception_output_25_1.png)

-mkc