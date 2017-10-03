

```python
import numpy as np
from keras.preprocessing import image
from keras.applications.imagenet_utils import preprocess_input, decode_predictions
from keras.models import Model
from keras.models import load_model
from keras.applications.vgg16 import VGG16
```

## Load image


```python
img_path = "cat.jpg"
img = image.load_img(img_path,target_size=(224,224))
```

## Preprocess the image


```python
array_img = image.img_to_array(img)
array_img = np.expand_dims(array_img, axis=0)
array_img = preprocess_input(array_img)
```

![_config.yml]({{ site.baseurl}}/images/cat_face.jpg)

## Load Pre-built model 


```python
vgg16 = VGG16(weights='imagenet')
```

## Use Model for Prediction  


```python
predictions = vgg16.predict(array_img)
decoded_predictions = decode_predictions(predictions)
print(str(decoded_predictions))
```

    [[('n02356798', 'fox_squirrel', 0.61198878), ('n02123159', 'tiger_cat', 0.048480008), ('n01877812', 'wallaby', 0.035403267), ('n02123045', 'tabby', 0.03323001), ('n02124075', 'Egyptian_cat', 0.031758856)]]
    


```python
decoded_predictions = decode_predictions(predictions, top=3)
print(str(decoded_predictions))
```

    [[('n02356798', 'fox_squirrel', 0.61198878), ('n02123159', 'tiger_cat', 0.048480008), ('n01877812', 'wallaby', 0.035403267)]]
    


```python
decoded_predictions = decode_predictions(predictions, top=1)
print(str(decoded_predictions))
```

    [[('n02356798', 'fox_squirrel', 0.61198878)]]
    
- mkc