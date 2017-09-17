
Deep Learning Intro in Keras (MNIST Tutorial 1)
=============

Assignment 1-a: Deep Learning Hello World! (Baseline)
------------

Objective: To be able to implement a basic MLP in Keras for MNIST Classification

Step 1: Taking care of the imports which includes numpy, datasets, models, layers, optimizers, and utils. <br />
You will also be able to tell if your set-up is correct/complete.


```python
from __future__ import print_function
import numpy as np
from keras.datasets import mnist
from keras.models import Sequential
from keras.models import model_from_json
from keras.layers.core import Dense, Activation
from keras.optimizers import SGD
from keras.utils import np_utils

from matplotlib import pyplot as plt
%matplotlib inline
```

Step 2: Set-up some constants to be utilized in the training/testing of the model


```python
NB_EPOCH = 200
BATCH_SIZE = 128
VERBOSE = 1
NB_CLASSES = 10   # number of outputs = number of digits, i.e. 0,1,2,3,4,5,6,7,8,9
OPTIMIZER = SGD() # Stocastic Gradient Descent optimizer
N_HIDDEN = 128
VALIDATION_SPLIT=0.2 # how much TRAIN dataset is reserved for VALIDATION

np.random.seed(1983)  # for reproducibility
```

Step 3: Load the MNIST Dataset which are shuffled and split between train and test sets <br\>
- X_train is 60000 rows of 28x28 values
- X_test is 10000 rows of 28x28 values


```python
(X_train, y_train), (X_test, y_test) = mnist.load_data()
plt.subplot(1, 4, 1)
plt.imshow(X_train[0])        # first train
plt.subplot(1, 4, 2)
plt.imshow(X_train[59999])    # last train
plt.subplot(1, 4, 3)
plt.imshow(X_test[0])         # first test 
plt.subplot(1, 4, 4)
plt.imshow(X_test[9999])      # last test  
```




    <matplotlib.image.AxesImage at 0x153eb8a6588>





![_config.yml]({{ site.baseurl}}/images/dl_intro_output.png)


Step 4: Preprocess the input data by reshaping it, converting it to float, and normalizing it [0-1].


```python
# reshape
X_train = X_train.reshape(60000, 784)
X_test = X_test.reshape(10000, 784)
X_train = X_train.astype('float32')
X_test = X_test.astype('float32')

# normalize 
X_train /= 255
X_test /= 255

print(X_train.shape, 'train samples')
print(X_test.shape, 'test samples')
```

    (60000, 784) train samples
    (10000, 784) test samples
    

Step 5: Convert class vectors to binary class matrices; One-Hot-Encoding (OHE)


```python
Y_train = np_utils.to_categorical(y_train, NB_CLASSES)
Y_test = np_utils.to_categorical(y_test, NB_CLASSES)
```

Step 6: Create the model: Input:784 ==> Output:10 (with Softmax activation)


```python
model = Sequential()
model.add(Dense(NB_CLASSES, input_shape=(784,)))
model.add(Activation('softmax'))

model.summary()
```

    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    dense_3 (Dense)              (None, 10)                7850      
    _________________________________________________________________
    activation_3 (Activation)    (None, 10)                0         
    =================================================================
    Total params: 7,850
    Trainable params: 7,850
    Non-trainable params: 0
    _________________________________________________________________
    

Step 7: Compile the model with categorical_crossentropy loss function, SGD optimizer, and accuracy metric


```python
model.compile(loss='categorical_crossentropy',
              optimizer=OPTIMIZER,
              metrics=['accuracy'])
```

Step 8: Perform the training with 128 batch size, 200 epochs, and 20 % of the train data used for validation


```python
history = model.fit(X_train, Y_train,
                    batch_size=BATCH_SIZE, epochs=NB_EPOCH,
                    verbose=VERBOSE, validation_split=VALIDATION_SPLIT)
```

    Train on 48000 samples, validate on 12000 samples
    Epoch 1/200
    48000/48000 [==============================] - 1s - loss: 1.4054 - acc: 0.6486 - val_loss: 0.9016 - val_acc: 0.8237
    Epoch 2/200
    48000/48000 [==============================] - 1s - loss: 0.7984 - acc: 0.8248 - val_loss: 0.6587 - val_acc: 0.8559
    Epoch 3/200
    48000/48000 [==============================] - 1s - loss: 0.6451 - acc: 0.8484 - val_loss: 0.5609 - val_acc: 0.8706
    Epoch 4/200
    48000/48000 [==============================] - 1s - loss: 0.5713 - acc: 0.8606 - val_loss: 0.5073 - val_acc: 0.8788
    Epoch 5/200
    48000/48000 [==============================] - 1s - loss: 0.5265 - acc: 0.8677 - val_loss: 0.4729 - val_acc: 0.8836
    Epoch 6/200
    48000/48000 [==============================] - 1s - loss: 0.4958 - acc: 0.8736 - val_loss: 0.4488 - val_acc: 0.8872
    Epoch 7/200
    48000/48000 [==============================] - 1s - loss: 0.4733 - acc: 0.8779 - val_loss: 0.4305 - val_acc: 0.8902
    Epoch 8/200
    48000/48000 [==============================] - 1s - loss: 0.4557 - acc: 0.8812 - val_loss: 0.4166 - val_acc: 0.8918
    Epoch 9/200
    48000/48000 [==============================] - 1s - loss: 0.4417 - acc: 0.8840 - val_loss: 0.4049 - val_acc: 0.8938
    Epoch 10/200
    48000/48000 [==============================] - 1s - loss: 0.4301 - acc: 0.8862 - val_loss: 0.3954 - val_acc: 0.8950
    Epoch 11/200
    48000/48000 [==============================] - 1s - loss: 0.4203 - acc: 0.8885 - val_loss: 0.3873 - val_acc: 0.89720.88
    Epoch 12/200
    48000/48000 [==============================] - 1s - loss: 0.4119 - acc: 0.8899 - val_loss: 0.3804 - val_acc: 0.8987
    Epoch 13/200
    48000/48000 [==============================] - 1s - loss: 0.4045 - acc: 0.8913 - val_loss: 0.3743 - val_acc: 0.9004
    Epoch 14/200
    48000/48000 [==============================] - 1s - loss: 0.3980 - acc: 0.8929 - val_loss: 0.3692 - val_acc: 0.9008
    Epoch 15/200
    48000/48000 [==============================] - 1s - loss: 0.3923 - acc: 0.8937 - val_loss: 0.3645 - val_acc: 0.9023
    Epoch 16/200
    48000/48000 [==============================] - 1s - loss: 0.3872 - acc: 0.8949 - val_loss: 0.3601 - val_acc: 0.9046
    Epoch 17/200
    48000/48000 [==============================] - 1s - loss: 0.3824 - acc: 0.8959 - val_loss: 0.3563 - val_acc: 0.9040
    Epoch 18/200
    48000/48000 [==============================] - 1s - loss: 0.3782 - acc: 0.8963 - val_loss: 0.3528 - val_acc: 0.9048
    Epoch 19/200
    48000/48000 [==============================] - 1s - loss: 0.3743 - acc: 0.8976 - val_loss: 0.3496 - val_acc: 0.9053
    Epoch 20/200
    48000/48000 [==============================] - 1s - loss: 0.3707 - acc: 0.8982 - val_loss: 0.3465 - val_acc: 0.9058
    Epoch 21/200
    48000/48000 [==============================] - 1s - loss: 0.3674 - acc: 0.8990 - val_loss: 0.3441 - val_acc: 0.9065
    Epoch 22/200
    48000/48000 [==============================] - 1s - loss: 0.3643 - acc: 0.8995 - val_loss: 0.3415 - val_acc: 0.9072
    Epoch 23/200
    48000/48000 [==============================] - 1s - loss: 0.3614 - acc: 0.8999 - val_loss: 0.3390 - val_acc: 0.9081
    Epoch 24/200
    48000/48000 [==============================] - 1s - loss: 0.3587 - acc: 0.9005 - val_loss: 0.3369 - val_acc: 0.9086
    Epoch 25/200
    48000/48000 [==============================] - 1s - loss: 0.3562 - acc: 0.9015 - val_loss: 0.3349 - val_acc: 0.9085
    Epoch 26/200
    48000/48000 [==============================] - 1s - loss: 0.3539 - acc: 0.9020 - val_loss: 0.3330 - val_acc: 0.9087
    Epoch 27/200
    48000/48000 [==============================] - 1s - loss: 0.3516 - acc: 0.9026 - val_loss: 0.3310 - val_acc: 0.9088
    Epoch 28/200
    48000/48000 [==============================] - 1s - loss: 0.3495 - acc: 0.9032 - val_loss: 0.3294 - val_acc: 0.9091
    Epoch 29/200
    48000/48000 [==============================] - 1s - loss: 0.3475 - acc: 0.9037 - val_loss: 0.3278 - val_acc: 0.9092
    Epoch 30/200
    48000/48000 [==============================] - 1s - loss: 0.3456 - acc: 0.9044 - val_loss: 0.3263 - val_acc: 0.9097
    Epoch 31/200
    48000/48000 [==============================] - 1s - loss: 0.3438 - acc: 0.9047 - val_loss: 0.3248 - val_acc: 0.9098
    Epoch 32/200
    48000/48000 [==============================] - 1s - loss: 0.3421 - acc: 0.9052 - val_loss: 0.3234 - val_acc: 0.9103
    Epoch 33/200
    48000/48000 [==============================] - 1s - loss: 0.3405 - acc: 0.9054 - val_loss: 0.3221 - val_acc: 0.9105
    Epoch 34/200
    48000/48000 [==============================] - 1s - loss: 0.3388 - acc: 0.9059 - val_loss: 0.3208 - val_acc: 0.9107
    Epoch 35/200
    48000/48000 [==============================] - 1s - loss: 0.3374 - acc: 0.9065 - val_loss: 0.3197 - val_acc: 0.9112
    Epoch 36/200
    48000/48000 [==============================] - 1s - loss: 0.3359 - acc: 0.9069 - val_loss: 0.3185 - val_acc: 0.9115
    Epoch 37/200
    48000/48000 [==============================] - 1s - loss: 0.3346 - acc: 0.9071 - val_loss: 0.3174 - val_acc: 0.9115
    Epoch 38/200
    48000/48000 [==============================] - 1s - loss: 0.3332 - acc: 0.9073 - val_loss: 0.3164 - val_acc: 0.9118
    Epoch 39/200
    48000/48000 [==============================] - 1s - loss: 0.3319 - acc: 0.9078 - val_loss: 0.3153 - val_acc: 0.9123
    Epoch 40/200
    48000/48000 [==============================] - 1s - loss: 0.3307 - acc: 0.9079 - val_loss: 0.3144 - val_acc: 0.9123
    Epoch 41/200
    48000/48000 [==============================] - 1s - loss: 0.3295 - acc: 0.9085 - val_loss: 0.3135 - val_acc: 0.9125
    Epoch 42/200
    48000/48000 [==============================] - 1s - loss: 0.3284 - acc: 0.9086 - val_loss: 0.3125 - val_acc: 0.9128
    Epoch 43/200
    48000/48000 [==============================] - 1s - loss: 0.3273 - acc: 0.9087 - val_loss: 0.3117 - val_acc: 0.9127
    Epoch 44/200
    48000/48000 [==============================] - 1s - loss: 0.3262 - acc: 0.9093 - val_loss: 0.3109 - val_acc: 0.9128
    Epoch 45/200
    48000/48000 [==============================] - 1s - loss: 0.3252 - acc: 0.9094 - val_loss: 0.3100 - val_acc: 0.9131
    Epoch 46/200
    48000/48000 [==============================] - 1s - loss: 0.3242 - acc: 0.9098 - val_loss: 0.3092 - val_acc: 0.9139
    Epoch 47/200
    48000/48000 [==============================] - 1s - loss: 0.3233 - acc: 0.9097 - val_loss: 0.3086 - val_acc: 0.9132
    Epoch 48/200
    48000/48000 [==============================] - 1s - loss: 0.3223 - acc: 0.9101 - val_loss: 0.3078 - val_acc: 0.9144
    Epoch 49/200
    48000/48000 [==============================] - 1s - loss: 0.3214 - acc: 0.9108 - val_loss: 0.3072 - val_acc: 0.9137
    Epoch 50/200
    48000/48000 [==============================] - 1s - loss: 0.3206 - acc: 0.9107 - val_loss: 0.3064 - val_acc: 0.9139
    Epoch 51/200
    48000/48000 [==============================] - 1s - loss: 0.3197 - acc: 0.9109 - val_loss: 0.3059 - val_acc: 0.9141
    Epoch 52/200
    48000/48000 [==============================] - 1s - loss: 0.3189 - acc: 0.9112 - val_loss: 0.3052 - val_acc: 0.9147
    Epoch 53/200
    48000/48000 [==============================] - 1s - loss: 0.3181 - acc: 0.9114 - val_loss: 0.3045 - val_acc: 0.9148
    Epoch 54/200
    48000/48000 [==============================] - 1s - loss: 0.3173 - acc: 0.9117 - val_loss: 0.3039 - val_acc: 0.9148
    Epoch 55/200
    48000/48000 [==============================] - 1s - loss: 0.3165 - acc: 0.9120 - val_loss: 0.3033 - val_acc: 0.9153
    Epoch 56/200
    48000/48000 [==============================] - 1s - loss: 0.3158 - acc: 0.9122 - val_loss: 0.3027 - val_acc: 0.9147
    Epoch 57/200
    48000/48000 [==============================] - 1s - loss: 0.3150 - acc: 0.9124 - val_loss: 0.3023 - val_acc: 0.9159
    Epoch 58/200
    48000/48000 [==============================] - 1s - loss: 0.3143 - acc: 0.9125 - val_loss: 0.3018 - val_acc: 0.9152
    Epoch 59/200
    48000/48000 [==============================] - 1s - loss: 0.3137 - acc: 0.9126 - val_loss: 0.3011 - val_acc: 0.9155
    Epoch 60/200
    48000/48000 [==============================] - 1s - loss: 0.3130 - acc: 0.9131 - val_loss: 0.3006 - val_acc: 0.9157
    Epoch 61/200
    48000/48000 [==============================] - 1s - loss: 0.3124 - acc: 0.9135 - val_loss: 0.3001 - val_acc: 0.9161
    Epoch 62/200
    48000/48000 [==============================] - 1s - loss: 0.3117 - acc: 0.9135 - val_loss: 0.2997 - val_acc: 0.9157
    Epoch 63/200
    48000/48000 [==============================] - 1s - loss: 0.3111 - acc: 0.9134 - val_loss: 0.2992 - val_acc: 0.9163
    Epoch 64/200
    48000/48000 [==============================] - 1s - loss: 0.3105 - acc: 0.9139 - val_loss: 0.2988 - val_acc: 0.9167
    Epoch 65/200
    48000/48000 [==============================] - 1s - loss: 0.3098 - acc: 0.9140 - val_loss: 0.2983 - val_acc: 0.9162
    Epoch 66/200
    48000/48000 [==============================] - 1s - loss: 0.3093 - acc: 0.9140 - val_loss: 0.2978 - val_acc: 0.9171
    Epoch 67/200
    48000/48000 [==============================] - 1s - loss: 0.3088 - acc: 0.9143 - val_loss: 0.2974 - val_acc: 0.9169
    Epoch 68/200
    48000/48000 [==============================] - 1s - loss: 0.3082 - acc: 0.9141 - val_loss: 0.2970 - val_acc: 0.9172
    Epoch 69/200
    48000/48000 [==============================] - 1s - loss: 0.3076 - acc: 0.9144 - val_loss: 0.2967 - val_acc: 0.9172
    Epoch 70/200
    48000/48000 [==============================] - 1s - loss: 0.3071 - acc: 0.9148 - val_loss: 0.2963 - val_acc: 0.9176
    Epoch 71/200
    48000/48000 [==============================] - 1s - loss: 0.3066 - acc: 0.9147 - val_loss: 0.2958 - val_acc: 0.9170
    Epoch 72/200
    48000/48000 [==============================] - 1s - loss: 0.3060 - acc: 0.9149 - val_loss: 0.2955 - val_acc: 0.9177
    Epoch 73/200
    48000/48000 [==============================] - 1s - loss: 0.3056 - acc: 0.9148 - val_loss: 0.2951 - val_acc: 0.9175
    Epoch 74/200
    48000/48000 [==============================] - 1s - loss: 0.3051 - acc: 0.9150 - val_loss: 0.2947 - val_acc: 0.9175
    Epoch 75/200
    48000/48000 [==============================] - 1s - loss: 0.3046 - acc: 0.9154 - val_loss: 0.2943 - val_acc: 0.9175
    Epoch 76/200
    48000/48000 [==============================] - 1s - loss: 0.3041 - acc: 0.9154 - val_loss: 0.2939 - val_acc: 0.9180
    Epoch 77/200
    48000/48000 [==============================] - 1s - loss: 0.3037 - acc: 0.9157 - val_loss: 0.2936 - val_acc: 0.9182
    Epoch 78/200
    48000/48000 [==============================] - 1s - loss: 0.3032 - acc: 0.9156 - val_loss: 0.2933 - val_acc: 0.9182
    Epoch 79/200
    48000/48000 [==============================] - 1s - loss: 0.3027 - acc: 0.9160 - val_loss: 0.2930 - val_acc: 0.9184
    Epoch 80/200
    48000/48000 [==============================] - 1s - loss: 0.3023 - acc: 0.9160 - val_loss: 0.2926 - val_acc: 0.9184
    Epoch 81/200
    48000/48000 [==============================] - 1s - loss: 0.3019 - acc: 0.9163 - val_loss: 0.2924 - val_acc: 0.9184
    Epoch 82/200
    48000/48000 [==============================] - 1s - loss: 0.3014 - acc: 0.9160 - val_loss: 0.2920 - val_acc: 0.9187
    Epoch 83/200
    48000/48000 [==============================] - 1s - loss: 0.3010 - acc: 0.9162 - val_loss: 0.2917 - val_acc: 0.9188
    Epoch 84/200
    48000/48000 [==============================] - 1s - loss: 0.3006 - acc: 0.9166 - val_loss: 0.2915 - val_acc: 0.9184
    Epoch 85/200
    48000/48000 [==============================] - 1s - loss: 0.3002 - acc: 0.9165 - val_loss: 0.2912 - val_acc: 0.9185
    Epoch 86/200
    48000/48000 [==============================] - 1s - loss: 0.2999 - acc: 0.9167 - val_loss: 0.2909 - val_acc: 0.9186
    Epoch 87/200
    48000/48000 [==============================] - 1s - loss: 0.2994 - acc: 0.9165 - val_loss: 0.2906 - val_acc: 0.9188
    Epoch 88/200
    48000/48000 [==============================] - 1s - loss: 0.2990 - acc: 0.9165 - val_loss: 0.2904 - val_acc: 0.9188
    Epoch 89/200
    48000/48000 [==============================] - 1s - loss: 0.2987 - acc: 0.9169 - val_loss: 0.2900 - val_acc: 0.9187
    Epoch 90/200
    48000/48000 [==============================] - 1s - loss: 0.2983 - acc: 0.9173 - val_loss: 0.2898 - val_acc: 0.9192
    Epoch 91/200
    48000/48000 [==============================] - 1s - loss: 0.2979 - acc: 0.9170 - val_loss: 0.2895 - val_acc: 0.9192
    Epoch 92/200
    48000/48000 [==============================] - 1s - loss: 0.2976 - acc: 0.9172 - val_loss: 0.2893 - val_acc: 0.9192
    Epoch 93/200
    48000/48000 [==============================] - 1s - loss: 0.2973 - acc: 0.9173 - val_loss: 0.2890 - val_acc: 0.9192
    Epoch 94/200
    48000/48000 [==============================] - 1s - loss: 0.2968 - acc: 0.9177 - val_loss: 0.2889 - val_acc: 0.9193
    Epoch 95/200
    48000/48000 [==============================] - 1s - loss: 0.2965 - acc: 0.9173 - val_loss: 0.2886 - val_acc: 0.9198
    Epoch 96/200
    48000/48000 [==============================] - 1s - loss: 0.2962 - acc: 0.9175 - val_loss: 0.2883 - val_acc: 0.9194
    Epoch 97/200
    48000/48000 [==============================] - 1s - loss: 0.2959 - acc: 0.9178 - val_loss: 0.2880 - val_acc: 0.9195
    Epoch 98/200
    48000/48000 [==============================] - 1s - loss: 0.2956 - acc: 0.9179 - val_loss: 0.2878 - val_acc: 0.9196
    Epoch 99/200
    48000/48000 [==============================] - 1s - loss: 0.2952 - acc: 0.9178 - val_loss: 0.2877 - val_acc: 0.9201
    Epoch 100/200
    48000/48000 [==============================] - 1s - loss: 0.2949 - acc: 0.9179 - val_loss: 0.2874 - val_acc: 0.92080.917
    Epoch 101/200
    48000/48000 [==============================] - 1s - loss: 0.2946 - acc: 0.9180 - val_loss: 0.2871 - val_acc: 0.9200
    Epoch 102/200
    48000/48000 [==============================] - 1s - loss: 0.2943 - acc: 0.9179 - val_loss: 0.2869 - val_acc: 0.9199
    Epoch 103/200
    48000/48000 [==============================] - 1s - loss: 0.2939 - acc: 0.9180 - val_loss: 0.2868 - val_acc: 0.9199
    Epoch 104/200
    48000/48000 [==============================] - 1s - loss: 0.2937 - acc: 0.9181 - val_loss: 0.2866 - val_acc: 0.9202
    Epoch 105/200
    48000/48000 [==============================] - 1s - loss: 0.2934 - acc: 0.9181 - val_loss: 0.2863 - val_acc: 0.9198
    Epoch 106/200
    48000/48000 [==============================] - 1s - loss: 0.2931 - acc: 0.9186 - val_loss: 0.2862 - val_acc: 0.9201
    Epoch 107/200
    48000/48000 [==============================] - 1s - loss: 0.2928 - acc: 0.9186 - val_loss: 0.2859 - val_acc: 0.9203
    Epoch 108/200
    48000/48000 [==============================] - 1s - loss: 0.2925 - acc: 0.9183 - val_loss: 0.2856 - val_acc: 0.9204
    Epoch 109/200
    48000/48000 [==============================] - 1s - loss: 0.2922 - acc: 0.9187 - val_loss: 0.2855 - val_acc: 0.9202
    Epoch 110/200
    48000/48000 [==============================] - 1s - loss: 0.2920 - acc: 0.9188 - val_loss: 0.2853 - val_acc: 0.9202
    Epoch 111/200
    48000/48000 [==============================] - 1s - loss: 0.2917 - acc: 0.9185 - val_loss: 0.2852 - val_acc: 0.9203
    Epoch 112/200
    48000/48000 [==============================] - 1s - loss: 0.2914 - acc: 0.9187 - val_loss: 0.2849 - val_acc: 0.9201
    Epoch 113/200
    48000/48000 [==============================] - 1s - loss: 0.2912 - acc: 0.9191 - val_loss: 0.2847 - val_acc: 0.9203
    Epoch 114/200
    48000/48000 [==============================] - 1s - loss: 0.2909 - acc: 0.9190 - val_loss: 0.2845 - val_acc: 0.9205
    Epoch 115/200
    48000/48000 [==============================] - 1s - loss: 0.2906 - acc: 0.9189 - val_loss: 0.2844 - val_acc: 0.9207
    Epoch 116/200
    48000/48000 [==============================] - 1s - loss: 0.2904 - acc: 0.9190 - val_loss: 0.2842 - val_acc: 0.9204
    Epoch 117/200
    48000/48000 [==============================] - 1s - loss: 0.2901 - acc: 0.9191 - val_loss: 0.2840 - val_acc: 0.9211
    Epoch 118/200
    48000/48000 [==============================] - 1s - loss: 0.2898 - acc: 0.9192 - val_loss: 0.2840 - val_acc: 0.9208
    Epoch 119/200
    48000/48000 [==============================] - 1s - loss: 0.2896 - acc: 0.9193 - val_loss: 0.2838 - val_acc: 0.9207
    Epoch 120/200
    48000/48000 [==============================] - 1s - loss: 0.2894 - acc: 0.9193 - val_loss: 0.2835 - val_acc: 0.9210
    Epoch 121/200
    48000/48000 [==============================] - 1s - loss: 0.2891 - acc: 0.9192 - val_loss: 0.2833 - val_acc: 0.9209
    Epoch 122/200
    48000/48000 [==============================] - 1s - loss: 0.2889 - acc: 0.9196 - val_loss: 0.2832 - val_acc: 0.9212
    Epoch 123/200
    48000/48000 [==============================] - 1s - loss: 0.2886 - acc: 0.9195 - val_loss: 0.2831 - val_acc: 0.9208
    Epoch 124/200
    48000/48000 [==============================] - 1s - loss: 0.2884 - acc: 0.9194 - val_loss: 0.2829 - val_acc: 0.9210
    Epoch 125/200
    48000/48000 [==============================] - 1s - loss: 0.2882 - acc: 0.9196 - val_loss: 0.2827 - val_acc: 0.9212
    Epoch 126/200
    48000/48000 [==============================] - 1s - loss: 0.2880 - acc: 0.9196 - val_loss: 0.2826 - val_acc: 0.9210
    Epoch 127/200
    48000/48000 [==============================] - 1s - loss: 0.2877 - acc: 0.9198 - val_loss: 0.2825 - val_acc: 0.9216
    Epoch 128/200
    48000/48000 [==============================] - 1s - loss: 0.2875 - acc: 0.9199 - val_loss: 0.2823 - val_acc: 0.9212
    Epoch 129/200
    48000/48000 [==============================] - 1s - loss: 0.2873 - acc: 0.9199 - val_loss: 0.2822 - val_acc: 0.9212
    Epoch 130/200
    48000/48000 [==============================] - 1s - loss: 0.2871 - acc: 0.9201 - val_loss: 0.2820 - val_acc: 0.9216
    Epoch 131/200
    48000/48000 [==============================] - 1s - loss: 0.2868 - acc: 0.9202 - val_loss: 0.2819 - val_acc: 0.9214
    Epoch 132/200
    48000/48000 [==============================] - 1s - loss: 0.2867 - acc: 0.9200 - val_loss: 0.2817 - val_acc: 0.9216
    Epoch 133/200
    48000/48000 [==============================] - 1s - loss: 0.2864 - acc: 0.9199 - val_loss: 0.2815 - val_acc: 0.9217
    Epoch 134/200
    48000/48000 [==============================] - 1s - loss: 0.2862 - acc: 0.9202 - val_loss: 0.2814 - val_acc: 0.9213
    Epoch 135/200
    48000/48000 [==============================] - 1s - loss: 0.2860 - acc: 0.9202 - val_loss: 0.2813 - val_acc: 0.9216
    Epoch 136/200
    48000/48000 [==============================] - 1s - loss: 0.2858 - acc: 0.9204 - val_loss: 0.2812 - val_acc: 0.9214
    Epoch 137/200
    48000/48000 [==============================] - 1s - loss: 0.2856 - acc: 0.9203 - val_loss: 0.2811 - val_acc: 0.9216
    Epoch 138/200
    48000/48000 [==============================] - 1s - loss: 0.2854 - acc: 0.9202 - val_loss: 0.2809 - val_acc: 0.9216
    Epoch 139/200
    48000/48000 [==============================] - 1s - loss: 0.2852 - acc: 0.9205 - val_loss: 0.2808 - val_acc: 0.9218
    Epoch 140/200
    48000/48000 [==============================] - 1s - loss: 0.2850 - acc: 0.9206 - val_loss: 0.2807 - val_acc: 0.9217
    Epoch 141/200
    48000/48000 [==============================] - 1s - loss: 0.2848 - acc: 0.9208 - val_loss: 0.2806 - val_acc: 0.9217
    Epoch 142/200
    48000/48000 [==============================] - 1s - loss: 0.2846 - acc: 0.9206 - val_loss: 0.2804 - val_acc: 0.9220
    Epoch 143/200
    48000/48000 [==============================] - 1s - loss: 0.2844 - acc: 0.9203 - val_loss: 0.2802 - val_acc: 0.9219
    Epoch 144/200
    48000/48000 [==============================] - 1s - loss: 0.2842 - acc: 0.9208 - val_loss: 0.2801 - val_acc: 0.9222
    Epoch 145/200
    48000/48000 [==============================] - 1s - loss: 0.2841 - acc: 0.9207 - val_loss: 0.2800 - val_acc: 0.9221
    Epoch 146/200
    48000/48000 [==============================] - 1s - loss: 0.2839 - acc: 0.9209 - val_loss: 0.2799 - val_acc: 0.9222
    Epoch 147/200
    48000/48000 [==============================] - 1s - loss: 0.2837 - acc: 0.9209 - val_loss: 0.2798 - val_acc: 0.9221
    Epoch 148/200
    48000/48000 [==============================] - 1s - loss: 0.2835 - acc: 0.9209 - val_loss: 0.2797 - val_acc: 0.9225
    Epoch 149/200
    48000/48000 [==============================] - 1s - loss: 0.2833 - acc: 0.9209 - val_loss: 0.2796 - val_acc: 0.9222
    Epoch 150/200
    48000/48000 [==============================] - 1s - loss: 0.2831 - acc: 0.9212 - val_loss: 0.2795 - val_acc: 0.9220
    Epoch 151/200
    48000/48000 [==============================] - 1s - loss: 0.2830 - acc: 0.9210 - val_loss: 0.2793 - val_acc: 0.9219
    Epoch 152/200
    48000/48000 [==============================] - 1s - loss: 0.2828 - acc: 0.9208 - val_loss: 0.2793 - val_acc: 0.9222
    Epoch 153/200
    48000/48000 [==============================] - 1s - loss: 0.2826 - acc: 0.9213 - val_loss: 0.2792 - val_acc: 0.9222
    Epoch 154/200
    48000/48000 [==============================] - 1s - loss: 0.2824 - acc: 0.9212 - val_loss: 0.2790 - val_acc: 0.9224
    Epoch 155/200
    48000/48000 [==============================] - 1s - loss: 0.2823 - acc: 0.9213 - val_loss: 0.2789 - val_acc: 0.9226
    Epoch 156/200
    48000/48000 [==============================] - 1s - loss: 0.2821 - acc: 0.9214 - val_loss: 0.2787 - val_acc: 0.9224
    Epoch 157/200
    48000/48000 [==============================] - 1s - loss: 0.2819 - acc: 0.9216 - val_loss: 0.2787 - val_acc: 0.9223
    Epoch 158/200
    48000/48000 [==============================] - 1s - loss: 0.2818 - acc: 0.9214 - val_loss: 0.2786 - val_acc: 0.9223
    Epoch 159/200
    48000/48000 [==============================] - 1s - loss: 0.2816 - acc: 0.9216 - val_loss: 0.2785 - val_acc: 0.9225
    Epoch 160/200
    48000/48000 [==============================] - 1s - loss: 0.2815 - acc: 0.9217 - val_loss: 0.2784 - val_acc: 0.9223
    Epoch 161/200
    48000/48000 [==============================] - 1s - loss: 0.2813 - acc: 0.9217 - val_loss: 0.2783 - val_acc: 0.9224
    Epoch 162/200
    48000/48000 [==============================] - 1s - loss: 0.2811 - acc: 0.9219 - val_loss: 0.2782 - val_acc: 0.9224
    Epoch 163/200
    48000/48000 [==============================] - 1s - loss: 0.2809 - acc: 0.9215 - val_loss: 0.2781 - val_acc: 0.9226
    Epoch 164/200
    48000/48000 [==============================] - 1s - loss: 0.2808 - acc: 0.9218 - val_loss: 0.2780 - val_acc: 0.9227
    Epoch 165/200
    48000/48000 [==============================] - 1s - loss: 0.2806 - acc: 0.9218 - val_loss: 0.2780 - val_acc: 0.9224
    Epoch 166/200
    48000/48000 [==============================] - 1s - loss: 0.2805 - acc: 0.9218 - val_loss: 0.2777 - val_acc: 0.9228
    Epoch 167/200
    48000/48000 [==============================] - 1s - loss: 0.2803 - acc: 0.9219 - val_loss: 0.2777 - val_acc: 0.9227
    Epoch 168/200
    48000/48000 [==============================] - 1s - loss: 0.2801 - acc: 0.9219 - val_loss: 0.2777 - val_acc: 0.9223
    Epoch 169/200
    48000/48000 [==============================] - 1s - loss: 0.2800 - acc: 0.9223 - val_loss: 0.2775 - val_acc: 0.9228
    Epoch 170/200
    48000/48000 [==============================] - 1s - loss: 0.2799 - acc: 0.9221 - val_loss: 0.2774 - val_acc: 0.9227
    Epoch 171/200
    48000/48000 [==============================] - 1s - loss: 0.2797 - acc: 0.9220 - val_loss: 0.2773 - val_acc: 0.9232
    Epoch 172/200
    48000/48000 [==============================] - 1s - loss: 0.2796 - acc: 0.9220 - val_loss: 0.2773 - val_acc: 0.9230
    Epoch 173/200
    48000/48000 [==============================] - 1s - loss: 0.2794 - acc: 0.9221 - val_loss: 0.2772 - val_acc: 0.9227
    Epoch 174/200
    48000/48000 [==============================] - 1s - loss: 0.2793 - acc: 0.9221 - val_loss: 0.2771 - val_acc: 0.9229
    Epoch 175/200
    48000/48000 [==============================] - 1s - loss: 0.2791 - acc: 0.9225 - val_loss: 0.2770 - val_acc: 0.9232
    Epoch 176/200
    48000/48000 [==============================] - 1s - loss: 0.2790 - acc: 0.9225 - val_loss: 0.2769 - val_acc: 0.9232
    Epoch 177/200
    48000/48000 [==============================] - 1s - loss: 0.2788 - acc: 0.9222 - val_loss: 0.2768 - val_acc: 0.9227
    Epoch 178/200
    48000/48000 [==============================] - 1s - loss: 0.2787 - acc: 0.9225 - val_loss: 0.2767 - val_acc: 0.9229
    Epoch 179/200
    48000/48000 [==============================] - 1s - loss: 0.2786 - acc: 0.9224 - val_loss: 0.2767 - val_acc: 0.9229
    Epoch 180/200
    48000/48000 [==============================] - 1s - loss: 0.2784 - acc: 0.9226 - val_loss: 0.2766 - val_acc: 0.9230
    Epoch 181/200
    48000/48000 [==============================] - 1s - loss: 0.2783 - acc: 0.9228 - val_loss: 0.2765 - val_acc: 0.9232
    Epoch 182/200
    48000/48000 [==============================] - 1s - loss: 0.2781 - acc: 0.9226 - val_loss: 0.2764 - val_acc: 0.9232
    Epoch 183/200
    48000/48000 [==============================] - 1s - loss: 0.2780 - acc: 0.9227 - val_loss: 0.2764 - val_acc: 0.9230
    Epoch 184/200
    48000/48000 [==============================] - 1s - loss: 0.2779 - acc: 0.9227 - val_loss: 0.2762 - val_acc: 0.9231
    Epoch 185/200
    48000/48000 [==============================] - 1s - loss: 0.2777 - acc: 0.9229 - val_loss: 0.2762 - val_acc: 0.9230
    Epoch 186/200
    48000/48000 [==============================] - 1s - loss: 0.2776 - acc: 0.9229 - val_loss: 0.2761 - val_acc: 0.9233
    Epoch 187/200
    48000/48000 [==============================] - 1s - loss: 0.2775 - acc: 0.9228 - val_loss: 0.2760 - val_acc: 0.9232
    Epoch 188/200
    48000/48000 [==============================] - 1s - loss: 0.2774 - acc: 0.9229 - val_loss: 0.2759 - val_acc: 0.9233
    Epoch 189/200
    48000/48000 [==============================] - 1s - loss: 0.2772 - acc: 0.9229 - val_loss: 0.2758 - val_acc: 0.9232
    Epoch 190/200
    48000/48000 [==============================] - 1s - loss: 0.2771 - acc: 0.9230 - val_loss: 0.2758 - val_acc: 0.9232
    Epoch 191/200
    48000/48000 [==============================] - 1s - loss: 0.2770 - acc: 0.9230 - val_loss: 0.2757 - val_acc: 0.9232
    Epoch 192/200
    48000/48000 [==============================] - 1s - loss: 0.2768 - acc: 0.9229 - val_loss: 0.2756 - val_acc: 0.9236
    Epoch 193/200
    48000/48000 [==============================] - 1s - loss: 0.2767 - acc: 0.9230 - val_loss: 0.2756 - val_acc: 0.9234
    Epoch 194/200
    48000/48000 [==============================] - 1s - loss: 0.2766 - acc: 0.9229 - val_loss: 0.2755 - val_acc: 0.9234
    Epoch 195/200
    48000/48000 [==============================] - 1s - loss: 0.2765 - acc: 0.9231 - val_loss: 0.2754 - val_acc: 0.9237
    Epoch 196/200
    48000/48000 [==============================] - 1s - loss: 0.2764 - acc: 0.9232 - val_loss: 0.2754 - val_acc: 0.9237
    Epoch 197/200
    48000/48000 [==============================] - 1s - loss: 0.2762 - acc: 0.9230 - val_loss: 0.2753 - val_acc: 0.9237
    Epoch 198/200
    48000/48000 [==============================] - 1s - loss: 0.2761 - acc: 0.9231 - val_loss: 0.2752 - val_acc: 0.9242
    Epoch 199/200
    48000/48000 [==============================] - 1s - loss: 0.2760 - acc: 0.9233 - val_loss: 0.2751 - val_acc: 0.9238
    Epoch 200/200
    48000/48000 [==============================] - 1s - loss: 0.2758 - acc: 0.9231 - val_loss: 0.2751 - val_acc: 0.9237
    

Step 9: Evaluate the model on the test dataset (10,000 images)


```python
score = model.evaluate(X_test, Y_test, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

     9856/10000 [============================>.] - ETA: 0s
    Test score: 0.276437405369
    Test accuracy: 0.9228
    

[Optional] Step 10: Save the model (serialized) to JSON


```python
model_json = model.to_json()
with open("model.json", "w") as json_file:
    json_file.write(model_json)
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 7252-C405
    
     Directory of C:\Users\cobalt\workspace
    
    09/17/2017  12:53 PM    <DIR>          .
    09/17/2017  12:53 PM    <DIR>          ..
    09/16/2017  11:08 PM    <DIR>          .ipynb_checkpoints
    01/07/2017  12:22 PM    <DIR>          .metadata
    09/17/2017  12:53 PM            48,179 DeepLearningHelloWorld.ipynb
    01/08/2017  10:52 AM    <DIR>          Hello
    01/08/2017  10:52 AM    <DIR>          Hellocpp11
    01/09/2017  04:45 PM    <DIR>          HelloOpenCV
    09/17/2017  12:53 PM               723 model.json
    01/07/2017  12:22 PM    <DIR>          RemoteSystemsTempFiles
                   2 File(s)         48,902 bytes
                   8 Dir(s)  199,888,052,224 bytes free
    

[Optional] Step 11: Save the model weights


```python
model.save_weights("model.h5")
%ls
```

     Volume in drive C is Windows
     Volume Serial Number is 7252-C405
    
     Directory of C:\Users\cobalt\workspace
    
    09/17/2017  01:07 PM    <DIR>          .
    09/17/2017  01:07 PM    <DIR>          ..
    09/16/2017  11:08 PM    <DIR>          .ipynb_checkpoints
    01/07/2017  12:22 PM    <DIR>          .metadata
    09/17/2017  12:55 PM            47,588 DeepLearningHelloWorld.ipynb
    01/08/2017  10:52 AM    <DIR>          Hello
    01/08/2017  10:52 AM    <DIR>          Hellocpp11
    01/09/2017  04:45 PM    <DIR>          HelloOpenCV
    09/17/2017  01:07 PM            42,528 model.h5
    09/17/2017  12:53 PM               723 model.json
    01/07/2017  12:22 PM    <DIR>          RemoteSystemsTempFiles
                   3 File(s)         90,839 bytes
                   8 Dir(s)  199,868,473,344 bytes free
    

[Optional] Step 12: Load the saved model


```python
json_file = open('model.json', 'r')
loaded_model_json = json_file.read()
json_file.close()
loaded_model = model_from_json(loaded_model_json)
loaded_model.load_weights("model.h5")
```

[Optional] Step 13: Compile and evaluate loaded model


```python
loaded_model.compile(loss='categorical_crossentropy',
              optimizer=OPTIMIZER,
              metrics=['accuracy'])
score = loaded_model.evaluate(X_test, Y_test, verbose=VERBOSE)
print("\nTest score:", score[0])
print('Test accuracy:', score[1])
```

     9440/10000 [===========================>..] - ETA: 0s
    Test score: 0.276437405369
    Test accuracy: 0.9228
    
- mkc