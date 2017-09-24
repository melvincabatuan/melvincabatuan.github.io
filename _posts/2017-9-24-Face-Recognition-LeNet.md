Step 1: Organize imports


```python
from sklearn.model_selection import train_test_split  
from keras import backend as K
K.set_image_dim_ordering('th') # this works
from keras.utils import np_utils
from keras.optimizers import SGD, RMSprop, Adam
from keras.models import Sequential  
from keras.layers.convolutional import Conv2D
from keras.layers.convolutional import MaxPooling2D 
from keras.layers.core import Activation  
from keras.layers.core import Flatten  
from keras.layers.core import Dense
from keras.models import model_from_json
```

    Using TensorFlow backend.
    


```python
import numpy as np  
import cv2  
import os  
import sys 
import argparse 
from PIL import Image
from matplotlib import pyplot as plt
%matplotlib inline
```


```python
class LeNet:
	@staticmethod
	def build(input_shape, classes):
		model = Sequential()
		# CONV => RELU => POOL
		model.add(Conv2D(20, kernel_size=5, padding="same",
			input_shape=input_shape))
		model.add(Activation("relu"))
		model.add(MaxPooling2D(pool_size=(2, 2), strides=(2, 2)))
		# CONV => RELU => POOL
		model.add(Conv2D(50, kernel_size=5, padding="same"))
		model.add(Activation("relu"))
		model.add(MaxPooling2D(pool_size=(2, 2), strides=(2, 2)))
		# Flatten => RELU layers
		model.add(Flatten())
		model.add(Dense(500))
		model.add(Activation("relu"))
 
		# a softmax classifier
		model.add(Dense(classes))
		model.add(Activation("softmax"))

		return model
```


```python
NB_EPOCH = 200
BATCH_SIZE = 128
VERBOSE = 1
OPTIMIZER = Adam()
VALIDATION_SPLIT=0.2

IMG_ROWS, IMG_COLS = 28, 28 
NB_CLASSES = 10  
INPUT_SHAPE = (1, IMG_ROWS, IMG_COLS)

np.random.seed(1983)  # for reproducibility
```

Step 2: Define method to load images from subfolders of category


```python
def load_images_from_folders(folders, root_dir):
    print('Acquiring images...')
    images = []
    labels = []
    for folder in folders:        
        for filename in os.listdir(os.path.join(root_dir,folder)):
            if any([filename.endswith(x) for x in ['.jpeg', '.jpg','.pgm','png']]):
                img = cv2.imread(os.path.join(root_dir, folder, filename), cv2.IMREAD_GRAYSCALE)
                if img is not None:
                    image = cv2.resize(img, (IMG_COLS, IMG_ROWS)) 
                    image = np.array(image, 'uint8') # convert to numpy array
                    images.append(image)
                    label = os.path.split(folder)[1].split("_")[1] # number from person_0_
                    labels.append(label)
    return images, labels;
```

Step 3: Load images from subfolders of category: 10 persons; 50 images each


```python
folders = [
    'person_0_abad',
    'person_1_agui',
    'person_2_cans',
    'person_3_degu',
    'person_4_hato',
    'person_5_libb',
    'person_6_palo',
    'person_7_prim',
    'person_8_rosa',
    'person_9_venu'
]

root_dir = r'C:\Users\cobalt\workspace\FaceRecognition\front_database'

(images, labels) = load_images_from_folders(folders, root_dir)
print('No. of images = %s. ' %  len(images))
print('No. of labels = %s. ' %  len(labels))
print(labels[0])
```

    Acquiring images...
    No. of images = 500. 
    No. of labels = 500. 
    0
    

Step 4: Plot the first image.


```python
plt.imshow(images[0], cmap='gray')
```




    <matplotlib.image.AxesImage at 0x2a15b0d7438>




![_config.yml]({{ site.baseurl}}/images/facerecog_output_11_0.png)


Step 5: Define method for plotting a person: 50 images 


```python
def plot_person_category(images, id):
    for k in range(50*id,50*id + 50):
        plt.subplot(5, 10, k - (50*id - 1))      
        plt.gca().axes.get_yaxis().set_visible(False)
        plt.gca().axes.get_xaxis().set_visible(False)
        plt.imshow(images[k], cmap='gray')
```


```python
# Plot person 0
plot_person_category(images, 0)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_11_1.png)



```python
# Plot person 1
plot_person_category(images, 1)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_12_0.png)



```python
# Plot person 2
plot_person_category(images, 2)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_13_0.png)



```python
# Plot person 3
plot_person_category(images, 3)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_14_0.png)



```python
# Plot person 4
plot_person_category(images, 4)
```



![_config.yml]({{ site.baseurl}}/images/facerecog_output_15_0.png)



```python
# Plot person 5
plot_person_category(images, 5)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_16_0.png)



```python
# Plot person 6
plot_person_category(images, 6)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_17_0.png)



```python
# Plot person 7
plot_person_category(images, 7)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_18_0.png)



```python
# Plot person 8
plot_person_category(images, 8)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_19_0.png)



```python
# Plot person 9
plot_person_category(images, 9)
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_20_0.png)


Separate into test and train


```python
(trainData, testData, trainLabels, testLabels) = train_test_split(images, labels, test_size=0.10)
```

One-Hot Encoding


```python
print('Before: trainLabels[0] = %s' % trainLabels[0])
print('Before: testLabels[0] = %s' % testLabels[0])
trainLabels = np_utils.to_categorical(trainLabels, NB_CLASSES)  
testLabels = np_utils.to_categorical(testLabels, NB_CLASSES) 
print('After: trainLabels[0] = %s' % trainLabels[0])
print('After: testLabels[0] = %s' % testLabels[0])
```

    Before: trainLabels[0] = 9
    Before: testLabels[0] = 4
    After: trainLabels[0] = [ 0.  0.  0.  0.  0.  0.  0.  0.  0.  1.]
    After: testLabels[0] = [ 0.  0.  0.  0.  1.  0.  0.  0.  0.  0.]
    


```python
# Convert to NumPy array
trainData = np.asarray(trainData)  
testData = np.asarray(testData)  

# Convert uint to float32
trainData = trainData.astype('float32')
testData = testData.astype('float32')

# Normalize
trainData /= 255 
testData /= 255

# No. of Samples x [1 x 28 x 28] shape as input to the CONVNET
trainData = trainData[:, np.newaxis, :, :]
testData = testData[:, np.newaxis, :, :]

print(trainData.shape, 'train samples')
print(testData.shape, 'test samples')
```

    (450, 1, 28, 28) train samples
    (50, 1, 28, 28) test samples
    


```python
model = LeNet.build(input_shape=INPUT_SHAPE, classes=NB_CLASSES)
model.summary()
```

    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv2d_1 (Conv2D)            (None, 20, 28, 28)        520       
    _________________________________________________________________
    activation_1 (Activation)    (None, 20, 28, 28)        0         
    _________________________________________________________________
    max_pooling2d_1 (MaxPooling2 (None, 20, 14, 14)        0         
    _________________________________________________________________
    conv2d_2 (Conv2D)            (None, 50, 14, 14)        25050     
    _________________________________________________________________
    activation_2 (Activation)    (None, 50, 14, 14)        0         
    _________________________________________________________________
    max_pooling2d_2 (MaxPooling2 (None, 50, 7, 7)          0         
    _________________________________________________________________
    flatten_1 (Flatten)          (None, 2450)              0         
    _________________________________________________________________
    dense_1 (Dense)              (None, 500)               1225500   
    _________________________________________________________________
    activation_3 (Activation)    (None, 500)               0         
    _________________________________________________________________
    dense_2 (Dense)              (None, 10)                5010      
    _________________________________________________________________
    activation_4 (Activation)    (None, 10)                0         
    =================================================================
    Total params: 1,256,080
    Trainable params: 1,256,080
    Non-trainable params: 0
    _________________________________________________________________
    


```python
model.compile(loss='categorical_crossentropy',
              optimizer=OPTIMIZER,
              metrics=['accuracy'])
```


```python
history = model.fit(trainData, trainLabels,
                    batch_size=BATCH_SIZE, epochs=NB_EPOCH,
                    verbose=VERBOSE, validation_split=VALIDATION_SPLIT)
```

    Train on 360 samples, validate on 90 samples
    Epoch 1/200
    360/360 [==============================] - 2s - loss: 2.3452 - acc: 0.1028 - val_loss: 2.2137 - val_acc: 0.3333
    Epoch 2/200
    360/360 [==============================] - 2s - loss: 2.1671 - acc: 0.3111 - val_loss: 2.0855 - val_acc: 0.2778
    Epoch 3/200
    360/360 [==============================] - 2s - loss: 1.9784 - acc: 0.3417 - val_loss: 1.8160 - val_acc: 0.4556
    Epoch 4/200
    360/360 [==============================] - 2s - loss: 1.7140 - acc: 0.4667 - val_loss: 1.5235 - val_acc: 0.6333
    Epoch 5/200
    360/360 [==============================] - 2s - loss: 1.4624 - acc: 0.5778 - val_loss: 1.4190 - val_acc: 0.4778
    Epoch 6/200
    360/360 [==============================] - 2s - loss: 1.3565 - acc: 0.4972 - val_loss: 1.3731 - val_acc: 0.5444
    Epoch 7/200
    360/360 [==============================] - 2s - loss: 1.2495 - acc: 0.5667 - val_loss: 1.2482 - val_acc: 0.5778
    Epoch 8/200
    360/360 [==============================] - 2s - loss: 1.0745 - acc: 0.6528 - val_loss: 1.1067 - val_acc: 0.6889
    Epoch 9/200
    360/360 [==============================] - 2s - loss: 1.0017 - acc: 0.6639 - val_loss: 0.9497 - val_acc: 0.7111
    Epoch 10/200
    360/360 [==============================] - 2s - loss: 0.8675 - acc: 0.6944 - val_loss: 0.8913 - val_acc: 0.8222
    Epoch 11/200
    360/360 [==============================] - 2s - loss: 0.7489 - acc: 0.8083 - val_loss: 0.8248 - val_acc: 0.7333
    Epoch 12/200
    360/360 [==============================] - 2s - loss: 0.6251 - acc: 0.8361 - val_loss: 0.7060 - val_acc: 0.8667
    Epoch 13/200
    360/360 [==============================] - 2s - loss: 0.5279 - acc: 0.8972 - val_loss: 0.5960 - val_acc: 0.8667
    Epoch 14/200
    360/360 [==============================] - 2s - loss: 0.4468 - acc: 0.9028 - val_loss: 0.5569 - val_acc: 0.8556
    Epoch 15/200
    360/360 [==============================] - 2s - loss: 0.3793 - acc: 0.9333 - val_loss: 0.4327 - val_acc: 0.9111
    Epoch 16/200
    360/360 [==============================] - 2s - loss: 0.3398 - acc: 0.9111 - val_loss: 0.4705 - val_acc: 0.8667
    Epoch 17/200
    360/360 [==============================] - 2s - loss: 0.3304 - acc: 0.9111 - val_loss: 0.4155 - val_acc: 0.9000
    Epoch 18/200
    360/360 [==============================] - 2s - loss: 0.2747 - acc: 0.9389 - val_loss: 0.4036 - val_acc: 0.8556
    Epoch 19/200
    360/360 [==============================] - 2s - loss: 0.2262 - acc: 0.9361 - val_loss: 0.4583 - val_acc: 0.8778
    Epoch 20/200
    360/360 [==============================] - 2s - loss: 0.1916 - acc: 0.9500 - val_loss: 0.3545 - val_acc: 0.8778
    Epoch 21/200
    360/360 [==============================] - 2s - loss: 0.1646 - acc: 0.9639 - val_loss: 0.2726 - val_acc: 0.9222
    Epoch 22/200
    360/360 [==============================] - 2s - loss: 0.1340 - acc: 0.9750 - val_loss: 0.3217 - val_acc: 0.9111
    Epoch 23/200
    360/360 [==============================] - 2s - loss: 0.1141 - acc: 0.9806 - val_loss: 0.3324 - val_acc: 0.9111
    Epoch 24/200
    360/360 [==============================] - 2s - loss: 0.0863 - acc: 0.9917 - val_loss: 0.2397 - val_acc: 0.9444
    Epoch 25/200
    360/360 [==============================] - 2s - loss: 0.0722 - acc: 0.9889 - val_loss: 0.2567 - val_acc: 0.9222
    Epoch 26/200
    360/360 [==============================] - 2s - loss: 0.0569 - acc: 0.9944 - val_loss: 0.2518 - val_acc: 0.9333
    Epoch 27/200
    360/360 [==============================] - 2s - loss: 0.0463 - acc: 0.9972 - val_loss: 0.2391 - val_acc: 0.9333
    Epoch 28/200
    360/360 [==============================] - 2s - loss: 0.0344 - acc: 0.9972 - val_loss: 0.2581 - val_acc: 0.9333
    Epoch 29/200
    360/360 [==============================] - 2s - loss: 0.0306 - acc: 0.9972 - val_loss: 0.2346 - val_acc: 0.9444
    Epoch 30/200
    360/360 [==============================] - 2s - loss: 0.0220 - acc: 1.0000 - val_loss: 0.2154 - val_acc: 0.9444
    Epoch 31/200
    360/360 [==============================] - 2s - loss: 0.0204 - acc: 1.0000 - val_loss: 0.2404 - val_acc: 0.9333
    Epoch 32/200
    360/360 [==============================] - 2s - loss: 0.0152 - acc: 1.0000 - val_loss: 0.2258 - val_acc: 0.9444
    Epoch 33/200
    360/360 [==============================] - 2s - loss: 0.0143 - acc: 1.0000 - val_loss: 0.2214 - val_acc: 0.9556
    Epoch 34/200
    360/360 [==============================] - 2s - loss: 0.0125 - acc: 1.0000 - val_loss: 0.2308 - val_acc: 0.9444
    Epoch 35/200
    360/360 [==============================] - 2s - loss: 0.0103 - acc: 1.0000 - val_loss: 0.2287 - val_acc: 0.9444
    Epoch 36/200
    360/360 [==============================] - 2s - loss: 0.0094 - acc: 1.0000 - val_loss: 0.2187 - val_acc: 0.9444
    Epoch 37/200
    360/360 [==============================] - 2s - loss: 0.0085 - acc: 1.0000 - val_loss: 0.2126 - val_acc: 0.9444
    Epoch 38/200
    360/360 [==============================] - 2s - loss: 0.0075 - acc: 1.0000 - val_loss: 0.2206 - val_acc: 0.9556
    Epoch 39/200
    360/360 [==============================] - 2s - loss: 0.0070 - acc: 1.0000 - val_loss: 0.2232 - val_acc: 0.9444
    Epoch 40/200
    360/360 [==============================] - 2s - loss: 0.0063 - acc: 1.0000 - val_loss: 0.2229 - val_acc: 0.9444
    Epoch 41/200
    360/360 [==============================] - 2s - loss: 0.0060 - acc: 1.0000 - val_loss: 0.2273 - val_acc: 0.9444
    Epoch 42/200
    360/360 [==============================] - 2s - loss: 0.0055 - acc: 1.0000 - val_loss: 0.2272 - val_acc: 0.9444
    Epoch 43/200
    360/360 [==============================] - 2s - loss: 0.0053 - acc: 1.0000 - val_loss: 0.2205 - val_acc: 0.9444
    Epoch 44/200
    360/360 [==============================] - 2s - loss: 0.0050 - acc: 1.0000 - val_loss: 0.2166 - val_acc: 0.9444
    Epoch 45/200
    360/360 [==============================] - 2s - loss: 0.0047 - acc: 1.0000 - val_loss: 0.2257 - val_acc: 0.9444
    Epoch 46/200
    360/360 [==============================] - 2s - loss: 0.0044 - acc: 1.0000 - val_loss: 0.2310 - val_acc: 0.9444
    Epoch 47/200
    360/360 [==============================] - 2s - loss: 0.0042 - acc: 1.0000 - val_loss: 0.2303 - val_acc: 0.9444
    Epoch 48/200
    360/360 [==============================] - 2s - loss: 0.0040 - acc: 1.0000 - val_loss: 0.2277 - val_acc: 0.9444
    Epoch 49/200
    360/360 [==============================] - 2s - loss: 0.0038 - acc: 1.0000 - val_loss: 0.2268 - val_acc: 0.9444
    Epoch 50/200
    360/360 [==============================] - 2s - loss: 0.0037 - acc: 1.0000 - val_loss: 0.2277 - val_acc: 0.9444
    Epoch 51/200
    360/360 [==============================] - 2s - loss: 0.0035 - acc: 1.0000 - val_loss: 0.2284 - val_acc: 0.9444
    Epoch 52/200
    360/360 [==============================] - 2s - loss: 0.0034 - acc: 1.0000 - val_loss: 0.2296 - val_acc: 0.9444
    Epoch 53/200
    360/360 [==============================] - 2s - loss: 0.0033 - acc: 1.0000 - val_loss: 0.2333 - val_acc: 0.9444
    Epoch 54/200
    360/360 [==============================] - 2s - loss: 0.0032 - acc: 1.0000 - val_loss: 0.2329 - val_acc: 0.9444
    Epoch 55/200
    360/360 [==============================] - 2s - loss: 0.0030 - acc: 1.0000 - val_loss: 0.2320 - val_acc: 0.9444
    Epoch 56/200
    360/360 [==============================] - 2s - loss: 0.0029 - acc: 1.0000 - val_loss: 0.2322 - val_acc: 0.9444
    Epoch 57/200
    360/360 [==============================] - 2s - loss: 0.0028 - acc: 1.0000 - val_loss: 0.2332 - val_acc: 0.9444
    Epoch 58/200
    360/360 [==============================] - 2s - loss: 0.0027 - acc: 1.0000 - val_loss: 0.2328 - val_acc: 0.9444
    Epoch 59/200
    360/360 [==============================] - 2s - loss: 0.0026 - acc: 1.0000 - val_loss: 0.2305 - val_acc: 0.9444
    Epoch 60/200
    360/360 [==============================] - 2s - loss: 0.0026 - acc: 1.0000 - val_loss: 0.2324 - val_acc: 0.9444
    Epoch 61/200
    360/360 [==============================] - 2s - loss: 0.0025 - acc: 1.0000 - val_loss: 0.2356 - val_acc: 0.9444
    Epoch 62/200
    360/360 [==============================] - 2s - loss: 0.0024 - acc: 1.0000 - val_loss: 0.2377 - val_acc: 0.9444
    Epoch 63/200
    360/360 [==============================] - 2s - loss: 0.0023 - acc: 1.0000 - val_loss: 0.2374 - val_acc: 0.9444
    Epoch 64/200
    360/360 [==============================] - 2s - loss: 0.0023 - acc: 1.0000 - val_loss: 0.2384 - val_acc: 0.9444
    Epoch 65/200
    360/360 [==============================] - 2s - loss: 0.0022 - acc: 1.0000 - val_loss: 0.2384 - val_acc: 0.9444
    Epoch 66/200
    360/360 [==============================] - 2s - loss: 0.0021 - acc: 1.0000 - val_loss: 0.2362 - val_acc: 0.9444
    Epoch 67/200
    360/360 [==============================] - 2s - loss: 0.0021 - acc: 1.0000 - val_loss: 0.2345 - val_acc: 0.9444
    Epoch 68/200
    360/360 [==============================] - 2s - loss: 0.0020 - acc: 1.0000 - val_loss: 0.2342 - val_acc: 0.9444
    Epoch 69/200
    360/360 [==============================] - 2s - loss: 0.0019 - acc: 1.0000 - val_loss: 0.2358 - val_acc: 0.9444
    Epoch 70/200
    360/360 [==============================] - 2s - loss: 0.0019 - acc: 1.0000 - val_loss: 0.2378 - val_acc: 0.9444
    Epoch 71/200
    360/360 [==============================] - 2s - loss: 0.0018 - acc: 1.0000 - val_loss: 0.2410 - val_acc: 0.9444
    Epoch 72/200
    360/360 [==============================] - 2s - loss: 0.0018 - acc: 1.0000 - val_loss: 0.2416 - val_acc: 0.9444
    Epoch 73/200
    360/360 [==============================] - 2s - loss: 0.0017 - acc: 1.0000 - val_loss: 0.2396 - val_acc: 0.9444
    Epoch 74/200
    360/360 [==============================] - 2s - loss: 0.0017 - acc: 1.0000 - val_loss: 0.2397 - val_acc: 0.9444
    Epoch 75/200
    360/360 [==============================] - 2s - loss: 0.0017 - acc: 1.0000 - val_loss: 0.2400 - val_acc: 0.9444
    Epoch 76/200
    360/360 [==============================] - 2s - loss: 0.0016 - acc: 1.0000 - val_loss: 0.2406 - val_acc: 0.9444
    Epoch 77/200
    360/360 [==============================] - 2s - loss: 0.0016 - acc: 1.0000 - val_loss: 0.2404 - val_acc: 0.9444
    Epoch 78/200
    360/360 [==============================] - 2s - loss: 0.0015 - acc: 1.0000 - val_loss: 0.2406 - val_acc: 0.9444
    Epoch 79/200
    360/360 [==============================] - 2s - loss: 0.0015 - acc: 1.0000 - val_loss: 0.2407 - val_acc: 0.9444
    Epoch 80/200
    360/360 [==============================] - 2s - loss: 0.0015 - acc: 1.0000 - val_loss: 0.2430 - val_acc: 0.9444
    Epoch 81/200
    360/360 [==============================] - 2s - loss: 0.0014 - acc: 1.0000 - val_loss: 0.2444 - val_acc: 0.9444
    Epoch 82/200
    360/360 [==============================] - 2s - loss: 0.0014 - acc: 1.0000 - val_loss: 0.2454 - val_acc: 0.9444
    Epoch 83/200
    360/360 [==============================] - 2s - loss: 0.0014 - acc: 1.0000 - val_loss: 0.2449 - val_acc: 0.9444
    Epoch 84/200
    360/360 [==============================] - 2s - loss: 0.0013 - acc: 1.0000 - val_loss: 0.2439 - val_acc: 0.9444
    Epoch 85/200
    360/360 [==============================] - 2s - loss: 0.0013 - acc: 1.0000 - val_loss: 0.2435 - val_acc: 0.9444
    Epoch 86/200
    360/360 [==============================] - 2s - loss: 0.0013 - acc: 1.0000 - val_loss: 0.2449 - val_acc: 0.9444
    Epoch 87/200
    360/360 [==============================] - 2s - loss: 0.0013 - acc: 1.0000 - val_loss: 0.2469 - val_acc: 0.9444
    Epoch 88/200
    360/360 [==============================] - 2s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.2449 - val_acc: 0.9444
    Epoch 89/200
    360/360 [==============================] - 2s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.2441 - val_acc: 0.9444
    Epoch 90/200
    360/360 [==============================] - 3s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.2441 - val_acc: 0.9444
    Epoch 91/200
    360/360 [==============================] - 2s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.2452 - val_acc: 0.9444
    Epoch 92/200
    360/360 [==============================] - 2s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.2476 - val_acc: 0.9444
    Epoch 93/200
    360/360 [==============================] - 2s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.2482 - val_acc: 0.9444
    Epoch 94/200
    360/360 [==============================] - 2s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.2463 - val_acc: 0.9444
    Epoch 95/200
    360/360 [==============================] - 2s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.2481 - val_acc: 0.9444
    Epoch 96/200
    360/360 [==============================] - 2s - loss: 0.0010 - acc: 1.0000 - val_loss: 0.2485 - val_acc: 0.9444
    Epoch 97/200
    360/360 [==============================] - 2s - loss: 0.0010 - acc: 1.0000 - val_loss: 0.2481 - val_acc: 0.9444
    Epoch 98/200
    360/360 [==============================] - 2s - loss: 0.0010 - acc: 1.0000 - val_loss: 0.2477 - val_acc: 0.9444
    Epoch 99/200
    360/360 [==============================] - 2s - loss: 9.8657e-04 - acc: 1.0000 - val_loss: 0.2488 - val_acc: 0.9444
    Epoch 100/200
    360/360 [==============================] - 2s - loss: 9.6770e-04 - acc: 1.0000 - val_loss: 0.2491 - val_acc: 0.9444
    Epoch 101/200
    360/360 [==============================] - 2s - loss: 9.4242e-04 - acc: 1.0000 - val_loss: 0.2491 - val_acc: 0.9444
    Epoch 102/200
    360/360 [==============================] - 2s - loss: 9.2863e-04 - acc: 1.0000 - val_loss: 0.2491 - val_acc: 0.9444
    Epoch 103/200
    360/360 [==============================] - 2s - loss: 9.1705e-04 - acc: 1.0000 - val_loss: 0.2500 - val_acc: 0.9444
    Epoch 104/200
    360/360 [==============================] - 2s - loss: 8.9716e-04 - acc: 1.0000 - val_loss: 0.2487 - val_acc: 0.9444
    Epoch 105/200
    360/360 [==============================] - 2s - loss: 8.8229e-04 - acc: 1.0000 - val_loss: 0.2498 - val_acc: 0.9444
    Epoch 106/200
    360/360 [==============================] - 2s - loss: 8.6837e-04 - acc: 1.0000 - val_loss: 0.2513 - val_acc: 0.9444
    Epoch 107/200
    360/360 [==============================] - 2s - loss: 8.4940e-04 - acc: 1.0000 - val_loss: 0.2520 - val_acc: 0.9444
    Epoch 108/200
    360/360 [==============================] - 2s - loss: 8.3558e-04 - acc: 1.0000 - val_loss: 0.2523 - val_acc: 0.9444
    Epoch 109/200
    360/360 [==============================] - 2s - loss: 8.1715e-04 - acc: 1.0000 - val_loss: 0.2512 - val_acc: 0.9444
    Epoch 110/200
    360/360 [==============================] - 2s - loss: 8.0449e-04 - acc: 1.0000 - val_loss: 0.2507 - val_acc: 0.9444
    Epoch 111/200
    360/360 [==============================] - 2s - loss: 7.9074e-04 - acc: 1.0000 - val_loss: 0.2516 - val_acc: 0.9444
    Epoch 112/200
    360/360 [==============================] - 2s - loss: 7.7602e-04 - acc: 1.0000 - val_loss: 0.2526 - val_acc: 0.9444
    Epoch 113/200
    360/360 [==============================] - 2s - loss: 7.6337e-04 - acc: 1.0000 - val_loss: 0.2529 - val_acc: 0.9444
    Epoch 114/200
    360/360 [==============================] - 2s - loss: 7.5072e-04 - acc: 1.0000 - val_loss: 0.2534 - val_acc: 0.9444
    Epoch 115/200
    360/360 [==============================] - 2s - loss: 7.3934e-04 - acc: 1.0000 - val_loss: 0.2532 - val_acc: 0.9444
    Epoch 116/200
    360/360 [==============================] - 2s - loss: 7.2741e-04 - acc: 1.0000 - val_loss: 0.2536 - val_acc: 0.9444
    Epoch 117/200
    360/360 [==============================] - 2s - loss: 7.1569e-04 - acc: 1.0000 - val_loss: 0.2532 - val_acc: 0.9444
    Epoch 118/200
    360/360 [==============================] - 2s - loss: 7.0323e-04 - acc: 1.0000 - val_loss: 0.2539 - val_acc: 0.9444
    Epoch 119/200
    360/360 [==============================] - 2s - loss: 6.9245e-04 - acc: 1.0000 - val_loss: 0.2538 - val_acc: 0.9444
    Epoch 120/200
    360/360 [==============================] - 2s - loss: 6.8110e-04 - acc: 1.0000 - val_loss: 0.2528 - val_acc: 0.9444
    Epoch 121/200
    360/360 [==============================] - 2s - loss: 6.7066e-04 - acc: 1.0000 - val_loss: 0.2535 - val_acc: 0.9444
    Epoch 122/200
    360/360 [==============================] - 2s - loss: 6.6083e-04 - acc: 1.0000 - val_loss: 0.2550 - val_acc: 0.9444
    Epoch 123/200
    360/360 [==============================] - 2s - loss: 6.4996e-04 - acc: 1.0000 - val_loss: 0.2557 - val_acc: 0.9444
    Epoch 124/200
    360/360 [==============================] - 2s - loss: 6.4255e-04 - acc: 1.0000 - val_loss: 0.2553 - val_acc: 0.9444
    Epoch 125/200
    360/360 [==============================] - 2s - loss: 6.3170e-04 - acc: 1.0000 - val_loss: 0.2552 - val_acc: 0.9444
    Epoch 126/200
    360/360 [==============================] - 2s - loss: 6.2259e-04 - acc: 1.0000 - val_loss: 0.2553 - val_acc: 0.9444
    Epoch 127/200
    360/360 [==============================] - 2s - loss: 6.1229e-04 - acc: 1.0000 - val_loss: 0.2559 - val_acc: 0.9444
    Epoch 128/200
    360/360 [==============================] - 2s - loss: 6.0260e-04 - acc: 1.0000 - val_loss: 0.2555 - val_acc: 0.9444
    Epoch 129/200
    360/360 [==============================] - 2s - loss: 5.9392e-04 - acc: 1.0000 - val_loss: 0.2565 - val_acc: 0.9444
    Epoch 130/200
    360/360 [==============================] - 2s - loss: 5.8615e-04 - acc: 1.0000 - val_loss: 0.2567 - val_acc: 0.9444
    Epoch 131/200
    360/360 [==============================] - 2s - loss: 5.7752e-04 - acc: 1.0000 - val_loss: 0.2574 - val_acc: 0.9444
    Epoch 132/200
    360/360 [==============================] - 2s - loss: 5.6949e-04 - acc: 1.0000 - val_loss: 0.2571 - val_acc: 0.9444
    Epoch 133/200
    360/360 [==============================] - 2s - loss: 5.6139e-04 - acc: 1.0000 - val_loss: 0.2572 - val_acc: 0.9444
    Epoch 134/200
    360/360 [==============================] - 2s - loss: 5.5201e-04 - acc: 1.0000 - val_loss: 0.2571 - val_acc: 0.9444
    Epoch 135/200
    360/360 [==============================] - 2s - loss: 5.4517e-04 - acc: 1.0000 - val_loss: 0.2563 - val_acc: 0.9444
    Epoch 136/200
    360/360 [==============================] - 2s - loss: 5.3804e-04 - acc: 1.0000 - val_loss: 0.2563 - val_acc: 0.9444
    Epoch 137/200
    360/360 [==============================] - 2s - loss: 5.3143e-04 - acc: 1.0000 - val_loss: 0.2568 - val_acc: 0.9444
    Epoch 138/200
    360/360 [==============================] - 2s - loss: 5.2563e-04 - acc: 1.0000 - val_loss: 0.2594 - val_acc: 0.9444
    Epoch 139/200
    360/360 [==============================] - 2s - loss: 5.1772e-04 - acc: 1.0000 - val_loss: 0.2593 - val_acc: 0.9444
    Epoch 140/200
    360/360 [==============================] - 2s - loss: 5.0978e-04 - acc: 1.0000 - val_loss: 0.2593 - val_acc: 0.9444
    Epoch 141/200
    360/360 [==============================] - 2s - loss: 5.0318e-04 - acc: 1.0000 - val_loss: 0.2588 - val_acc: 0.9444
    Epoch 142/200
    360/360 [==============================] - 2s - loss: 4.9566e-04 - acc: 1.0000 - val_loss: 0.2581 - val_acc: 0.9444
    Epoch 143/200
    360/360 [==============================] - 2s - loss: 4.9009e-04 - acc: 1.0000 - val_loss: 0.2579 - val_acc: 0.9444
    Epoch 144/200
    360/360 [==============================] - 2s - loss: 4.8343e-04 - acc: 1.0000 - val_loss: 0.2584 - val_acc: 0.9444
    Epoch 145/200
    360/360 [==============================] - 2s - loss: 4.7699e-04 - acc: 1.0000 - val_loss: 0.2595 - val_acc: 0.9444
    Epoch 146/200
    360/360 [==============================] - 2s - loss: 4.7133e-04 - acc: 1.0000 - val_loss: 0.2601 - val_acc: 0.9444
    Epoch 147/200
    360/360 [==============================] - 2s - loss: 4.6397e-04 - acc: 1.0000 - val_loss: 0.2599 - val_acc: 0.9444
    Epoch 148/200
    360/360 [==============================] - 2s - loss: 4.5807e-04 - acc: 1.0000 - val_loss: 0.2597 - val_acc: 0.9444
    Epoch 149/200
    360/360 [==============================] - 2s - loss: 4.5238e-04 - acc: 1.0000 - val_loss: 0.2597 - val_acc: 0.9444
    Epoch 150/200
    360/360 [==============================] - 2s - loss: 4.4700e-04 - acc: 1.0000 - val_loss: 0.2601 - val_acc: 0.9444
    Epoch 151/200
    360/360 [==============================] - 2s - loss: 4.4124e-04 - acc: 1.0000 - val_loss: 0.2613 - val_acc: 0.9444
    Epoch 152/200
    360/360 [==============================] - 2s - loss: 4.3707e-04 - acc: 1.0000 - val_loss: 0.2615 - val_acc: 0.9444
    Epoch 153/200
    360/360 [==============================] - 2s - loss: 4.3089e-04 - acc: 1.0000 - val_loss: 0.2625 - val_acc: 0.9444
    Epoch 154/200
    360/360 [==============================] - 2s - loss: 4.2526e-04 - acc: 1.0000 - val_loss: 0.2620 - val_acc: 0.9444
    Epoch 155/200
    360/360 [==============================] - 2s - loss: 4.2036e-04 - acc: 1.0000 - val_loss: 0.2614 - val_acc: 0.9444
    Epoch 156/200
    360/360 [==============================] - 2s - loss: 4.1478e-04 - acc: 1.0000 - val_loss: 0.2611 - val_acc: 0.9444
    Epoch 157/200
    360/360 [==============================] - 2s - loss: 4.0983e-04 - acc: 1.0000 - val_loss: 0.2607 - val_acc: 0.9444
    Epoch 158/200
    360/360 [==============================] - 2s - loss: 4.0459e-04 - acc: 1.0000 - val_loss: 0.2617 - val_acc: 0.9444
    Epoch 159/200
    360/360 [==============================] - 2s - loss: 3.9978e-04 - acc: 1.0000 - val_loss: 0.2624 - val_acc: 0.9444
    Epoch 160/200
    360/360 [==============================] - 2s - loss: 3.9527e-04 - acc: 1.0000 - val_loss: 0.2628 - val_acc: 0.9444
    Epoch 161/200
    360/360 [==============================] - 2s - loss: 3.8989e-04 - acc: 1.0000 - val_loss: 0.2634 - val_acc: 0.9444
    Epoch 162/200
    360/360 [==============================] - 2s - loss: 3.8613e-04 - acc: 1.0000 - val_loss: 0.2632 - val_acc: 0.9444
    Epoch 163/200
    360/360 [==============================] - 2s - loss: 3.8139e-04 - acc: 1.0000 - val_loss: 0.2634 - val_acc: 0.9444
    Epoch 164/200
    360/360 [==============================] - 2s - loss: 3.7764e-04 - acc: 1.0000 - val_loss: 0.2637 - val_acc: 0.9444
    Epoch 165/200
    360/360 [==============================] - 2s - loss: 3.7256e-04 - acc: 1.0000 - val_loss: 0.2638 - val_acc: 0.9444
    Epoch 166/200
    360/360 [==============================] - 2s - loss: 3.6913e-04 - acc: 1.0000 - val_loss: 0.2643 - val_acc: 0.9444
    Epoch 167/200
    360/360 [==============================] - 2s - loss: 3.6423e-04 - acc: 1.0000 - val_loss: 0.2656 - val_acc: 0.9444
    Epoch 168/200
    360/360 [==============================] - 2s - loss: 3.6111e-04 - acc: 1.0000 - val_loss: 0.2658 - val_acc: 0.9444
    Epoch 169/200
    360/360 [==============================] - 2s - loss: 3.5655e-04 - acc: 1.0000 - val_loss: 0.2649 - val_acc: 0.9444
    Epoch 170/200
    360/360 [==============================] - 2s - loss: 3.5221e-04 - acc: 1.0000 - val_loss: 0.2638 - val_acc: 0.9444
    Epoch 171/200
    360/360 [==============================] - 2s - loss: 3.4803e-04 - acc: 1.0000 - val_loss: 0.2638 - val_acc: 0.9444
    Epoch 172/200
    360/360 [==============================] - 2s - loss: 3.4458e-04 - acc: 1.0000 - val_loss: 0.2640 - val_acc: 0.9444
    Epoch 173/200
    360/360 [==============================] - 2s - loss: 3.4091e-04 - acc: 1.0000 - val_loss: 0.2647 - val_acc: 0.9444
    Epoch 174/200
    360/360 [==============================] - 2s - loss: 3.3629e-04 - acc: 1.0000 - val_loss: 0.2656 - val_acc: 0.9444
    Epoch 175/200
    360/360 [==============================] - 2s - loss: 3.3250e-04 - acc: 1.0000 - val_loss: 0.2659 - val_acc: 0.9444
    Epoch 176/200
    360/360 [==============================] - 2s - loss: 3.2902e-04 - acc: 1.0000 - val_loss: 0.2660 - val_acc: 0.9444
    Epoch 177/200
    360/360 [==============================] - 2s - loss: 3.2587e-04 - acc: 1.0000 - val_loss: 0.2657 - val_acc: 0.9444
    Epoch 178/200
    360/360 [==============================] - 2s - loss: 3.2240e-04 - acc: 1.0000 - val_loss: 0.2661 - val_acc: 0.9444
    Epoch 179/200
    360/360 [==============================] - 2s - loss: 3.1887e-04 - acc: 1.0000 - val_loss: 0.2654 - val_acc: 0.9444
    Epoch 180/200
    360/360 [==============================] - 2s - loss: 3.1541e-04 - acc: 1.0000 - val_loss: 0.2658 - val_acc: 0.9444
    Epoch 181/200
    360/360 [==============================] - 2s - loss: 3.1213e-04 - acc: 1.0000 - val_loss: 0.2653 - val_acc: 0.9444
    Epoch 182/200
    360/360 [==============================] - 2s - loss: 3.0908e-04 - acc: 1.0000 - val_loss: 0.2663 - val_acc: 0.9444
    Epoch 183/200
    360/360 [==============================] - 2s - loss: 3.0566e-04 - acc: 1.0000 - val_loss: 0.2667 - val_acc: 0.9444
    Epoch 184/200
    360/360 [==============================] - 2s - loss: 3.0225e-04 - acc: 1.0000 - val_loss: 0.2665 - val_acc: 0.9444
    Epoch 185/200
    360/360 [==============================] - 2s - loss: 2.9998e-04 - acc: 1.0000 - val_loss: 0.2660 - val_acc: 0.9444
    Epoch 186/200
    360/360 [==============================] - 2s - loss: 2.9648e-04 - acc: 1.0000 - val_loss: 0.2664 - val_acc: 0.9444
    Epoch 187/200
    360/360 [==============================] - 2s - loss: 2.9289e-04 - acc: 1.0000 - val_loss: 0.2665 - val_acc: 0.9444
    Epoch 188/200
    360/360 [==============================] - 2s - loss: 2.9004e-04 - acc: 1.0000 - val_loss: 0.2670 - val_acc: 0.9444
    Epoch 189/200
    360/360 [==============================] - 2s - loss: 2.8689e-04 - acc: 1.0000 - val_loss: 0.2671 - val_acc: 0.9444
    Epoch 190/200
    360/360 [==============================] - 2s - loss: 2.8417e-04 - acc: 1.0000 - val_loss: 0.2668 - val_acc: 0.9444
    Epoch 191/200
    360/360 [==============================] - 2s - loss: 2.8153e-04 - acc: 1.0000 - val_loss: 0.2671 - val_acc: 0.9444
    Epoch 192/200
    360/360 [==============================] - 3s - loss: 2.7888e-04 - acc: 1.0000 - val_loss: 0.2669 - val_acc: 0.9444
    Epoch 193/200
    360/360 [==============================] - 3s - loss: 2.7652e-04 - acc: 1.0000 - val_loss: 0.2671 - val_acc: 0.9444
    Epoch 194/200
    360/360 [==============================] - 3s - loss: 2.7332e-04 - acc: 1.0000 - val_loss: 0.2678 - val_acc: 0.9444
    Epoch 195/200
    360/360 [==============================] - 3s - loss: 2.7029e-04 - acc: 1.0000 - val_loss: 0.2683 - val_acc: 0.9444
    Epoch 196/200
    360/360 [==============================] - 2s - loss: 2.6789e-04 - acc: 1.0000 - val_loss: 0.2687 - val_acc: 0.9444
    Epoch 197/200
    360/360 [==============================] - 2s - loss: 2.6522e-04 - acc: 1.0000 - val_loss: 0.2690 - val_acc: 0.9444
    Epoch 198/200
    360/360 [==============================] - 2s - loss: 2.6279e-04 - acc: 1.0000 - val_loss: 0.2694 - val_acc: 0.9444
    Epoch 199/200
    360/360 [==============================] - 2s - loss: 2.6065e-04 - acc: 1.0000 - val_loss: 0.2682 - val_acc: 0.9444
    Epoch 200/200
    360/360 [==============================] - 2s - loss: 2.5772e-04 - acc: 1.0000 - val_loss: 0.2678 - val_acc: 0.9444
    


```python
score = model.evaluate(testData, testLabels, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

    50/50 [==============================] - 0s     
    
    Test score: 0.666664040089
    Test accuracy: 0.899999990463
    


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
    


![_config.yml]({{ site.baseurl}}/images/facerecog_output_33_1.png)



```python
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/facerecog_output_34_0.png)



```python
model_json = model.to_json()
with open("model.json", "w") as json_file:
    json_file.write(model_json)
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 7252-C405
    
     Directory of C:\Users\cobalt\workspace\FaceRecognition
    
    09/24/2017  05:19 PM    <DIR>          .
    09/24/2017  05:19 PM    <DIR>          ..
    09/24/2017  05:19 PM    <DIR>          .ipynb_checkpoints
    09/24/2017  05:19 PM         1,125,355 FaceRecognitionLeNet.ipynb
    09/24/2017  05:19 PM         1,125,355 FaceRecognitionLeNetv2.ipynb
    09/24/2017  09:59 AM    <DIR>          front_database
    09/24/2017  05:00 PM         5,049,496 model.h5
    09/24/2017  05:19 PM             3,146 model.json
    09/24/2017  03:32 PM         1,134,255 ReadMultipleImages.ipynb
    04/13/2016  11:59 AM         4,373,288 Revised Final Thesis Draft.pdf
    09/24/2017  10:00 AM    <DIR>          side_database
    09/24/2017  10:00 AM    <DIR>          videos
                   6 File(s)     12,810,895 bytes
                   6 Dir(s)  199,579,463,680 bytes free
    


```python
model.save_weights("model.h5")
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 7252-C405
    
     Directory of C:\Users\cobalt\workspace\FaceRecognition
    
    09/24/2017  05:19 PM    <DIR>          .
    09/24/2017  05:19 PM    <DIR>          ..
    09/24/2017  05:19 PM    <DIR>          .ipynb_checkpoints
    09/24/2017  05:19 PM         1,125,355 FaceRecognitionLeNet.ipynb
    09/24/2017  05:19 PM         1,125,355 FaceRecognitionLeNetv2.ipynb
    09/24/2017  09:59 AM    <DIR>          front_database
    09/24/2017  05:19 PM         5,049,496 model.h5
    09/24/2017  05:19 PM             3,146 model.json
    09/24/2017  03:32 PM         1,134,255 ReadMultipleImages.ipynb
    04/13/2016  11:59 AM         4,373,288 Revised Final Thesis Draft.pdf
    09/24/2017  10:00 AM    <DIR>          side_database
    09/24/2017  10:00 AM    <DIR>          videos
                   6 File(s)     12,810,895 bytes
                   6 Dir(s)  199,579,463,680 bytes free
    


```python
json_file = open('model.json', 'r')
loaded_model_json = json_file.read()
json_file.close()
loaded_model = model_from_json(loaded_model_json)
loaded_model.load_weights("model.h5")
```


```python
loaded_model.compile(loss='categorical_crossentropy',
              optimizer=OPTIMIZER,
              metrics=['accuracy'])
score = loaded_model.evaluate(testData, testLabels, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

    50/50 [==============================] - 0s     
    
    Test score: 0.666664040089
    Test accuracy: 0.899999990463
    
-mkc