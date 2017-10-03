
##  Face recognition with Xception
(Inspired by: https://gogul09.github.io/software/flower-recognition-deep-learning)

## Step 1: Organize imports


```python
from keras import backend as K
K.set_image_dim_ordering('tf') # this is for Xception
from keras.applications.vgg16 import VGG16, preprocess_input
from keras.applications.vgg19 import VGG19, preprocess_input
from keras.applications.xception import Xception, preprocess_input 
from keras.applications.resnet50 import ResNet50, preprocess_input
from keras.applications.inception_v3 import InceptionV3, preprocess_input
from keras.preprocessing import image
from keras.models import Model
import _pickle   
```

    Using TensorFlow backend.
    


```python
from sklearn.preprocessing import LabelEncoder
import numpy as np  
import glob
import cv2
import h5py
import os
import json
import datetime
import time 
import sys 
import argparse 
from PIL import Image
from matplotlib import pyplot as plt
%matplotlib inline
```

[OPTIONAL] Handle the GPU partition


```python
import tensorflow as tf
from keras.backend.tensorflow_backend import set_session
gpu_config = tf.ConfigProto()
gpu_config.gpu_options.per_process_gpu_memory_fraction = 0.3
set_session(tf.Session(config=gpu_config))
```

## Step 2: Load configuration written in .json


```python
!more config.json
```

    {
            "model"                 : "resnet50",
            "weights"               : "imagenet",
            "include_top"           : false,
    
            "train_path"            : "C:/Users/ECE/workspace/SimpleFaceRecognitionDemo/front_database/train",
            "features_path"         : "output/resnet50/features.h5",
            "labels_path"           : "output/resnet50/labels.h5",
            "results"                   : "output/resnet50/results.txt",
            "classifier_path"       : "output/resnet50/classifier.cpickle",
    
            "test_size"             : 0.10,
            "seed"                      : 1983,
            "num_classes"           : 10
    }
    


```python
with open('config.json') as f:    
	config = json.load(f)

# config variables
model_name		= config["model"]
weights 		= config["weights"]
include_top 	= config["include_top"]
train_path 		= config["train_path"]
features_path	= config["features_path"]
labels_path 	= config["labels_path"]
test_size		= config["test_size"]
results			= config["results"]
```

## Step 3: Create output directory


```python
!mkdir "output\xception"
```

    A subdirectory or file output\xception already exists.
    

## Step 4: Create base model 


```python
if model_name == "vgg16":
	base_model = VGG16(weights=weights)
	model = Model(input=base_model.input, output=base_model.get_layer('fc1').output)
	image_size = (224, 224)
elif model_name == "vgg19":
	base_model = VGG19(weights=weights)
	model = Model(input=base_model.input, output=base_model.get_layer('fc1').output)
	image_size = (224, 224)
elif model_name == "resnet50":
	base_model = ResNet50(weights=weights)
	model = Model(input=base_model.input, output=base_model.get_layer('avg_pool').output)
	image_size = (224, 224)
elif model_name == "inceptionv3":
	base_model = InceptionV3(weights=weights)
	model = Model(input=base_model.input, output=base_model.get_layer('mixed7').output)
	image_size = (299, 299)
elif model_name == "xception":
	base_model = Xception(weights=weights)
	model = Model(input=base_model.input, output=base_model.get_layer('avg_pool').output)
	image_size = (299, 299)
else:
	base_model = None
```

    C:\Program Files\Anaconda3\envs\tensorflow-gpu\lib\site-packages\ipykernel_launcher.py:11: UserWarning: Update your `Model` call to the Keras 2 API: `Model(inputs=Tensor("in..., outputs=Tensor("av...)`
      # This is added back by InteractiveShellApp.init_path()
    

## Step 5: Acquire the labels from the train path


```python
train_labels = os.listdir(train_path)
le = LabelEncoder()
le.fit([tl for tl in train_labels])
```




    LabelEncoder()



## Step 6: Acquire the features


```python
features = []
labels   = []

i = 0
for label in train_labels:
	cur_path = train_path + "/" + label
	for image_path in glob.glob(cur_path + "/*.pgm"):
		img = image.load_img(image_path, target_size=image_size)
		x = image.img_to_array(img)
		x = np.expand_dims(x, axis=0)
		x = preprocess_input(x)
		feature = model.predict(x)
		flat = feature.flatten()
		features.append(flat)
		labels.append(label)
		print("[INFO] processed - %s" % i)
		i += 1
	print("[INFO] completed label - %s" % label)
```

    [INFO] processed - 0
    [INFO] processed - 1
    [INFO] processed - 2
    [INFO] processed - 3
    [INFO] processed - 4
    [INFO] processed - 5
    [INFO] processed - 6
    [INFO] processed - 7
    [INFO] processed - 8
    [INFO] processed - 9
    [INFO] processed - 10
    [INFO] processed - 11
    [INFO] processed - 12
    [INFO] processed - 13
    [INFO] processed - 14
    [INFO] processed - 15
    [INFO] processed - 16
    [INFO] processed - 17
    [INFO] processed - 18
    [INFO] processed - 19
    [INFO] processed - 20
    [INFO] processed - 21
    [INFO] processed - 22
    [INFO] processed - 23
    [INFO] processed - 24
    [INFO] processed - 25
    [INFO] processed - 26
    [INFO] processed - 27
    [INFO] processed - 28
    [INFO] processed - 29
    [INFO] processed - 30
    [INFO] processed - 31
    [INFO] processed - 32
    [INFO] processed - 33
    [INFO] processed - 34
    [INFO] processed - 35
    [INFO] processed - 36
    [INFO] processed - 37
    [INFO] processed - 38
    [INFO] processed - 39
    [INFO] processed - 40
    [INFO] processed - 41
    [INFO] processed - 42
    [INFO] processed - 43
    [INFO] processed - 44
    [INFO] processed - 45
    [INFO] processed - 46
    [INFO] processed - 47
    [INFO] processed - 48
    [INFO] processed - 49
    [INFO] completed label - person_0_abad
    [INFO] processed - 50
    [INFO] processed - 51
    [INFO] processed - 52
    [INFO] processed - 53
    [INFO] processed - 54
    [INFO] processed - 55
    [INFO] processed - 56
    [INFO] processed - 57
    [INFO] processed - 58
    [INFO] processed - 59
    [INFO] processed - 60
    [INFO] processed - 61
    [INFO] processed - 62
    [INFO] processed - 63
    [INFO] processed - 64
    [INFO] processed - 65
    [INFO] processed - 66
    [INFO] processed - 67
    [INFO] processed - 68
    [INFO] processed - 69
    [INFO] processed - 70
    [INFO] processed - 71
    [INFO] processed - 72
    [INFO] processed - 73
    [INFO] processed - 74
    [INFO] processed - 75
    [INFO] processed - 76
    [INFO] processed - 77
    [INFO] processed - 78
    [INFO] processed - 79
    [INFO] processed - 80
    [INFO] processed - 81
    [INFO] processed - 82
    [INFO] processed - 83
    [INFO] processed - 84
    [INFO] processed - 85
    [INFO] processed - 86
    [INFO] processed - 87
    [INFO] processed - 88
    [INFO] processed - 89
    [INFO] processed - 90
    [INFO] processed - 91
    [INFO] processed - 92
    [INFO] processed - 93
    [INFO] processed - 94
    [INFO] processed - 95
    [INFO] processed - 96
    [INFO] processed - 97
    [INFO] processed - 98
    [INFO] processed - 99
    [INFO] completed label - person_1_agui
    [INFO] processed - 100
    [INFO] processed - 101
    [INFO] processed - 102
    [INFO] processed - 103
    [INFO] processed - 104
    [INFO] processed - 105
    [INFO] processed - 106
    [INFO] processed - 107
    [INFO] processed - 108
    [INFO] processed - 109
    [INFO] processed - 110
    [INFO] processed - 111
    [INFO] processed - 112
    [INFO] processed - 113
    [INFO] processed - 114
    [INFO] processed - 115
    [INFO] processed - 116
    [INFO] processed - 117
    [INFO] processed - 118
    [INFO] processed - 119
    [INFO] processed - 120
    [INFO] processed - 121
    [INFO] processed - 122
    [INFO] processed - 123
    [INFO] processed - 124
    [INFO] processed - 125
    [INFO] processed - 126
    [INFO] processed - 127
    [INFO] processed - 128
    [INFO] processed - 129
    [INFO] processed - 130
    [INFO] processed - 131
    [INFO] processed - 132
    [INFO] processed - 133
    [INFO] processed - 134
    [INFO] processed - 135
    [INFO] processed - 136
    [INFO] processed - 137
    [INFO] processed - 138
    [INFO] processed - 139
    [INFO] processed - 140
    [INFO] processed - 141
    [INFO] processed - 142
    [INFO] processed - 143
    [INFO] processed - 144
    [INFO] processed - 145
    [INFO] processed - 146
    [INFO] processed - 147
    [INFO] processed - 148
    [INFO] processed - 149
    [INFO] completed label - person_2_cans
    [INFO] processed - 150
    [INFO] processed - 151
    [INFO] processed - 152
    [INFO] processed - 153
    [INFO] processed - 154
    [INFO] processed - 155
    [INFO] processed - 156
    [INFO] processed - 157
    [INFO] processed - 158
    [INFO] processed - 159
    [INFO] processed - 160
    [INFO] processed - 161
    [INFO] processed - 162
    [INFO] processed - 163
    [INFO] processed - 164
    [INFO] processed - 165
    [INFO] processed - 166
    [INFO] processed - 167
    [INFO] processed - 168
    [INFO] processed - 169
    [INFO] processed - 170
    [INFO] processed - 171
    [INFO] processed - 172
    [INFO] processed - 173
    [INFO] processed - 174
    [INFO] processed - 175
    [INFO] processed - 176
    [INFO] processed - 177
    [INFO] processed - 178
    [INFO] processed - 179
    [INFO] processed - 180
    [INFO] processed - 181
    [INFO] processed - 182
    [INFO] processed - 183
    [INFO] processed - 184
    [INFO] processed - 185
    [INFO] processed - 186
    [INFO] processed - 187
    [INFO] processed - 188
    [INFO] processed - 189
    [INFO] processed - 190
    [INFO] processed - 191
    [INFO] processed - 192
    [INFO] processed - 193
    [INFO] processed - 194
    [INFO] processed - 195
    [INFO] processed - 196
    [INFO] processed - 197
    [INFO] processed - 198
    [INFO] processed - 199
    [INFO] completed label - person_3_degu
    [INFO] processed - 200
    [INFO] processed - 201
    [INFO] processed - 202
    [INFO] processed - 203
    [INFO] processed - 204
    [INFO] processed - 205
    [INFO] processed - 206
    [INFO] processed - 207
    [INFO] processed - 208
    [INFO] processed - 209
    [INFO] processed - 210
    [INFO] processed - 211
    [INFO] processed - 212
    [INFO] processed - 213
    [INFO] processed - 214
    [INFO] processed - 215
    [INFO] processed - 216
    [INFO] processed - 217
    [INFO] processed - 218
    [INFO] processed - 219
    [INFO] processed - 220
    [INFO] processed - 221
    [INFO] processed - 222
    [INFO] processed - 223
    [INFO] processed - 224
    [INFO] processed - 225
    [INFO] processed - 226
    [INFO] processed - 227
    [INFO] processed - 228
    [INFO] processed - 229
    [INFO] processed - 230
    [INFO] processed - 231
    [INFO] processed - 232
    [INFO] processed - 233
    [INFO] processed - 234
    [INFO] processed - 235
    [INFO] processed - 236
    [INFO] processed - 237
    [INFO] processed - 238
    [INFO] processed - 239
    [INFO] processed - 240
    [INFO] processed - 241
    [INFO] processed - 242
    [INFO] processed - 243
    [INFO] processed - 244
    [INFO] processed - 245
    [INFO] processed - 246
    [INFO] processed - 247
    [INFO] processed - 248
    [INFO] processed - 249
    [INFO] completed label - person_4_hato
    [INFO] processed - 250
    [INFO] processed - 251
    [INFO] processed - 252
    [INFO] processed - 253
    [INFO] processed - 254
    [INFO] processed - 255
    [INFO] processed - 256
    [INFO] processed - 257
    [INFO] processed - 258
    [INFO] processed - 259
    [INFO] processed - 260
    [INFO] processed - 261
    [INFO] processed - 262
    [INFO] processed - 263
    [INFO] processed - 264
    [INFO] processed - 265
    [INFO] processed - 266
    [INFO] processed - 267
    [INFO] processed - 268
    [INFO] processed - 269
    [INFO] processed - 270
    [INFO] processed - 271
    [INFO] processed - 272
    [INFO] processed - 273
    [INFO] processed - 274
    [INFO] processed - 275
    [INFO] processed - 276
    [INFO] processed - 277
    [INFO] processed - 278
    [INFO] processed - 279
    [INFO] processed - 280
    [INFO] processed - 281
    [INFO] processed - 282
    [INFO] processed - 283
    [INFO] processed - 284
    [INFO] processed - 285
    [INFO] processed - 286
    [INFO] processed - 287
    [INFO] processed - 288
    [INFO] processed - 289
    [INFO] processed - 290
    [INFO] processed - 291
    [INFO] processed - 292
    [INFO] processed - 293
    [INFO] processed - 294
    [INFO] processed - 295
    [INFO] processed - 296
    [INFO] processed - 297
    [INFO] processed - 298
    [INFO] processed - 299
    [INFO] completed label - person_5_libb
    [INFO] processed - 300
    [INFO] processed - 301
    [INFO] processed - 302
    [INFO] processed - 303
    [INFO] processed - 304
    [INFO] processed - 305
    [INFO] processed - 306
    [INFO] processed - 307
    [INFO] processed - 308
    [INFO] processed - 309
    [INFO] processed - 310
    [INFO] processed - 311
    [INFO] processed - 312
    [INFO] processed - 313
    [INFO] processed - 314
    [INFO] processed - 315
    [INFO] processed - 316
    [INFO] processed - 317
    [INFO] processed - 318
    [INFO] processed - 319
    [INFO] processed - 320
    [INFO] processed - 321
    [INFO] processed - 322
    [INFO] processed - 323
    [INFO] processed - 324
    [INFO] processed - 325
    [INFO] processed - 326
    [INFO] processed - 327
    [INFO] processed - 328
    [INFO] processed - 329
    [INFO] processed - 330
    [INFO] processed - 331
    [INFO] processed - 332
    [INFO] processed - 333
    [INFO] processed - 334
    [INFO] processed - 335
    [INFO] processed - 336
    [INFO] processed - 337
    [INFO] processed - 338
    [INFO] processed - 339
    [INFO] processed - 340
    [INFO] processed - 341
    [INFO] processed - 342
    [INFO] processed - 343
    [INFO] processed - 344
    [INFO] processed - 345
    [INFO] processed - 346
    [INFO] processed - 347
    [INFO] processed - 348
    [INFO] processed - 349
    [INFO] completed label - person_6_palo
    [INFO] processed - 350
    [INFO] processed - 351
    [INFO] processed - 352
    [INFO] processed - 353
    [INFO] processed - 354
    [INFO] processed - 355
    [INFO] processed - 356
    [INFO] processed - 357
    [INFO] processed - 358
    [INFO] processed - 359
    [INFO] processed - 360
    [INFO] processed - 361
    [INFO] processed - 362
    [INFO] processed - 363
    [INFO] processed - 364
    [INFO] processed - 365
    [INFO] processed - 366
    [INFO] processed - 367
    [INFO] processed - 368
    [INFO] processed - 369
    [INFO] processed - 370
    [INFO] processed - 371
    [INFO] processed - 372
    [INFO] processed - 373
    [INFO] processed - 374
    [INFO] processed - 375
    [INFO] processed - 376
    [INFO] processed - 377
    [INFO] processed - 378
    [INFO] processed - 379
    [INFO] processed - 380
    [INFO] processed - 381
    [INFO] processed - 382
    [INFO] processed - 383
    [INFO] processed - 384
    [INFO] processed - 385
    [INFO] processed - 386
    [INFO] processed - 387
    [INFO] processed - 388
    [INFO] processed - 389
    [INFO] processed - 390
    [INFO] processed - 391
    [INFO] processed - 392
    [INFO] processed - 393
    [INFO] processed - 394
    [INFO] processed - 395
    [INFO] processed - 396
    [INFO] processed - 397
    [INFO] processed - 398
    [INFO] processed - 399
    [INFO] completed label - person_7_prim
    [INFO] processed - 400
    [INFO] processed - 401
    [INFO] processed - 402
    [INFO] processed - 403
    [INFO] processed - 404
    [INFO] processed - 405
    [INFO] processed - 406
    [INFO] processed - 407
    [INFO] processed - 408
    [INFO] processed - 409
    [INFO] processed - 410
    [INFO] processed - 411
    [INFO] processed - 412
    [INFO] processed - 413
    [INFO] processed - 414
    [INFO] processed - 415
    [INFO] processed - 416
    [INFO] processed - 417
    [INFO] processed - 418
    [INFO] processed - 419
    [INFO] processed - 420
    [INFO] processed - 421
    [INFO] processed - 422
    [INFO] processed - 423
    [INFO] processed - 424
    [INFO] processed - 425
    [INFO] processed - 426
    [INFO] processed - 427
    [INFO] processed - 428
    [INFO] processed - 429
    [INFO] processed - 430
    [INFO] processed - 431
    [INFO] processed - 432
    [INFO] processed - 433
    [INFO] processed - 434
    [INFO] processed - 435
    [INFO] processed - 436
    [INFO] processed - 437
    [INFO] processed - 438
    [INFO] processed - 439
    [INFO] processed - 440
    [INFO] processed - 441
    [INFO] processed - 442
    [INFO] processed - 443
    [INFO] processed - 444
    [INFO] processed - 445
    [INFO] processed - 446
    [INFO] processed - 447
    [INFO] processed - 448
    [INFO] processed - 449
    [INFO] completed label - person_8_rosa
    [INFO] processed - 450
    [INFO] processed - 451
    [INFO] processed - 452
    [INFO] processed - 453
    [INFO] processed - 454
    [INFO] processed - 455
    [INFO] processed - 456
    [INFO] processed - 457
    [INFO] processed - 458
    [INFO] processed - 459
    [INFO] processed - 460
    [INFO] processed - 461
    [INFO] processed - 462
    [INFO] processed - 463
    [INFO] processed - 464
    [INFO] processed - 465
    [INFO] processed - 466
    [INFO] processed - 467
    [INFO] processed - 468
    [INFO] processed - 469
    [INFO] processed - 470
    [INFO] processed - 471
    [INFO] processed - 472
    [INFO] processed - 473
    [INFO] processed - 474
    [INFO] processed - 475
    [INFO] processed - 476
    [INFO] processed - 477
    [INFO] processed - 478
    [INFO] processed - 479
    [INFO] processed - 480
    [INFO] processed - 481
    [INFO] processed - 482
    [INFO] processed - 483
    [INFO] processed - 484
    [INFO] processed - 485
    [INFO] processed - 486
    [INFO] processed - 487
    [INFO] processed - 488
    [INFO] processed - 489
    [INFO] processed - 490
    [INFO] processed - 491
    [INFO] processed - 492
    [INFO] processed - 493
    [INFO] processed - 494
    [INFO] processed - 495
    [INFO] processed - 496
    [INFO] processed - 497
    [INFO] processed - 498
    [INFO] processed - 499
    [INFO] completed label - person_9_venu
    

## Step 7: Encode the labels using LabelEncoder


```python
targetNames = np.unique(labels)
le = LabelEncoder()
le_labels = le.fit_transform(labels)

print ("[STATUS] training labels: \n %s" % le_labels)
print ("[STATUS] training labels shape: %s" % le_labels.shape)
```

    [STATUS] training labels: 
     [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
     3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4
     4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 5 5 5 5 5 5 5 5 5
     5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5
     5 5 5 5 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
     6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7
     7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 8 8 8 8 8 8 8
     8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
     8 8 8 8 8 8 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9
     9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9]
    [STATUS] training labels shape: 500
    

## Step 8: Save features and labels


```python
h5f_data = h5py.File(features_path, 'w')
h5f_data.create_dataset('dataset_1', data=np.array(features))

h5f_label = h5py.File(labels_path, 'w')
h5f_label.create_dataset('dataset_1', data=np.array(le_labels))

h5f_data.close()
h5f_label.close()
```


```python
# Checking the output
!ls C:\Users\ECE\workspace\SimpleFaceRecognitionDemo\output\xception
```

    
    (tensorflow-gpu) C:\Users\ECE\workspace\SimpleFaceRecognitionDemo>dir C:\Users\ECE\workspace\SimpleFaceRecognitionDemo\output\xception  
     Volume in drive C is Windows
     Volume Serial Number is 4E4C-DF71
    
     Directory of C:\Users\ECE\workspace\SimpleFaceRecognitionDemo\output\xception
    
    03/10/2017  01:26 PM    <DIR>          .
    03/10/2017  01:26 PM    <DIR>          ..
    03/10/2017  01:26 PM           164,798 classifier.cpickle
    03/10/2017  11:59 AM         4,098,048 features.h5
    03/10/2017  11:59 AM             6,048 labels.h5
    03/10/2017  01:20 PM               687 results.txt
                   4 File(s)      4,269,581 bytes
                   2 Dir(s)  33,202,286,592 bytes free
    

## Note: The new files are features.h5 and labels.h5.  


```python
from sklearn.metrics import classification_report
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix
import h5py
import seaborn as sns
```

## Step 9: Load the user configs for training/testing


```python
with open('config.json') as f:    
	config = json.load(f)
```


```python
!more config.json
```

    {
            "model"                 : "resnet50",
            "weights"               : "imagenet",
            "include_top"           : false,
    
            "train_path"            : "C:/Users/ECE/workspace/SimpleFaceRecognitionDemo/front_database/train",
            "features_path"         : "output/resnet50/features.h5",
            "labels_path"           : "output/resnet50/labels.h5",
            "results"                   : "output/resnet50/results.txt",
            "classifier_path"       : "output/resnet50/classifier.cpickle",
    
            "test_size"             : 0.10,
            "seed"                      : 1983,
            "num_classes"           : 10
    }
    


```python
test_size 	= config["test_size"]
seed 		= config["seed"]
features_path	= config["features_path"]
labels_path 	= config["labels_path"]
results  	= config["results"]
classifier_path = config["classifier_path"]
train_path 	= config["train_path"]
num_classes	= config["num_classes"]
```

## Step 10: Import features and labels


```python
h5f_data = h5py.File(features_path, 'r')
h5f_label = h5py.File(labels_path, 'r')

features_string = h5f_data['dataset_1']
labels_string   = h5f_label['dataset_1']

features = np.array(features_string)
labels   = np.array(labels_string)

h5f_data.close()
h5f_label.close()

print ("[INFO] features shape: {}".format(features.shape))
print ("[INFO] labels shape: {}".format(labels.shape))
```

    [INFO] features shape: (500, 2048)
    [INFO] labels shape: (500,)
    

## Step 11: Split the training and testing data 


```python
(trainData, testData, trainLabels, testLabels) = train_test_split(np.array(features), 
                                                                  np.array(labels),
                                                                  test_size=test_size, 
                                                                  random_state=seed)

print ("[INFO] splitted train and test data...")
print ("[INFO] train data  : {}".format(trainData.shape))
print ("[INFO] test data   : {}".format(testData.shape))
print ("[INFO] train labels: {}".format(trainLabels.shape))
print ("[INFO] test labels : {}".format(testLabels.shape))
```

    [INFO] splitted train and test data...
    [INFO] train data  : (450, 2048)
    [INFO] test data   : (50, 2048)
    [INFO] train labels: (450,)
    [INFO] test labels : (50,)
    

## Step 12: Use Logistic Regression Model


```python
model = LogisticRegression(random_state=seed)
```


```python
model.fit(trainData, trainLabels)
```




    LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=1983, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)



## Step 13: Evaluate the model


```python
f = open(results, "w")
rank_1 = 0
rank_5 = 0

# loop over test data
for (label, features) in zip(testLabels, testData):
	# predict the probability of each class label and take the top-5 class labels
	predictions = model.predict_proba(np.atleast_2d(features))[0]
	predictions = np.argsort(predictions)[::-1][:5]

	# rank-1 prediction increment
	if label == predictions[0]:
		rank_1 += 1

	# rank-5 prediction increment
	if label in predictions:
		rank_5 += 1

# convert accuracies to percentages
rank_1 = (rank_1 / float(len(testLabels))) * 100
rank_5 = (rank_5 / float(len(testLabels))) * 100

# write the accuracies to file
f.write("rank-1: {:.2f}\n".format(rank_1))
f.write("rank-5: {:.2f}\n\n".format(rank_5))

# evaluate the model of test data
preds = model.predict(testData)

# write the classification report to file
f.write("{}\n".format(classification_report(testLabels, preds)))
f.close()
```


```python
!more C:\Users\ECE\workspace\SimpleFaceRecognitionDemo\output\xception\results.txt
```

    rank-1: 98.00
    rank-5: 100.00
    
                 precision    recall  f1-score   support
    
              0       1.00      1.00      1.00         5
              1       1.00      1.00      1.00         5
              2       1.00      1.00      1.00         4
              3       1.00      1.00      1.00         7
              4       1.00      1.00      1.00         3
              5       1.00      0.75      0.86         4
              6       1.00      1.00      1.00         6
              7       0.83      1.00      0.91         5
              8       1.00      1.00      1.00         7
              9       1.00      1.00      1.00         4
    
    avg / total       0.98      0.98      0.98        50
    
    

## Step 14: Save the model


```python
f = open(classifier_path, "wb")
f.write(_pickle.dumps(model))
f.close()
```

## Step 15: Plot the Confusion Matrix


```python
labels = sorted(list(os.listdir(train_path)))

# plot the confusion matrix
cm = confusion_matrix(testLabels, preds)
fig = plt.figure(figsize=(12, 12))
plt.rcParams.update({'font.size': 24})
sns.heatmap(cm, 
            annot=True,
            cmap=plt.cm.Blues)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/transfer_learn1_output_41_0.png)

-mkc