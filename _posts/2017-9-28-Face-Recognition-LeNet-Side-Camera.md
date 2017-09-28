
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
            if any([filename.endswith(x) for x in ['.jpeg', '.jpg','.pgm','.png']]):
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

root_dir = r'C:\Users\ECE\workspace\SimpleFaceRecognitionDemo\side_database'

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
    No. of images = 300. 
    No. of labels = 300. 
    0
    name_map[0] = abad. 
    name_map[1] = agui. 
    name_map[2] = cans. 
    name_map[9] = venu. 
    

Step 4: Plot the first image.


```python
plt.imshow(images[0], cmap='gray')
```




    <matplotlib.image.AxesImage at 0x1fc3be8d8d0>




![_config.yml]({{ site.baseurl}}/images/sidecam_output_11_1.png)


Step 5: Define method for plotting a person: 50 images 


```python
def plot_person_category(images, id):
    for k in range(30*id,30*id + 30):
        plt.subplot(3, 10, k - (30*id - 1))      
        plt.gca().axes.get_yaxis().set_visible(False)
        plt.gca().axes.get_xaxis().set_visible(False)
        plt.imshow(images[k], cmap='gray')
```


```python
# Plot person 0
plot_person_category(images, 0)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_14_0.png)



```python
# Plot person 1
plot_person_category(images, 1)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_15_0.png)



```python
# Plot person 2
plot_person_category(images, 2)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_16_0.png)



```python
# Plot person 3
plot_person_category(images, 3)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_17_0.png)



```python
# Plot person 4
plot_person_category(images, 4)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_18_0.png)



```python
# Plot person 5
plot_person_category(images, 5)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_19_0.png)



```python
# Plot person 6
plot_person_category(images, 6)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_20_0.png)



```python
# Plot person 7
plot_person_category(images, 7)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_21_0.png)



```python
# Plot person 8
plot_person_category(images, 8)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_22_0.png)



```python
# Plot person 9
plot_person_category(images, 9)
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_23_0.png)


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
    Before: testLabels[0] = 7
    After: trainLabels[0] = [ 0.  0.  0.  0.  0.  0.  0.  0.  0.  1.]
    After: testLabels[0] = [ 0.  0.  0.  0.  0.  0.  0.  1.  0.  0.]
    


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

    (270, 1, 50, 50) train samples
    (30, 1, 50, 50) test samples
    


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

    Train on 216 samples, validate on 54 samples
    Epoch 1/200
    216/216 [==============================] - 1s - loss: 2.3297 - acc: 0.1528 - val_loss: 2.1704 - val_acc: 0.2778
    Epoch 2/200
    216/216 [==============================] - 1s - loss: 2.1152 - acc: 0.2870 - val_loss: 1.9234 - val_acc: 0.4630
    Epoch 3/200
    216/216 [==============================] - 1s - loss: 1.8102 - acc: 0.6435 - val_loss: 1.5807 - val_acc: 0.6852
    Epoch 4/200
    216/216 [==============================] - 1s - loss: 1.4728 - acc: 0.6528 - val_loss: 1.2778 - val_acc: 0.6296
    Epoch 5/200
    216/216 [==============================] - 1s - loss: 1.1121 - acc: 0.6898 - val_loss: 1.0963 - val_acc: 0.5926
    Epoch 6/200
    216/216 [==============================] - 1s - loss: 0.9535 - acc: 0.6898 - val_loss: 0.9401 - val_acc: 0.5556
    Epoch 7/200
    216/216 [==============================] - 1s - loss: 0.8596 - acc: 0.6713 - val_loss: 0.8810 - val_acc: 0.7222
    Epoch 8/200
    216/216 [==============================] - 1s - loss: 0.6455 - acc: 0.7963 - val_loss: 0.5918 - val_acc: 0.8148
    Epoch 9/200
    216/216 [==============================] - 1s - loss: 0.5580 - acc: 0.8287 - val_loss: 0.4393 - val_acc: 0.9074
    Epoch 10/200
    216/216 [==============================] - 1s - loss: 0.3775 - acc: 0.9167 - val_loss: 0.4237 - val_acc: 0.8704
    Epoch 11/200
    216/216 [==============================] - 1s - loss: 0.2898 - acc: 0.9537 - val_loss: 0.2597 - val_acc: 0.9630
    Epoch 12/200
    216/216 [==============================] - 1s - loss: 0.2045 - acc: 0.9583 - val_loss: 0.2491 - val_acc: 0.9444
    Epoch 13/200
    216/216 [==============================] - 1s - loss: 0.1601 - acc: 0.9676 - val_loss: 0.2635 - val_acc: 0.9259
    Epoch 14/200
    216/216 [==============================] - 1s - loss: 0.1253 - acc: 0.9722 - val_loss: 0.1677 - val_acc: 0.9444
    Epoch 15/200
    216/216 [==============================] - 1s - loss: 0.0769 - acc: 0.9954 - val_loss: 0.1287 - val_acc: 0.9815
    Epoch 16/200
    216/216 [==============================] - 1s - loss: 0.0533 - acc: 1.0000 - val_loss: 0.1333 - val_acc: 0.9630
    Epoch 17/200
    216/216 [==============================] - 1s - loss: 0.0344 - acc: 1.0000 - val_loss: 0.1249 - val_acc: 0.9630
    Epoch 18/200
    216/216 [==============================] - 1s - loss: 0.0242 - acc: 1.0000 - val_loss: 0.1185 - val_acc: 0.9815
    Epoch 19/200
    216/216 [==============================] - 1s - loss: 0.0173 - acc: 1.0000 - val_loss: 0.1058 - val_acc: 0.9815
    Epoch 20/200
    216/216 [==============================] - 1s - loss: 0.0129 - acc: 1.0000 - val_loss: 0.1077 - val_acc: 0.9815
    Epoch 21/200
    216/216 [==============================] - 1s - loss: 0.0114 - acc: 1.0000 - val_loss: 0.0929 - val_acc: 0.9815
    Epoch 22/200
    216/216 [==============================] - 1s - loss: 0.0067 - acc: 1.0000 - val_loss: 0.0776 - val_acc: 0.9815
    Epoch 23/200
    216/216 [==============================] - 1s - loss: 0.0052 - acc: 1.0000 - val_loss: 0.0638 - val_acc: 0.9815
    Epoch 24/200
    216/216 [==============================] - 1s - loss: 0.0053 - acc: 1.0000 - val_loss: 0.0590 - val_acc: 0.9815
    Epoch 25/200
    216/216 [==============================] - 1s - loss: 0.0043 - acc: 1.0000 - val_loss: 0.0637 - val_acc: 0.9815
    Epoch 26/200
    216/216 [==============================] - 1s - loss: 0.0030 - acc: 1.0000 - val_loss: 0.0747 - val_acc: 0.9815
    Epoch 27/200
    216/216 [==============================] - 1s - loss: 0.0023 - acc: 1.0000 - val_loss: 0.0826 - val_acc: 0.9815
    Epoch 28/200
    216/216 [==============================] - 1s - loss: 0.0021 - acc: 1.0000 - val_loss: 0.0859 - val_acc: 0.9815
    Epoch 29/200
    216/216 [==============================] - 1s - loss: 0.0018 - acc: 1.0000 - val_loss: 0.0840 - val_acc: 0.9815
    Epoch 30/200
    216/216 [==============================] - 1s - loss: 0.0015 - acc: 1.0000 - val_loss: 0.0803 - val_acc: 0.9815
    Epoch 31/200
    216/216 [==============================] - 1s - loss: 0.0014 - acc: 1.0000 - val_loss: 0.0760 - val_acc: 0.9815
    Epoch 32/200
    216/216 [==============================] - 1s - loss: 0.0012 - acc: 1.0000 - val_loss: 0.0727 - val_acc: 0.9815
    Epoch 33/200
    216/216 [==============================] - 1s - loss: 0.0011 - acc: 1.0000 - val_loss: 0.0716 - val_acc: 0.9815
    Epoch 34/200
    216/216 [==============================] - 1s - loss: 9.9359e-04 - acc: 1.0000 - val_loss: 0.0713 - val_acc: 0.9815
    Epoch 35/200
    216/216 [==============================] - 1s - loss: 9.0935e-04 - acc: 1.0000 - val_loss: 0.0715 - val_acc: 0.9815
    Epoch 36/200
    216/216 [==============================] - 1s - loss: 8.3960e-04 - acc: 1.0000 - val_loss: 0.0711 - val_acc: 0.9815
    Epoch 37/200
    216/216 [==============================] - 1s - loss: 7.8279e-04 - acc: 1.0000 - val_loss: 0.0700 - val_acc: 0.9815
    Epoch 38/200
    216/216 [==============================] - 1s - loss: 7.4003e-04 - acc: 1.0000 - val_loss: 0.0691 - val_acc: 0.9815
    Epoch 39/200
    216/216 [==============================] - 1s - loss: 6.9540e-04 - acc: 1.0000 - val_loss: 0.0689 - val_acc: 0.9815
    Epoch 40/200
    216/216 [==============================] - 1s - loss: 6.6292e-04 - acc: 1.0000 - val_loss: 0.0682 - val_acc: 0.9815
    Epoch 41/200
    216/216 [==============================] - 1s - loss: 6.2673e-04 - acc: 1.0000 - val_loss: 0.0675 - val_acc: 0.9815
    Epoch 42/200
    216/216 [==============================] - 1s - loss: 5.9428e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 43/200
    216/216 [==============================] - 1s - loss: 5.7050e-04 - acc: 1.0000 - val_loss: 0.0655 - val_acc: 0.9815
    Epoch 44/200
    216/216 [==============================] - 1s - loss: 5.4605e-04 - acc: 1.0000 - val_loss: 0.0644 - val_acc: 0.9815
    Epoch 45/200
    216/216 [==============================] - 1s - loss: 5.3002e-04 - acc: 1.0000 - val_loss: 0.0637 - val_acc: 0.9815
    Epoch 46/200
    216/216 [==============================] - 1s - loss: 5.1548e-04 - acc: 1.0000 - val_loss: 0.0635 - val_acc: 0.9815
    Epoch 47/200
    216/216 [==============================] - 1s - loss: 5.0072e-04 - acc: 1.0000 - val_loss: 0.0638 - val_acc: 0.9815
    Epoch 48/200
    216/216 [==============================] - 1s - loss: 4.8597e-04 - acc: 1.0000 - val_loss: 0.0645 - val_acc: 0.9815
    Epoch 49/200
    216/216 [==============================] - 1s - loss: 4.7229e-04 - acc: 1.0000 - val_loss: 0.0656 - val_acc: 0.9815
    Epoch 50/200
    216/216 [==============================] - 1s - loss: 4.5777e-04 - acc: 1.0000 - val_loss: 0.0665 - val_acc: 0.9815
    Epoch 51/200
    216/216 [==============================] - 1s - loss: 4.4405e-04 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 52/200
    216/216 [==============================] - 1s - loss: 4.3254e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 53/200
    216/216 [==============================] - 1s - loss: 4.2196e-04 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 54/200
    216/216 [==============================] - 1s - loss: 4.0749e-04 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 55/200
    216/216 [==============================] - 1s - loss: 3.9810e-04 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 56/200
    216/216 [==============================] - 1s - loss: 3.8845e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 57/200
    216/216 [==============================] - 1s - loss: 3.7837e-04 - acc: 1.0000 - val_loss: 0.0674 - val_acc: 0.9815
    Epoch 58/200
    216/216 [==============================] - 1s - loss: 3.6900e-04 - acc: 1.0000 - val_loss: 0.0677 - val_acc: 0.9815
    Epoch 59/200
    216/216 [==============================] - 1s - loss: 3.6050e-04 - acc: 1.0000 - val_loss: 0.0676 - val_acc: 0.9815
    Epoch 60/200
    216/216 [==============================] - 1s - loss: 3.5287e-04 - acc: 1.0000 - val_loss: 0.0671 - val_acc: 0.9815
    Epoch 61/200
    216/216 [==============================] - 1s - loss: 3.4336e-04 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 62/200
    216/216 [==============================] - 1s - loss: 3.3587e-04 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 63/200
    216/216 [==============================] - 1s - loss: 3.2692e-04 - acc: 1.0000 - val_loss: 0.0665 - val_acc: 0.9815
    Epoch 64/200
    216/216 [==============================] - 1s - loss: 3.1922e-04 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 65/200
    216/216 [==============================] - 1s - loss: 3.1176e-04 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 66/200
    216/216 [==============================] - 1s - loss: 3.0443e-04 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 67/200
    216/216 [==============================] - 1s - loss: 2.9859e-04 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 68/200
    216/216 [==============================] - 1s - loss: 2.9116e-04 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 69/200
    216/216 [==============================] - 1s - loss: 2.8416e-04 - acc: 1.0000 - val_loss: 0.0672 - val_acc: 0.9815
    Epoch 70/200
    216/216 [==============================] - 1s - loss: 2.7777e-04 - acc: 1.0000 - val_loss: 0.0674 - val_acc: 0.9815
    Epoch 71/200
    216/216 [==============================] - 1s - loss: 2.7203e-04 - acc: 1.0000 - val_loss: 0.0671 - val_acc: 0.9815
    Epoch 72/200
    216/216 [==============================] - 1s - loss: 2.6497e-04 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 73/200
    216/216 [==============================] - 1s - loss: 2.5947e-04 - acc: 1.0000 - val_loss: 0.0671 - val_acc: 0.9815
    Epoch 74/200
    216/216 [==============================] - 1s - loss: 2.5296e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 75/200
    216/216 [==============================] - 1s - loss: 2.4790e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 76/200
    216/216 [==============================] - 1s - loss: 2.4229e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 77/200
    216/216 [==============================] - 1s - loss: 2.3696e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 78/200
    216/216 [==============================] - 1s - loss: 2.3199e-04 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 79/200
    216/216 [==============================] - 1s - loss: 2.2661e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 80/200
    216/216 [==============================] - 1s - loss: 2.2191e-04 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 81/200
    216/216 [==============================] - 1s - loss: 2.1708e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 82/200
    216/216 [==============================] - 1s - loss: 2.1205e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 83/200
    216/216 [==============================] - 1s - loss: 2.0761e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 84/200
    216/216 [==============================] - 1s - loss: 2.0338e-04 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 85/200
    216/216 [==============================] - 1s - loss: 1.9897e-04 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 86/200
    216/216 [==============================] - 1s - loss: 1.9469e-04 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 87/200
    216/216 [==============================] - 1s - loss: 1.9069e-04 - acc: 1.0000 - val_loss: 0.0663 - val_acc: 0.9815
    Epoch 88/200
    216/216 [==============================] - 1s - loss: 1.8658e-04 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 89/200
    216/216 [==============================] - 1s - loss: 1.8265e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 90/200
    216/216 [==============================] - 1s - loss: 1.7901e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 91/200
    216/216 [==============================] - 1s - loss: 1.7532e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 92/200
    216/216 [==============================] - 1s - loss: 1.7160e-04 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 93/200
    216/216 [==============================] - 1s - loss: 1.6802e-04 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 94/200
    216/216 [==============================] - 1s - loss: 1.6435e-04 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 95/200
    216/216 [==============================] - 1s - loss: 1.6141e-04 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 96/200
    216/216 [==============================] - 1s - loss: 1.5828e-04 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 97/200
    216/216 [==============================] - 1s - loss: 1.5529e-04 - acc: 1.0000 - val_loss: 0.0663 - val_acc: 0.9815
    Epoch 98/200
    216/216 [==============================] - 1s - loss: 1.5188e-04 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 99/200
    216/216 [==============================] - 1s - loss: 1.4878e-04 - acc: 1.0000 - val_loss: 0.0673 - val_acc: 0.9815
    Epoch 100/200
    216/216 [==============================] - 1s - loss: 1.4619e-04 - acc: 1.0000 - val_loss: 0.0677 - val_acc: 0.9815
    Epoch 101/200
    216/216 [==============================] - 1s - loss: 1.4313e-04 - acc: 1.0000 - val_loss: 0.0674 - val_acc: 0.9815
    Epoch 102/200
    216/216 [==============================] - 1s - loss: 1.4000e-04 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 103/200
    216/216 [==============================] - 1s - loss: 1.3752e-04 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 104/200
    216/216 [==============================] - 1s - loss: 1.3452e-04 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 105/200
    216/216 [==============================] - 1s - loss: 1.3182e-04 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 106/200
    216/216 [==============================] - 1s - loss: 1.2951e-04 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 107/200
    216/216 [==============================] - 1s - loss: 1.2745e-04 - acc: 1.0000 - val_loss: 0.0651 - val_acc: 0.9815
    Epoch 108/200
    216/216 [==============================] - 1s - loss: 1.2474e-04 - acc: 1.0000 - val_loss: 0.0655 - val_acc: 0.9815
    Epoch 109/200
    216/216 [==============================] - 1s - loss: 1.2239e-04 - acc: 1.0000 - val_loss: 0.0660 - val_acc: 0.9815
    Epoch 110/200
    216/216 [==============================] - 1s - loss: 1.1978e-04 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 111/200
    216/216 [==============================] - 1s - loss: 1.1758e-04 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 112/200
    216/216 [==============================] - 1s - loss: 1.1550e-04 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 113/200
    216/216 [==============================] - 1s - loss: 1.1311e-04 - acc: 1.0000 - val_loss: 0.0673 - val_acc: 0.9815
    Epoch 114/200
    216/216 [==============================] - 1s - loss: 1.1098e-04 - acc: 1.0000 - val_loss: 0.0671 - val_acc: 0.9815
    Epoch 115/200
    216/216 [==============================] - 1s - loss: 1.0898e-04 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 116/200
    216/216 [==============================] - 1s - loss: 1.0691e-04 - acc: 1.0000 - val_loss: 0.0665 - val_acc: 0.9815
    Epoch 117/200
    216/216 [==============================] - 1s - loss: 1.0504e-04 - acc: 1.0000 - val_loss: 0.0665 - val_acc: 0.9815
    Epoch 118/200
    216/216 [==============================] - 1s - loss: 1.0300e-04 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 119/200
    216/216 [==============================] - 1s - loss: 1.0133e-04 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 120/200
    216/216 [==============================] - 1s - loss: 9.9508e-05 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 121/200
    216/216 [==============================] - 1s - loss: 9.7730e-05 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 122/200
    216/216 [==============================] - 1s - loss: 9.5950e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 123/200
    216/216 [==============================] - 1s - loss: 9.4147e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 124/200
    216/216 [==============================] - 1s - loss: 9.2385e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 125/200
    216/216 [==============================] - 1s - loss: 9.0742e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 126/200
    216/216 [==============================] - 1s - loss: 8.9168e-05 - acc: 1.0000 - val_loss: 0.0665 - val_acc: 0.9815
    Epoch 127/200
    216/216 [==============================] - 1s - loss: 8.7582e-05 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 128/200
    216/216 [==============================] - 1s - loss: 8.6093e-05 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 129/200
    216/216 [==============================] - 1s - loss: 8.4625e-05 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 130/200
    216/216 [==============================] - 1s - loss: 8.3296e-05 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 131/200
    216/216 [==============================] - 1s - loss: 8.1832e-05 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 132/200
    216/216 [==============================] - 1s - loss: 8.0361e-05 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 133/200
    216/216 [==============================] - 1s - loss: 7.9021e-05 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 134/200
    216/216 [==============================] - 1s - loss: 7.7843e-05 - acc: 1.0000 - val_loss: 0.0670 - val_acc: 0.9815
    Epoch 135/200
    216/216 [==============================] - 1s - loss: 7.6466e-05 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 136/200
    216/216 [==============================] - 1s - loss: 7.5208e-05 - acc: 1.0000 - val_loss: 0.0663 - val_acc: 0.9815
    Epoch 137/200
    216/216 [==============================] - 1s - loss: 7.3944e-05 - acc: 1.0000 - val_loss: 0.0659 - val_acc: 0.9815
    Epoch 138/200
    216/216 [==============================] - 1s - loss: 7.2796e-05 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 139/200
    216/216 [==============================] - 1s - loss: 7.1548e-05 - acc: 1.0000 - val_loss: 0.0659 - val_acc: 0.9815
    Epoch 140/200
    216/216 [==============================] - 1s - loss: 7.0414e-05 - acc: 1.0000 - val_loss: 0.0660 - val_acc: 0.9815
    Epoch 141/200
    216/216 [==============================] - 1s - loss: 6.9232e-05 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 142/200
    216/216 [==============================] - 1s - loss: 6.8167e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 143/200
    216/216 [==============================] - 1s - loss: 6.6993e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 144/200
    216/216 [==============================] - 1s - loss: 6.5988e-05 - acc: 1.0000 - val_loss: 0.0666 - val_acc: 0.9815
    Epoch 145/200
    216/216 [==============================] - 1s - loss: 6.4958e-05 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 146/200
    216/216 [==============================] - 1s - loss: 6.3861e-05 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 147/200
    216/216 [==============================] - 1s - loss: 6.2931e-05 - acc: 1.0000 - val_loss: 0.0668 - val_acc: 0.9815
    Epoch 148/200
    216/216 [==============================] - 1s - loss: 6.1936e-05 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 149/200
    216/216 [==============================] - 1s - loss: 6.0932e-05 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 150/200
    216/216 [==============================] - 1s - loss: 6.0080e-05 - acc: 1.0000 - val_loss: 0.0669 - val_acc: 0.9815
    Epoch 151/200
    216/216 [==============================] - 1s - loss: 5.9186e-05 - acc: 1.0000 - val_loss: 0.0667 - val_acc: 0.9815
    Epoch 152/200
    216/216 [==============================] - 1s - loss: 5.8182e-05 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 153/200
    216/216 [==============================] - 1s - loss: 5.7280e-05 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 154/200
    216/216 [==============================] - 1s - loss: 5.6604e-05 - acc: 1.0000 - val_loss: 0.0653 - val_acc: 0.9815
    Epoch 155/200
    216/216 [==============================] - 1s - loss: 5.5757e-05 - acc: 1.0000 - val_loss: 0.0654 - val_acc: 0.9815
    Epoch 156/200
    216/216 [==============================] - 1s - loss: 5.4917e-05 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 157/200
    216/216 [==============================] - 1s - loss: 5.3962e-05 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 158/200
    216/216 [==============================] - 1s - loss: 5.3155e-05 - acc: 1.0000 - val_loss: 0.0660 - val_acc: 0.9815
    Epoch 159/200
    216/216 [==============================] - 1s - loss: 5.2390e-05 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 160/200
    216/216 [==============================] - 1s - loss: 5.1618e-05 - acc: 1.0000 - val_loss: 0.0664 - val_acc: 0.9815
    Epoch 161/200
    216/216 [==============================] - 1s - loss: 5.0989e-05 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 162/200
    216/216 [==============================] - 1s - loss: 5.0247e-05 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 163/200
    216/216 [==============================] - 1s - loss: 4.9455e-05 - acc: 1.0000 - val_loss: 0.0662 - val_acc: 0.9815
    Epoch 164/200
    216/216 [==============================] - 1s - loss: 4.8762e-05 - acc: 1.0000 - val_loss: 0.0659 - val_acc: 0.9815
    Epoch 165/200
    216/216 [==============================] - 1s - loss: 4.8079e-05 - acc: 1.0000 - val_loss: 0.0655 - val_acc: 0.9815
    Epoch 166/200
    216/216 [==============================] - 1s - loss: 4.7387e-05 - acc: 1.0000 - val_loss: 0.0653 - val_acc: 0.9815
    Epoch 167/200
    216/216 [==============================] - 1s - loss: 4.6675e-05 - acc: 1.0000 - val_loss: 0.0653 - val_acc: 0.9815
    Epoch 168/200
    216/216 [==============================] - 1s - loss: 4.5998e-05 - acc: 1.0000 - val_loss: 0.0656 - val_acc: 0.9815
    Epoch 169/200
    216/216 [==============================] - 1s - loss: 4.5399e-05 - acc: 1.0000 - val_loss: 0.0656 - val_acc: 0.9815
    Epoch 170/200
    216/216 [==============================] - 1s - loss: 4.4683e-05 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 171/200
    216/216 [==============================] - 1s - loss: 4.4086e-05 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 172/200
    216/216 [==============================] - 1s - loss: 4.3505e-05 - acc: 1.0000 - val_loss: 0.0661 - val_acc: 0.9815
    Epoch 173/200
    216/216 [==============================] - 1s - loss: 4.2925e-05 - acc: 1.0000 - val_loss: 0.0659 - val_acc: 0.9815
    Epoch 174/200
    216/216 [==============================] - 1s - loss: 4.2332e-05 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 175/200
    216/216 [==============================] - 1s - loss: 4.1722e-05 - acc: 1.0000 - val_loss: 0.0656 - val_acc: 0.9815
    Epoch 176/200
    216/216 [==============================] - 1s - loss: 4.1158e-05 - acc: 1.0000 - val_loss: 0.0654 - val_acc: 0.9815
    Epoch 177/200
    216/216 [==============================] - 1s - loss: 4.0592e-05 - acc: 1.0000 - val_loss: 0.0653 - val_acc: 0.9815
    Epoch 178/200
    216/216 [==============================] - 1s - loss: 4.0089e-05 - acc: 1.0000 - val_loss: 0.0653 - val_acc: 0.9815
    Epoch 179/200
    216/216 [==============================] - 1s - loss: 3.9537e-05 - acc: 1.0000 - val_loss: 0.0652 - val_acc: 0.9815
    Epoch 180/200
    216/216 [==============================] - 1s - loss: 3.9023e-05 - acc: 1.0000 - val_loss: 0.0652 - val_acc: 0.9815
    Epoch 181/200
    216/216 [==============================] - 1s - loss: 3.8486e-05 - acc: 1.0000 - val_loss: 0.0652 - val_acc: 0.9815
    Epoch 182/200
    216/216 [==============================] - 1s - loss: 3.7997e-05 - acc: 1.0000 - val_loss: 0.0655 - val_acc: 0.9815
    Epoch 183/200
    216/216 [==============================] - 1s - loss: 3.7522e-05 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 184/200
    216/216 [==============================] - 1s - loss: 3.6968e-05 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 185/200
    216/216 [==============================] - 1s - loss: 3.6549e-05 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 186/200
    216/216 [==============================] - 1s - loss: 3.6125e-05 - acc: 1.0000 - val_loss: 0.0658 - val_acc: 0.9815
    Epoch 187/200
    216/216 [==============================] - 1s - loss: 3.5552e-05 - acc: 1.0000 - val_loss: 0.0655 - val_acc: 0.9815
    Epoch 188/200
    216/216 [==============================] - 1s - loss: 3.5120e-05 - acc: 1.0000 - val_loss: 0.0652 - val_acc: 0.9815
    Epoch 189/200
    216/216 [==============================] - 1s - loss: 3.4698e-05 - acc: 1.0000 - val_loss: 0.0650 - val_acc: 0.9815
    Epoch 190/200
    216/216 [==============================] - 1s - loss: 3.4294e-05 - acc: 1.0000 - val_loss: 0.0649 - val_acc: 0.9815
    Epoch 191/200
    216/216 [==============================] - 1s - loss: 3.3860e-05 - acc: 1.0000 - val_loss: 0.0649 - val_acc: 0.9815
    Epoch 192/200
    216/216 [==============================] - 1s - loss: 3.3429e-05 - acc: 1.0000 - val_loss: 0.0649 - val_acc: 0.9815
    Epoch 193/200
    216/216 [==============================] - 1s - loss: 3.3027e-05 - acc: 1.0000 - val_loss: 0.0650 - val_acc: 0.9815
    Epoch 194/200
    216/216 [==============================] - 1s - loss: 3.2630e-05 - acc: 1.0000 - val_loss: 0.0652 - val_acc: 0.9815
    Epoch 195/200
    216/216 [==============================] - 1s - loss: 3.2265e-05 - acc: 1.0000 - val_loss: 0.0652 - val_acc: 0.9815
    Epoch 196/200
    216/216 [==============================] - 1s - loss: 3.1830e-05 - acc: 1.0000 - val_loss: 0.0655 - val_acc: 0.9815
    Epoch 197/200
    216/216 [==============================] - 1s - loss: 3.1440e-05 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 198/200
    216/216 [==============================] - 1s - loss: 3.1091e-05 - acc: 1.0000 - val_loss: 0.0657 - val_acc: 0.9815
    Epoch 199/200
    216/216 [==============================] - 1s - loss: 3.0724e-05 - acc: 1.0000 - val_loss: 0.0656 - val_acc: 0.9815
    Epoch 200/200
    216/216 [==============================] - 1s - loss: 3.0307e-05 - acc: 1.0000 - val_loss: 0.0654 - val_acc: 0.9815
    


```python
score = model.evaluate(testData, testLabels, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

    30/30 [==============================] - 0s
    
    Test score: 0.536379516125
    Test accuracy: 0.933333337307
    


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

    dict_keys(['val_acc', 'val_loss', 'acc', 'loss'])
    


![_config.yml]({{ site.baseurl}}/images/sidecam_output_33_1.png)



```python
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_34_0.png)



```python
model_json = model.to_json()
with open("modelv2_side.json", "w") as json_file:
    json_file.write(model_json)
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 4E4C-DF71
    
     Directory of C:\Users\ECE\workspace\SimpleFaceRecognitionDemo
    
    28/09/2017  07:53 AM    <DIR>          .
    28/09/2017  07:53 AM    <DIR>          ..
    28/09/2017  07:45 AM    <DIR>          .ipynb_checkpoints
    26/09/2017  03:23 PM         1,159,854 FaceRecognitionLeNet.ipynb
    26/09/2017  03:23 PM         1,272,131 FaceRecognitionLeNetv2.ipynb
    28/09/2017  07:52 AM           728,009 FaceRecognitionLeNetv2-Sideview.ipynb
    26/09/2017  03:23 PM         1,273,205 FaceRecognitionSimpleCNN.ipynb
    26/09/2017  03:23 PM    <DIR>          front_database
    26/09/2017  03:23 PM        14,549,496 model.h5
    26/09/2017  03:23 PM             3,146 model.json
    26/09/2017  03:23 PM         9,761,040 modelv2.h5
    26/09/2017  03:23 PM             6,534 modelv2.json
    28/09/2017  07:53 AM             3,146 modelv2_side.json
    26/09/2017  03:23 PM         1,134,658 ReadMultipleImages.ipynb
    26/09/2017  03:23 PM    <DIR>          side_database
                  10 File(s)     29,891,219 bytes
                   5 Dir(s)  60,779,360,256 bytes free
    


```python
model.save_weights("modelv2_side.h5")
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 4E4C-DF71
    
     Directory of C:\Users\ECE\workspace\SimpleFaceRecognitionDemo
    
    28/09/2017  07:53 AM    <DIR>          .
    28/09/2017  07:53 AM    <DIR>          ..
    28/09/2017  07:45 AM    <DIR>          .ipynb_checkpoints
    26/09/2017  03:23 PM         1,159,854 FaceRecognitionLeNet.ipynb
    26/09/2017  03:23 PM         1,272,131 FaceRecognitionLeNetv2.ipynb
    28/09/2017  07:52 AM           728,009 FaceRecognitionLeNetv2-Sideview.ipynb
    26/09/2017  03:23 PM         1,273,205 FaceRecognitionSimpleCNN.ipynb
    26/09/2017  03:23 PM    <DIR>          front_database
    26/09/2017  03:23 PM        14,549,496 model.h5
    26/09/2017  03:23 PM             3,146 model.json
    26/09/2017  03:23 PM         9,761,040 modelv2.h5
    26/09/2017  03:23 PM             6,534 modelv2.json
    28/09/2017  07:53 AM        14,549,504 modelv2_side.h5
    28/09/2017  07:53 AM             3,146 modelv2_side.json
    26/09/2017  03:23 PM         1,134,658 ReadMultipleImages.ipynb
    26/09/2017  03:23 PM    <DIR>          side_database
                  11 File(s)     44,440,723 bytes
                   5 Dir(s)  60,764,807,168 bytes free
    


```python
json_file = open('modelv2_side.json', 'r')
loaded_model_json = json_file.read()
json_file.close()
loaded_model = model_from_json(loaded_model_json)
loaded_model.load_weights("modelv2_side.h5")
```


```python
loaded_model.compile(loss='categorical_crossentropy',
              optimizer=OPTIMIZER,
              metrics=['accuracy'])
score = loaded_model.evaluate(testData, testLabels, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

    30/30 [==============================] - 0s
    
    Test score: 0.536379516125
    Test accuracy: 0.933333337307
    


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

    Probability: [[  2.38962933e-10   3.21299820e-10   2.19597784e-10   6.88336610e-11
        1.00032977e-08   4.05361578e-10   1.00000000e+00   1.83521962e-08
        3.30138417e-11   9.31914684e-11]]
    Prediction: [ 1.]
    [INFO] Predicted: 6, Actual: 6
    
    
    Probability: [[  1.14004306e-09   7.75151676e-09   2.47167486e-06   2.12647756e-05
        1.23196674e-04   1.16594885e-04   2.40304835e-17   9.99736369e-01
        1.05415744e-07   5.62618702e-11]]
    Prediction: [ 0.99973637]
    [INFO] Predicted: 7, Actual: 7
    
    
    Probability: [[  2.87111712e-09   4.02778788e-09   6.37268386e-05   9.99840856e-01
        3.73545518e-05   1.40569414e-08   1.63353491e-08   5.57994863e-05
        1.78941048e-06   4.39202779e-07]]
    Prediction: [ 0.99984086]
    [INFO] Predicted: 3, Actual: 3
    
    
    Probability: [[  1.48081925e-09   6.80416183e-12   9.99995232e-01   1.67848202e-06
        3.61044736e-11   2.66897726e-09   2.78441871e-06   3.64081302e-07
        4.30467528e-09   3.71892512e-13]]
    Prediction: [ 0.99999523]
    [INFO] Predicted: 2, Actual: 2
    
    
    Probability: [[  3.90606221e-13   2.65865818e-09   5.07930549e-15   6.10185798e-14
        9.24996812e-07   2.24755353e-11   9.99999046e-01   7.02793292e-12
        9.27358911e-14   4.75380313e-09]]
    Prediction: [ 0.99999905]
    [INFO] Predicted: 6, Actual: 6
    
    
    Probability: [[  3.45441517e-14   9.99803603e-01   4.34445492e-12   4.24936264e-09
        1.52539624e-12   1.57937960e-04   9.52945084e-11   3.85416679e-05
        1.55467375e-10   1.12030980e-08]]
    Prediction: [ 0.9998036]
    [INFO] Predicted: 1, Actual: 1
    
    
    Probability: [[  7.38263075e-14   1.59380022e-07   5.03500470e-11   8.31749336e-09
        9.99999285e-01   1.18022217e-10   5.73981871e-16   6.38727442e-07
        5.43443548e-08   1.78733807e-12]]
    Prediction: [ 0.99999928]
    [INFO] Predicted: 4, Actual: 4
    
    
    Probability: [[  3.90606221e-13   2.65865818e-09   5.07930549e-15   6.10185798e-14
        9.24996812e-07   2.24755353e-11   9.99999046e-01   7.02793292e-12
        9.27358911e-14   4.75380313e-09]]
    Prediction: [ 0.99999905]
    [INFO] Predicted: 6, Actual: 6
    
    
    Probability: [[  3.02732041e-11   8.19538036e-05   4.83256599e-06   8.85577798e-01
        3.21182050e-02   2.83969734e-06   1.19332502e-10   7.59462938e-02
        5.74701605e-03   5.21107751e-04]]
    Prediction: [ 0.8855778]
    [INFO] Predicted: 3, Actual: 7
    
    
    Probability: [[  1.23886166e-12   1.64609229e-10   1.52118679e-15   4.17629425e-15
        2.42550442e-08   5.44400611e-15   1.00000000e+00   9.42834027e-13
        1.08054087e-14   7.02130357e-13]]
    Prediction: [ 1.]
    [INFO] Predicted: 6, Actual: 6
    
    
    


```python
fig = plt.figure(figsize= (20,50))
for k in range(10):
    plt.subplot(1, 10, k+1)      
    plt.gca().axes.get_yaxis().set_visible(False)
    plt.gca().axes.get_xaxis().set_visible(False)
    plt.imshow(disp_images[k], cmap='gray')
```


![_config.yml]({{ site.baseurl}}/images/sidecam_output_40_0.png)

-mkc