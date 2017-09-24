
Step 1: Organize imports


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
                    image = np.array(img, 'uint8') # convert to numpy array
                    images.append(image)
                    labels.append(os.path.basename(folder))
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
```

    Acquiring images...
    No. of images = 500. 
    No. of labels = 500. 
    

Step 4: Plot the first image.


```python
plt.imshow(images[0], cmap='gray')
```




    <matplotlib.image.AxesImage at 0x1f332647ba8>




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



```python
-mkc
```
