
## Step 1: Organize imports


```python
from keras.preprocessing.image import ImageDataGenerator
from keras.datasets import cifar10
import numpy as np
```

    Using TensorFlow backend.
    

## Step 2: Define constant/s


```python
IMG_NUM_TO_AUGMENT=1  # Number of images to augment per original image
```

## Step 3: Load dataset 


```python
(X_train, y_train), (X_test, y_test) = cifar10.load_data()
```

## Step 4: Define the ImageDataGenerator object


```python
datagen = ImageDataGenerator(
rotation_range=40,       # 0 to 180
width_shift_range=0.2,   # horizontal-translation
height_shift_range=0.2,  # vertical-translation
zoom_range=0.2,          # random zoom
horizontal_flip=True,
fill_mode='nearest')     # filling in pixels strategy 
```

## Step 5: Generate the images
Note: Create a 'preview' folder within the directory


```python
xtas, ytas = [], []

for i in range(X_train.shape[0]):
    num_aug = 0
    x = X_train[i]    # (3, 32, 32) CIFAR image
    x = x.reshape((1,) + x.shape) # (1, 3, 32, 32)
    
    for x_aug in datagen.flow(x, batch_size=1,
                              save_to_dir='preview', save_prefix='cifar', save_format='jpeg'):
        if num_aug >= IMG_NUM_TO_AUGMENT:
            break
        xtas.append(x_aug[0])
        num_aug += 1
```

## Sample Result



![_config.yml]({{ site.baseurl}}/images/augment_preview.PNG)

-mkc