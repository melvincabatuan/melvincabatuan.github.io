
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

IMG_ROWS, IMG_COLS = 50, 50 
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
    name_map = {}
    for folder in folders:        
        for filename in os.listdir(os.path.join(root_dir,folder)):
            if any([filename.endswith(x) for x in ['.jpeg', '.jpg','.pgm','png']]):
                img = cv2.imread(os.path.join(root_dir, folder, filename), cv2.IMREAD_GRAYSCALE)
                if img is not None: 
                    image = np.array(img, 'uint8') # convert to numpy array
                    images.append(image)
                    label = os.path.split(folder)[1].split("_")[1] # number from person_0_
                    labels.append(label)
                    name = os.path.split(folder)[1].split("_")[2]  
                    name_map[label] = name
    return images, labels, name_map;
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

#name_map = {}

(images, labels, name_map) = load_images_from_folders(folders, root_dir)
print('No. of images = %s. ' %  len(images))
print('No. of labels = %s. ' %  len(labels))
print(labels[0])
print('name_map[0] = %s. ' %  name_map["0"])
print('name_map[1] = %s. ' %  name_map["1"])
print('name_map[2] = %s. ' %  name_map["2"])

print('name_map[9] = %s. ' %  name_map["9"])
```

    Acquiring images...
    No. of images = 500. 
    No. of labels = 500. 
    0
    name_map[0] = abad. 
    name_map[1] = agui. 
    name_map[2] = cans. 
    name_map[9] = venu. 
    

Step 4: Plot the first image.


```python
plt.imshow(images[0], cmap='gray')
```




    <matplotlib.image.AxesImage at 0x2c00cb79780>




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

    (450, 1, 50, 50) train samples
    (50, 1, 50, 50) test samples
    


```python
model = LeNet.build(input_shape=INPUT_SHAPE, classes=NB_CLASSES)
model.summary()
```

    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv2d_1 (Conv2D)            (None, 20, 50, 50)        520       
    _________________________________________________________________
    activation_1 (Activation)    (None, 20, 50, 50)        0         
    _________________________________________________________________
    max_pooling2d_1 (MaxPooling2 (None, 20, 25, 25)        0         
    _________________________________________________________________
    conv2d_2 (Conv2D)            (None, 50, 25, 25)        25050     
    _________________________________________________________________
    activation_2 (Activation)    (None, 50, 25, 25)        0         
    _________________________________________________________________
    max_pooling2d_2 (MaxPooling2 (None, 50, 12, 12)        0         
    _________________________________________________________________
    flatten_1 (Flatten)          (None, 7200)              0         
    _________________________________________________________________
    dense_1 (Dense)              (None, 500)               3600500   
    _________________________________________________________________
    activation_3 (Activation)    (None, 500)               0         
    _________________________________________________________________
    dense_2 (Dense)              (None, 10)                5010      
    _________________________________________________________________
    activation_4 (Activation)    (None, 10)                0         
    =================================================================
    Total params: 3,631,080
    Trainable params: 3,631,080
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
    360/360 [==============================] - 8s - loss: 2.4070 - acc: 0.1167 - val_loss: 2.1390 - val_acc: 0.3889
    Epoch 2/200
    360/360 [==============================] - 8s - loss: 2.0961 - acc: 0.3306 - val_loss: 1.9491 - val_acc: 0.3222
    Epoch 3/200
    360/360 [==============================] - 8s - loss: 1.7818 - acc: 0.4056 - val_loss: 1.5161 - val_acc: 0.6222
    Epoch 4/200
    360/360 [==============================] - 8s - loss: 1.3499 - acc: 0.5639 - val_loss: 1.2057 - val_acc: 0.6333
    Epoch 5/200
    360/360 [==============================] - 8s - loss: 1.0729 - acc: 0.6667 - val_loss: 1.0724 - val_acc: 0.6333
    Epoch 6/200
    360/360 [==============================] - 8s - loss: 0.9374 - acc: 0.6667 - val_loss: 0.9330 - val_acc: 0.6778
    Epoch 7/200
    360/360 [==============================] - 8s - loss: 0.7170 - acc: 0.7861 - val_loss: 0.7422 - val_acc: 0.7889
    Epoch 8/200
    360/360 [==============================] - 8s - loss: 0.4811 - acc: 0.9028 - val_loss: 0.5487 - val_acc: 0.8778
    Epoch 9/200
    360/360 [==============================] - 8s - loss: 0.3400 - acc: 0.9306 - val_loss: 0.4450 - val_acc: 0.8778
    Epoch 10/200
    360/360 [==============================] - 8s - loss: 0.2421 - acc: 0.9500 - val_loss: 0.4107 - val_acc: 0.9111
    Epoch 11/200
    360/360 [==============================] - 8s - loss: 0.1781 - acc: 0.9694 - val_loss: 0.3180 - val_acc: 0.9222
    Epoch 12/200
    360/360 [==============================] - 7s - loss: 0.1243 - acc: 0.9750 - val_loss: 0.2701 - val_acc: 0.9222
    Epoch 13/200
    360/360 [==============================] - 7s - loss: 0.0964 - acc: 0.9722 - val_loss: 0.2748 - val_acc: 0.9111
    Epoch 14/200
    360/360 [==============================] - 8s - loss: 0.0688 - acc: 0.9833 - val_loss: 0.3038 - val_acc: 0.9222
    Epoch 15/200
    360/360 [==============================] - 8s - loss: 0.0524 - acc: 0.9972 - val_loss: 0.1687 - val_acc: 0.9667
    Epoch 16/200
    360/360 [==============================] - 8s - loss: 0.0334 - acc: 0.9944 - val_loss: 0.1983 - val_acc: 0.9444
    Epoch 17/200
    360/360 [==============================] - 7s - loss: 0.0223 - acc: 1.0000 - val_loss: 0.3067 - val_acc: 0.9111
    Epoch 18/200
    360/360 [==============================] - 8s - loss: 0.0187 - acc: 1.0000 - val_loss: 0.2062 - val_acc: 0.9556
    Epoch 19/200
    360/360 [==============================] - 7s - loss: 0.0130 - acc: 1.0000 - val_loss: 0.1890 - val_acc: 0.9667
    Epoch 20/200
    360/360 [==============================] - 7s - loss: 0.0103 - acc: 1.0000 - val_loss: 0.1887 - val_acc: 0.9667
    Epoch 21/200
    360/360 [==============================] - 7s - loss: 0.0080 - acc: 1.0000 - val_loss: 0.2066 - val_acc: 0.9556
    Epoch 22/200
    360/360 [==============================] - 7s - loss: 0.0066 - acc: 1.0000 - val_loss: 0.2068 - val_acc: 0.9444
    Epoch 23/200
    360/360 [==============================] - 7s - loss: 0.0045 - acc: 1.0000 - val_loss: 0.1988 - val_acc: 0.9556
    Epoch 24/200
    360/360 [==============================] - 7s - loss: 0.0039 - acc: 1.0000 - val_loss: 0.1994 - val_acc: 0.9556
    Epoch 25/200
    360/360 [==============================] - 7s - loss: 0.0032 - acc: 1.0000 - val_loss: 0.1943 - val_acc: 0.9667
    Epoch 26/200
    360/360 [==============================] - 7s - loss: 0.0024 - acc: 1.0000 - val_loss: 0.1938 - val_acc: 0.9667
    Epoch 27/200
    360/360 [==============================] - 7s - loss: 0.0022 - acc: 1.0000 - val_loss: 0.1941 - val_acc: 0.9556
    Epoch 28/200
    360/360 [==============================] - 7s - loss: 0.0021 - acc: 1.0000 - val_loss: 0.1934 - val_acc: 0.9667
    Epoch 29/200
    360/360 [==============================] - 7s - loss: 0.0018 - acc: 1.0000 - val_loss: 0.1947 - val_acc: 0.9667
    Epoch 30/200
    360/360 [==============================] - 7s - loss: 0.0015 - acc: 1.0000 - val_loss: 0.2002 - val_acc: 0.9667
    Epoch 31/200
    360/360 [==============================] - 7s - loss: 0.0014 - acc: 1.0000 - val_loss: 0.2067 - val_acc: 0.9667
    Epoch 32/200
    360/360 [==============================] - 7s - loss: 0.0013 - acc: 1.0000 - val_loss: 0.2076 - val_acc: 0.9667
    Epoch 33/200
    360/360 [==============================] - 7s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.2038 - val_acc: 0.9667
    Epoch 34/200
    360/360 [==============================] - 8s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.2000 - val_acc: 0.9667
    Epoch 35/200
    360/360 [==============================] - 7s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.1994 - val_acc: 0.9667
    Epoch 36/200
    360/360 [==============================] - 7s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.2007 - val_acc: 0.9667
    Epoch 37/200
    360/360 [==============================] - 7s - loss: 0.0010 - acc: 1.0000 - val_loss: 0.2026 - val_acc: 0.9667
    Epoch 38/200
    360/360 [==============================] - 8s - loss: 9.5371e-04 - acc: 1.0000 - val_loss: 0.2032 - val_acc: 0.9667
    Epoch 39/200
    360/360 [==============================] - 7s - loss: 9.1215e-04 - acc: 1.0000 - val_loss: 0.2040 - val_acc: 0.9667
    Epoch 40/200
    360/360 [==============================] - 7s - loss: 8.7364e-04 - acc: 1.0000 - val_loss: 0.2029 - val_acc: 0.9667
    Epoch 41/200
    360/360 [==============================] - 7s - loss: 8.4320e-04 - acc: 1.0000 - val_loss: 0.2035 - val_acc: 0.9667
    Epoch 42/200
    360/360 [==============================] - 7s - loss: 8.1621e-04 - acc: 1.0000 - val_loss: 0.2056 - val_acc: 0.9667
    Epoch 43/200
    360/360 [==============================] - 7s - loss: 7.8616e-04 - acc: 1.0000 - val_loss: 0.2070 - val_acc: 0.9667
    Epoch 44/200
    360/360 [==============================] - 7s - loss: 7.6045e-04 - acc: 1.0000 - val_loss: 0.2080 - val_acc: 0.9667
    Epoch 45/200
    360/360 [==============================] - 7s - loss: 7.3207e-04 - acc: 1.0000 - val_loss: 0.2089 - val_acc: 0.9667
    Epoch 46/200
    360/360 [==============================] - 7s - loss: 7.0840e-04 - acc: 1.0000 - val_loss: 0.2087 - val_acc: 0.9667
    Epoch 47/200
    360/360 [==============================] - 8s - loss: 6.8675e-04 - acc: 1.0000 - val_loss: 0.2080 - val_acc: 0.9667
    Epoch 48/200
    360/360 [==============================] - 7s - loss: 6.6653e-04 - acc: 1.0000 - val_loss: 0.2076 - val_acc: 0.9667
    Epoch 49/200
    360/360 [==============================] - 7s - loss: 6.4524e-04 - acc: 1.0000 - val_loss: 0.2078 - val_acc: 0.9667
    Epoch 50/200
    360/360 [==============================] - 7s - loss: 6.2556e-04 - acc: 1.0000 - val_loss: 0.2084 - val_acc: 0.9667
    Epoch 51/200
    360/360 [==============================] - 7s - loss: 6.0697e-04 - acc: 1.0000 - val_loss: 0.2091 - val_acc: 0.9667
    Epoch 52/200
    360/360 [==============================] - 7s - loss: 5.9115e-04 - acc: 1.0000 - val_loss: 0.2100 - val_acc: 0.9667
    Epoch 53/200
    360/360 [==============================] - 7s - loss: 5.7355e-04 - acc: 1.0000 - val_loss: 0.2114 - val_acc: 0.9667
    Epoch 54/200
    360/360 [==============================] - 7s - loss: 5.5731e-04 - acc: 1.0000 - val_loss: 0.2112 - val_acc: 0.9667
    Epoch 55/200
    360/360 [==============================] - 7s - loss: 5.4126e-04 - acc: 1.0000 - val_loss: 0.2111 - val_acc: 0.9667
    Epoch 56/200
    360/360 [==============================] - 7s - loss: 5.2434e-04 - acc: 1.0000 - val_loss: 0.2115 - val_acc: 0.9667
    Epoch 57/200
    360/360 [==============================] - 7s - loss: 5.1054e-04 - acc: 1.0000 - val_loss: 0.2122 - val_acc: 0.9667
    Epoch 58/200
    360/360 [==============================] - 7s - loss: 4.9876e-04 - acc: 1.0000 - val_loss: 0.2135 - val_acc: 0.9667
    Epoch 59/200
    360/360 [==============================] - 7s - loss: 4.8343e-04 - acc: 1.0000 - val_loss: 0.2126 - val_acc: 0.9667
    Epoch 60/200
    360/360 [==============================] - 7s - loss: 4.7040e-04 - acc: 1.0000 - val_loss: 0.2124 - val_acc: 0.9667
    Epoch 61/200
    360/360 [==============================] - 7s - loss: 4.5996e-04 - acc: 1.0000 - val_loss: 0.2119 - val_acc: 0.9667
    Epoch 62/200
    360/360 [==============================] - 7s - loss: 4.4633e-04 - acc: 1.0000 - val_loss: 0.2126 - val_acc: 0.9667
    Epoch 63/200
    360/360 [==============================] - 7s - loss: 4.3571e-04 - acc: 1.0000 - val_loss: 0.2131 - val_acc: 0.9667
    Epoch 64/200
    360/360 [==============================] - 7s - loss: 4.2367e-04 - acc: 1.0000 - val_loss: 0.2150 - val_acc: 0.9667
    Epoch 65/200
    360/360 [==============================] - 7s - loss: 4.1303e-04 - acc: 1.0000 - val_loss: 0.2169 - val_acc: 0.9667
    Epoch 66/200
    360/360 [==============================] - 7s - loss: 4.0285e-04 - acc: 1.0000 - val_loss: 0.2169 - val_acc: 0.9667
    Epoch 67/200
    360/360 [==============================] - 7s - loss: 3.9423e-04 - acc: 1.0000 - val_loss: 0.2167 - val_acc: 0.9667
    Epoch 68/200
    360/360 [==============================] - 7s - loss: 3.8320e-04 - acc: 1.0000 - val_loss: 0.2160 - val_acc: 0.9667
    Epoch 69/200
    360/360 [==============================] - 7s - loss: 3.7319e-04 - acc: 1.0000 - val_loss: 0.2152 - val_acc: 0.9667
    Epoch 70/200
    360/360 [==============================] - 7s - loss: 3.6435e-04 - acc: 1.0000 - val_loss: 0.2150 - val_acc: 0.9667
    Epoch 71/200
    360/360 [==============================] - 7s - loss: 3.5503e-04 - acc: 1.0000 - val_loss: 0.2157 - val_acc: 0.9667
    Epoch 72/200
    360/360 [==============================] - 7s - loss: 3.4750e-04 - acc: 1.0000 - val_loss: 0.2166 - val_acc: 0.9667
    Epoch 73/200
    360/360 [==============================] - 7s - loss: 3.3860e-04 - acc: 1.0000 - val_loss: 0.2175 - val_acc: 0.9667
    Epoch 74/200
    360/360 [==============================] - 7s - loss: 3.3144e-04 - acc: 1.0000 - val_loss: 0.2189 - val_acc: 0.9667
    Epoch 75/200
    360/360 [==============================] - 7s - loss: 3.2312e-04 - acc: 1.0000 - val_loss: 0.2188 - val_acc: 0.9667
    Epoch 76/200
    360/360 [==============================] - 7s - loss: 3.1619e-04 - acc: 1.0000 - val_loss: 0.2193 - val_acc: 0.9667
    Epoch 77/200
    360/360 [==============================] - 7s - loss: 3.0944e-04 - acc: 1.0000 - val_loss: 0.2192 - val_acc: 0.9667
    Epoch 78/200
    360/360 [==============================] - 7s - loss: 3.0119e-04 - acc: 1.0000 - val_loss: 0.2189 - val_acc: 0.9667
    Epoch 79/200
    360/360 [==============================] - 7s - loss: 2.9527e-04 - acc: 1.0000 - val_loss: 0.2180 - val_acc: 0.9667
    Epoch 80/200
    360/360 [==============================] - 7s - loss: 2.8876e-04 - acc: 1.0000 - val_loss: 0.2186 - val_acc: 0.9667
    Epoch 81/200
    360/360 [==============================] - 7s - loss: 2.8342e-04 - acc: 1.0000 - val_loss: 0.2192 - val_acc: 0.9667
    Epoch 82/200
    360/360 [==============================] - 7s - loss: 2.7560e-04 - acc: 1.0000 - val_loss: 0.2208 - val_acc: 0.9667
    Epoch 83/200
    360/360 [==============================] - 7s - loss: 2.6969e-04 - acc: 1.0000 - val_loss: 0.2223 - val_acc: 0.9667
    Epoch 84/200
    360/360 [==============================] - 7s - loss: 2.6367e-04 - acc: 1.0000 - val_loss: 0.2235 - val_acc: 0.9667
    Epoch 85/200
    360/360 [==============================] - 7s - loss: 2.5889e-04 - acc: 1.0000 - val_loss: 0.2240 - val_acc: 0.9667
    Epoch 86/200
    360/360 [==============================] - 7s - loss: 2.5309e-04 - acc: 1.0000 - val_loss: 0.2239 - val_acc: 0.9667
    Epoch 87/200
    360/360 [==============================] - 8s - loss: 2.4783e-04 - acc: 1.0000 - val_loss: 0.2236 - val_acc: 0.9667
    Epoch 88/200
    360/360 [==============================] - 8s - loss: 2.4249e-04 - acc: 1.0000 - val_loss: 0.2223 - val_acc: 0.9667
    Epoch 89/200
    360/360 [==============================] - 7s - loss: 2.3732e-04 - acc: 1.0000 - val_loss: 0.2218 - val_acc: 0.9667
    Epoch 90/200
    360/360 [==============================] - 7s - loss: 2.3250e-04 - acc: 1.0000 - val_loss: 0.2222 - val_acc: 0.9667
    Epoch 91/200
    360/360 [==============================] - 7s - loss: 2.2739e-04 - acc: 1.0000 - val_loss: 0.2229 - val_acc: 0.9667
    Epoch 92/200
    360/360 [==============================] - 7s - loss: 2.2381e-04 - acc: 1.0000 - val_loss: 0.2247 - val_acc: 0.9667
    Epoch 93/200
    360/360 [==============================] - 7s - loss: 2.1883e-04 - acc: 1.0000 - val_loss: 0.2252 - val_acc: 0.9667
    Epoch 94/200
    360/360 [==============================] - 7s - loss: 2.1439e-04 - acc: 1.0000 - val_loss: 0.2249 - val_acc: 0.9667
    Epoch 95/200
    360/360 [==============================] - 7s - loss: 2.1016e-04 - acc: 1.0000 - val_loss: 0.2253 - val_acc: 0.9667
    Epoch 96/200
    360/360 [==============================] - 7s - loss: 2.0592e-04 - acc: 1.0000 - val_loss: 0.2258 - val_acc: 0.9667
    Epoch 97/200
    360/360 [==============================] - 7s - loss: 2.0170e-04 - acc: 1.0000 - val_loss: 0.2264 - val_acc: 0.9667
    Epoch 98/200
    360/360 [==============================] - 8s - loss: 1.9792e-04 - acc: 1.0000 - val_loss: 0.2267 - val_acc: 0.9667
    Epoch 99/200
    360/360 [==============================] - 7s - loss: 1.9421e-04 - acc: 1.0000 - val_loss: 0.2272 - val_acc: 0.9667
    Epoch 100/200
    360/360 [==============================] - 8s - loss: 1.9055e-04 - acc: 1.0000 - val_loss: 0.2275 - val_acc: 0.9667
    Epoch 101/200
    360/360 [==============================] - 7s - loss: 1.8659e-04 - acc: 1.0000 - val_loss: 0.2268 - val_acc: 0.9667
    Epoch 102/200
    360/360 [==============================] - 7s - loss: 1.8328e-04 - acc: 1.0000 - val_loss: 0.2268 - val_acc: 0.9667
    Epoch 103/200
    360/360 [==============================] - 7s - loss: 1.8001e-04 - acc: 1.0000 - val_loss: 0.2272 - val_acc: 0.9667
    Epoch 104/200
    360/360 [==============================] - 8s - loss: 1.7641e-04 - acc: 1.0000 - val_loss: 0.2273 - val_acc: 0.9667
    Epoch 105/200
    360/360 [==============================] - 7s - loss: 1.7359e-04 - acc: 1.0000 - val_loss: 0.2285 - val_acc: 0.9667
    Epoch 106/200
    360/360 [==============================] - 8s - loss: 1.7010e-04 - acc: 1.0000 - val_loss: 0.2292 - val_acc: 0.9667
    Epoch 107/200
    360/360 [==============================] - 8s - loss: 1.6675e-04 - acc: 1.0000 - val_loss: 0.2303 - val_acc: 0.9667
    Epoch 108/200
    360/360 [==============================] - 8s - loss: 1.6396e-04 - acc: 1.0000 - val_loss: 0.2312 - val_acc: 0.9667
    Epoch 109/200
    360/360 [==============================] - 8s - loss: 1.6081e-04 - acc: 1.0000 - val_loss: 0.2316 - val_acc: 0.9667
    Epoch 110/200
    360/360 [==============================] - 7s - loss: 1.5816e-04 - acc: 1.0000 - val_loss: 0.2319 - val_acc: 0.9667
    Epoch 111/200
    360/360 [==============================] - 8s - loss: 1.5524e-04 - acc: 1.0000 - val_loss: 0.2316 - val_acc: 0.9667
    Epoch 112/200
    360/360 [==============================] - 7s - loss: 1.5231e-04 - acc: 1.0000 - val_loss: 0.2318 - val_acc: 0.9667
    Epoch 113/200
    360/360 [==============================] - 7s - loss: 1.4957e-04 - acc: 1.0000 - val_loss: 0.2317 - val_acc: 0.9667
    Epoch 114/200
    360/360 [==============================] - 7s - loss: 1.4725e-04 - acc: 1.0000 - val_loss: 0.2319 - val_acc: 0.9667
    Epoch 115/200
    360/360 [==============================] - 7s - loss: 1.4484e-04 - acc: 1.0000 - val_loss: 0.2325 - val_acc: 0.9667
    Epoch 116/200
    360/360 [==============================] - 7s - loss: 1.4215e-04 - acc: 1.0000 - val_loss: 0.2329 - val_acc: 0.9667
    Epoch 117/200
    360/360 [==============================] - 7s - loss: 1.3988e-04 - acc: 1.0000 - val_loss: 0.2331 - val_acc: 0.9667
    Epoch 118/200
    360/360 [==============================] - 7s - loss: 1.3744e-04 - acc: 1.0000 - val_loss: 0.2339 - val_acc: 0.9667
    Epoch 119/200
    360/360 [==============================] - 7s - loss: 1.3502e-04 - acc: 1.0000 - val_loss: 0.2340 - val_acc: 0.9667
    Epoch 120/200
    360/360 [==============================] - 7s - loss: 1.3290e-04 - acc: 1.0000 - val_loss: 0.2336 - val_acc: 0.9667
    Epoch 121/200
    360/360 [==============================] - 7s - loss: 1.3069e-04 - acc: 1.0000 - val_loss: 0.2340 - val_acc: 0.9667
    Epoch 122/200
    360/360 [==============================] - 7s - loss: 1.2859e-04 - acc: 1.0000 - val_loss: 0.2348 - val_acc: 0.9667
    Epoch 123/200
    360/360 [==============================] - 8s - loss: 1.2632e-04 - acc: 1.0000 - val_loss: 0.2354 - val_acc: 0.9667
    Epoch 124/200
    360/360 [==============================] - 8s - loss: 1.2468e-04 - acc: 1.0000 - val_loss: 0.2359 - val_acc: 0.9667
    Epoch 125/200
    360/360 [==============================] - 7s - loss: 1.2245e-04 - acc: 1.0000 - val_loss: 0.2364 - val_acc: 0.9667
    Epoch 126/200
    360/360 [==============================] - 8s - loss: 1.2055e-04 - acc: 1.0000 - val_loss: 0.2368 - val_acc: 0.9667
    Epoch 127/200
    360/360 [==============================] - 7s - loss: 1.1861e-04 - acc: 1.0000 - val_loss: 0.2370 - val_acc: 0.9667
    Epoch 128/200
    360/360 [==============================] - 8s - loss: 1.1670e-04 - acc: 1.0000 - val_loss: 0.2366 - val_acc: 0.9667
    Epoch 129/200
    360/360 [==============================] - 7s - loss: 1.1488e-04 - acc: 1.0000 - val_loss: 0.2371 - val_acc: 0.9667
    Epoch 130/200
    360/360 [==============================] - 8s - loss: 1.1301e-04 - acc: 1.0000 - val_loss: 0.2368 - val_acc: 0.9667
    Epoch 131/200
    360/360 [==============================] - 8s - loss: 1.1122e-04 - acc: 1.0000 - val_loss: 0.2376 - val_acc: 0.9667
    Epoch 132/200
    360/360 [==============================] - 9s - loss: 1.0956e-04 - acc: 1.0000 - val_loss: 0.2384 - val_acc: 0.9667
    Epoch 133/200
    360/360 [==============================] - 7s - loss: 1.0793e-04 - acc: 1.0000 - val_loss: 0.2390 - val_acc: 0.9667
    Epoch 134/200
    360/360 [==============================] - 7s - loss: 1.0620e-04 - acc: 1.0000 - val_loss: 0.2397 - val_acc: 0.9667
    Epoch 135/200
    360/360 [==============================] - 7s - loss: 1.0457e-04 - acc: 1.0000 - val_loss: 0.2392 - val_acc: 0.9667
    Epoch 136/200
    360/360 [==============================] - 7s - loss: 1.0304e-04 - acc: 1.0000 - val_loss: 0.2391 - val_acc: 0.9667
    Epoch 137/200
    360/360 [==============================] - 7s - loss: 1.0157e-04 - acc: 1.0000 - val_loss: 0.2387 - val_acc: 0.9667
    Epoch 138/200
    360/360 [==============================] - 7s - loss: 1.0014e-04 - acc: 1.0000 - val_loss: 0.2396 - val_acc: 0.9667
    Epoch 139/200
    360/360 [==============================] - 7s - loss: 9.8447e-05 - acc: 1.0000 - val_loss: 0.2403 - val_acc: 0.9667
    Epoch 140/200
    360/360 [==============================] - 7s - loss: 9.6999e-05 - acc: 1.0000 - val_loss: 0.2404 - val_acc: 0.9667
    Epoch 141/200
    360/360 [==============================] - 7s - loss: 9.5584e-05 - acc: 1.0000 - val_loss: 0.2410 - val_acc: 0.9667
    Epoch 142/200
    360/360 [==============================] - 7s - loss: 9.4235e-05 - acc: 1.0000 - val_loss: 0.2410 - val_acc: 0.9667
    Epoch 143/200
    360/360 [==============================] - 8s - loss: 9.2884e-05 - acc: 1.0000 - val_loss: 0.2415 - val_acc: 0.9667
    Epoch 144/200
    360/360 [==============================] - 7s - loss: 9.1453e-05 - acc: 1.0000 - val_loss: 0.2417 - val_acc: 0.9667
    Epoch 145/200
    360/360 [==============================] - 8s - loss: 9.0127e-05 - acc: 1.0000 - val_loss: 0.2422 - val_acc: 0.9667
    Epoch 146/200
    360/360 [==============================] - 7s - loss: 8.8948e-05 - acc: 1.0000 - val_loss: 0.2427 - val_acc: 0.9667
    Epoch 147/200
    360/360 [==============================] - 8s - loss: 8.7622e-05 - acc: 1.0000 - val_loss: 0.2430 - val_acc: 0.9667
    Epoch 148/200
    360/360 [==============================] - 8s - loss: 8.6387e-05 - acc: 1.0000 - val_loss: 0.2432 - val_acc: 0.9667
    Epoch 149/200
    360/360 [==============================] - 8s - loss: 8.5216e-05 - acc: 1.0000 - val_loss: 0.2437 - val_acc: 0.9667
    Epoch 150/200
    360/360 [==============================] - 8s - loss: 8.4102e-05 - acc: 1.0000 - val_loss: 0.2437 - val_acc: 0.9667
    Epoch 151/200
    360/360 [==============================] - 8s - loss: 8.2816e-05 - acc: 1.0000 - val_loss: 0.2439 - val_acc: 0.9667
    Epoch 152/200
    360/360 [==============================] - 7s - loss: 8.1792e-05 - acc: 1.0000 - val_loss: 0.2442 - val_acc: 0.9667
    Epoch 153/200
    360/360 [==============================] - 8s - loss: 8.0608e-05 - acc: 1.0000 - val_loss: 0.2452 - val_acc: 0.9667
    Epoch 154/200
    360/360 [==============================] - 9s - loss: 7.9455e-05 - acc: 1.0000 - val_loss: 0.2452 - val_acc: 0.9667
    Epoch 155/200
    360/360 [==============================] - 8s - loss: 7.8410e-05 - acc: 1.0000 - val_loss: 0.2455 - val_acc: 0.9667
    Epoch 156/200
    360/360 [==============================] - 8s - loss: 7.7260e-05 - acc: 1.0000 - val_loss: 0.2460 - val_acc: 0.9667
    Epoch 157/200
    360/360 [==============================] - 8s - loss: 7.6211e-05 - acc: 1.0000 - val_loss: 0.2462 - val_acc: 0.9667
    Epoch 158/200
    360/360 [==============================] - 8s - loss: 7.5246e-05 - acc: 1.0000 - val_loss: 0.2465 - val_acc: 0.9667
    Epoch 159/200
    360/360 [==============================] - 8s - loss: 7.4265e-05 - acc: 1.0000 - val_loss: 0.2465 - val_acc: 0.9667
    Epoch 160/200
    360/360 [==============================] - 8s - loss: 7.3306e-05 - acc: 1.0000 - val_loss: 0.2466 - val_acc: 0.9667
    Epoch 161/200
    360/360 [==============================] - 9s - loss: 7.2220e-05 - acc: 1.0000 - val_loss: 0.2471 - val_acc: 0.9667
    Epoch 162/200
    360/360 [==============================] - 9s - loss: 7.1352e-05 - acc: 1.0000 - val_loss: 0.2477 - val_acc: 0.9667
    Epoch 163/200
    360/360 [==============================] - 9s - loss: 7.0441e-05 - acc: 1.0000 - val_loss: 0.2479 - val_acc: 0.9667
    Epoch 164/200
    360/360 [==============================] - 10s - loss: 6.9558e-05 - acc: 1.0000 - val_loss: 0.2480 - val_acc: 0.9667
    Epoch 165/200
    360/360 [==============================] - 8s - loss: 6.8522e-05 - acc: 1.0000 - val_loss: 0.2484 - val_acc: 0.9667
    Epoch 166/200
    360/360 [==============================] - 8s - loss: 6.7717e-05 - acc: 1.0000 - val_loss: 0.2485 - val_acc: 0.9667
    Epoch 167/200
    360/360 [==============================] - 8s - loss: 6.6771e-05 - acc: 1.0000 - val_loss: 0.2493 - val_acc: 0.9667
    Epoch 168/200
    360/360 [==============================] - 10s - loss: 6.6074e-05 - acc: 1.0000 - val_loss: 0.2500 - val_acc: 0.9667
    Epoch 169/200
    360/360 [==============================] - 10s - loss: 6.5206e-05 - acc: 1.0000 - val_loss: 0.2500 - val_acc: 0.9667
    Epoch 170/200
    360/360 [==============================] - 9s - loss: 6.4402e-05 - acc: 1.0000 - val_loss: 0.2495 - val_acc: 0.9667
    Epoch 171/200
    360/360 [==============================] - 8s - loss: 6.3488e-05 - acc: 1.0000 - val_loss: 0.2498 - val_acc: 0.9667
    Epoch 172/200
    360/360 [==============================] - 7s - loss: 6.2812e-05 - acc: 1.0000 - val_loss: 0.2497 - val_acc: 0.9667
    Epoch 173/200
    360/360 [==============================] - 7s - loss: 6.1908e-05 - acc: 1.0000 - val_loss: 0.2502 - val_acc: 0.9667
    Epoch 174/200
    360/360 [==============================] - 7s - loss: 6.1101e-05 - acc: 1.0000 - val_loss: 0.2507 - val_acc: 0.9667
    Epoch 175/200
    360/360 [==============================] - 7s - loss: 6.0332e-05 - acc: 1.0000 - val_loss: 0.2510 - val_acc: 0.9667
    Epoch 176/200
    360/360 [==============================] - 8s - loss: 5.9576e-05 - acc: 1.0000 - val_loss: 0.2512 - val_acc: 0.9667
    Epoch 177/200
    360/360 [==============================] - 8s - loss: 5.8893e-05 - acc: 1.0000 - val_loss: 0.2516 - val_acc: 0.9667
    Epoch 178/200
    360/360 [==============================] - 9s - loss: 5.8246e-05 - acc: 1.0000 - val_loss: 0.2523 - val_acc: 0.9667
    Epoch 179/200
    360/360 [==============================] - 9s - loss: 5.7458e-05 - acc: 1.0000 - val_loss: 0.2524 - val_acc: 0.9667
    Epoch 180/200
    360/360 [==============================] - 10s - loss: 5.6778e-05 - acc: 1.0000 - val_loss: 0.2523 - val_acc: 0.9667
    Epoch 181/200
    360/360 [==============================] - 9s - loss: 5.6043e-05 - acc: 1.0000 - val_loss: 0.2523 - val_acc: 0.9667
    Epoch 182/200
    360/360 [==============================] - 8s - loss: 5.5418e-05 - acc: 1.0000 - val_loss: 0.2526 - val_acc: 0.9667
    Epoch 183/200
    360/360 [==============================] - 8s - loss: 5.4749e-05 - acc: 1.0000 - val_loss: 0.2528 - val_acc: 0.9667
    Epoch 184/200
    360/360 [==============================] - 8s - loss: 5.4074e-05 - acc: 1.0000 - val_loss: 0.2528 - val_acc: 0.9667
    Epoch 185/200
    360/360 [==============================] - 8s - loss: 5.3481e-05 - acc: 1.0000 - val_loss: 0.2526 - val_acc: 0.9667
    Epoch 186/200
    360/360 [==============================] - 9s - loss: 5.2815e-05 - acc: 1.0000 - val_loss: 0.2531 - val_acc: 0.9667
    Epoch 187/200
    360/360 [==============================] - 9s - loss: 5.2150e-05 - acc: 1.0000 - val_loss: 0.2535 - val_acc: 0.9667
    Epoch 188/200
    360/360 [==============================] - 9s - loss: 5.1577e-05 - acc: 1.0000 - val_loss: 0.2542 - val_acc: 0.9667
    Epoch 189/200
    360/360 [==============================] - 8s - loss: 5.0962e-05 - acc: 1.0000 - val_loss: 0.2544 - val_acc: 0.9667
    Epoch 190/200
    360/360 [==============================] - 8s - loss: 5.0352e-05 - acc: 1.0000 - val_loss: 0.2546 - val_acc: 0.9667
    Epoch 191/200
    360/360 [==============================] - 7s - loss: 4.9782e-05 - acc: 1.0000 - val_loss: 0.2548 - val_acc: 0.9667
    Epoch 192/200
    360/360 [==============================] - 8s - loss: 4.9252e-05 - acc: 1.0000 - val_loss: 0.2549 - val_acc: 0.9667
    Epoch 193/200
    360/360 [==============================] - 7s - loss: 4.8699e-05 - acc: 1.0000 - val_loss: 0.2550 - val_acc: 0.9667
    Epoch 194/200
    360/360 [==============================] - 8s - loss: 4.8082e-05 - acc: 1.0000 - val_loss: 0.2553 - val_acc: 0.9667
    Epoch 195/200
    360/360 [==============================] - 8s - loss: 4.7522e-05 - acc: 1.0000 - val_loss: 0.2555 - val_acc: 0.9667
    Epoch 196/200
    360/360 [==============================] - 8s - loss: 4.7005e-05 - acc: 1.0000 - val_loss: 0.2560 - val_acc: 0.9667
    Epoch 197/200
    360/360 [==============================] - 8s - loss: 4.6471e-05 - acc: 1.0000 - val_loss: 0.2566 - val_acc: 0.9667
    Epoch 198/200
    360/360 [==============================] - 7s - loss: 4.5953e-05 - acc: 1.0000 - val_loss: 0.2571 - val_acc: 0.9667
    Epoch 199/200
    360/360 [==============================] - 8s - loss: 4.5439e-05 - acc: 1.0000 - val_loss: 0.2566 - val_acc: 0.9667
    Epoch 200/200
    360/360 [==============================] - 8s - loss: 4.4908e-05 - acc: 1.0000 - val_loss: 0.2567 - val_acc: 0.9667
    


```python
score = model.evaluate(testData, testLabels, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

    50/50 [==============================] - 0s     
    
    Test score: 0.712118139267
    Test accuracy: 0.939999990463
    


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

    dict_keys(['acc', 'val_acc', 'loss', 'val_loss'])
    


![_config.yml]({{ site.baseurl}}/images/lenetv2_output_33_1.png)



```python
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/lenetv2_output_34_0.png)



```python
model_json = model.to_json()
with open("modelv2.json", "w") as json_file:
    json_file.write(model_json)
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 7252-C405
    
     Directory of C:\Users\cobalt\workspace\FaceRecognition
    
    09/24/2017  05:57 PM    <DIR>          .
    09/24/2017  05:57 PM    <DIR>          ..
    09/24/2017  05:19 PM    <DIR>          .ipynb_checkpoints
    09/24/2017  05:21 PM         1,158,593 FaceRecognitionLeNet.ipynb
    09/24/2017  05:57 PM         1,172,692 FaceRecognitionLeNetv2.ipynb
    09/24/2017  09:59 AM    <DIR>          front_database
    09/24/2017  05:19 PM         5,049,496 model.h5
    09/24/2017  05:59 PM             3,146 model.json
    09/24/2017  03:32 PM         1,134,255 ReadMultipleImages.ipynb
    04/13/2016  11:59 AM         4,373,288 Revised Final Thesis Draft.pdf
    09/24/2017  10:00 AM    <DIR>          side_database
    09/24/2017  10:00 AM    <DIR>          videos
                   6 File(s)     12,891,470 bytes
                   6 Dir(s)  199,572,041,728 bytes free
    


```python
model.save_weights("modelv2.h5")
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 7252-C405
    
     Directory of C:\Users\cobalt\workspace\FaceRecognition
    
    09/24/2017  05:57 PM    <DIR>          .
    09/24/2017  05:57 PM    <DIR>          ..
    09/24/2017  05:19 PM    <DIR>          .ipynb_checkpoints
    09/24/2017  05:21 PM         1,158,593 FaceRecognitionLeNet.ipynb
    09/24/2017  05:57 PM         1,172,692 FaceRecognitionLeNetv2.ipynb
    09/24/2017  09:59 AM    <DIR>          front_database
    09/24/2017  05:59 PM        14,549,496 model.h5
    09/24/2017  05:59 PM             3,146 model.json
    09/24/2017  03:32 PM         1,134,255 ReadMultipleImages.ipynb
    04/13/2016  11:59 AM         4,373,288 Revised Final Thesis Draft.pdf
    09/24/2017  10:00 AM    <DIR>          side_database
    09/24/2017  10:00 AM    <DIR>          videos
                   6 File(s)     22,391,470 bytes
                   6 Dir(s)  199,562,539,008 bytes free
    


```python
json_file = open('modelv2.json', 'r')
loaded_model_json = json_file.read()
json_file.close()
loaded_model = model_from_json(loaded_model_json)
loaded_model.load_weights("modelv2.h5")
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
    
    Test score: 0.712118139267
    Test accuracy: 0.939999990463
    


```python
disp_images = []
for i in np.random.choice(np.arange(0, len(testLabels)), size=(10, )):
        probs = model.predict(testData[np.newaxis, i])
        print('Probability: %s' % probs)     
        prediction = probs.argmax(axis=1)
        print('Prediction: %s' % probs[0][prediction])  
        image = (testData[i][0] * 255).astype("uint8")  
        name = str(prediction[0]) + ':' + name_map[str(prediction[0])]
        if prediction[0] in name_map:
            name = name_map[name]
        cv2.putText(image, name, (0, 40), cv2.FONT_HERSHEY_PLAIN, 1, (255, 255, 255), 1)  
        disp_images.append(image)
        print("[INFO] Predicted: {}, Actual: {}".format(prediction[0], np.argmax(testLabels[i])))  
        print('\n')
```

    Probability: [[  8.03453780e-15   5.17064891e-10   5.85350608e-06   4.64853607e-11
        1.91105482e-11   3.90518517e-06   1.85521720e-11   2.61722360e-15
        9.99990225e-01   1.61211388e-19]]
    Prediction: [ 0.99999022]
    [INFO] Predicted: 8, Actual: 8
    
    
    Probability: [[  1.78185900e-07   2.15672455e-16   1.64836189e-09   1.25840722e-12
        4.04218281e-06   2.01808107e-05   9.99975443e-01   1.26211702e-07
        5.31563149e-09   3.83116152e-16]]
    Prediction: [ 0.99997544]
    [INFO] Predicted: 6, Actual: 6
    
    
    Probability: [[  2.50768521e-17   1.71083939e-10   5.60480871e-08   1.00649853e-11
        1.86242013e-13   6.50737964e-10   2.78688218e-11   9.60999178e-20
        1.00000000e+00   2.41323456e-19]]
    Prediction: [ 1.]
    [INFO] Predicted: 8, Actual: 8
    
    
    Probability: [[  1.50045835e-07   4.15935064e-11   4.50072065e-07   2.02657631e-07
        3.10643733e-09   4.62222170e-11   7.05312715e-08   9.99999166e-01
        6.09761807e-12   5.05142328e-15]]
    Prediction: [ 0.99999917]
    [INFO] Predicted: 7, Actual: 7
    
    
    Probability: [[  6.51613789e-07   3.73432414e-07   5.23403469e-06   9.99955893e-01
        7.14593534e-06   2.00251922e-08   1.66456921e-05   1.39141921e-05
        3.27722134e-08   1.86121669e-12]]
    Prediction: [ 0.99995589]
    [INFO] Predicted: 3, Actual: 3
    
    
    Probability: [[  1.80809343e-06   5.05559417e-09   1.96767451e-06   9.99856353e-01
        9.14117300e-06   1.37929035e-07   2.96969611e-05   1.00873658e-04
        8.16047852e-09   4.34419319e-13]]
    Prediction: [ 0.99985635]
    [INFO] Predicted: 3, Actual: 3
    
    
    Probability: [[  2.07763053e-14   1.39258591e-05   3.92980084e-08   9.99961495e-01
        1.57431561e-08   2.31823451e-05   7.15672964e-12   1.16721655e-09
        1.47138371e-06   3.63437070e-14]]
    Prediction: [ 0.9999615]
    [INFO] Predicted: 3, Actual: 3
    
    
    Probability: [[  1.02223496e-09   8.08510980e-10   9.99916434e-01   9.11639028e-11
        8.60298846e-11   9.09794740e-10   2.88723823e-09   1.23814592e-09
        8.35282917e-05   1.39087564e-15]]
    Prediction: [ 0.99991643]
    [INFO] Predicted: 2, Actual: 2
    
    
    Probability: [[  9.99933839e-01   5.58049459e-18   3.63664689e-11   1.54425550e-12
        4.33314912e-10   2.45942711e-10   1.16289903e-05   5.44454597e-05
        1.75191216e-11   1.29337157e-11]]
    Prediction: [ 0.99993384]
    [INFO] Predicted: 0, Actual: 0
    
    
    Probability: [[  5.78699345e-11   2.68773321e-04   1.25578697e-06   1.23696402e-04
        3.79698645e-06   9.99597013e-01   3.22482447e-06   1.66603206e-06
        5.37658423e-07   2.97908920e-10]]
    Prediction: [ 0.99959701]
    [INFO] Predicted: 5, Actual: 5
    
    
    


```python
fig = plt.figure(figsize= (20,50))
for k in range(10):
    plt.subplot(1, 10, k+1)      
    plt.gca().axes.get_yaxis().set_visible(False)
    plt.gca().axes.get_xaxis().set_visible(False)
    plt.imshow(disp_images[k], cmap='gray')
```


![_config.yml]({{ site.baseurl}}/images/lenetv2_output_40_0.png)

-mkc
