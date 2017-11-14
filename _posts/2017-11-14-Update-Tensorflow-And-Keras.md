
## Updating Tensorflow and Building Keras from Github

### Step 1: Update Tensorflow using pip

Note: Make sure to activate your conda environment first, e.g. 'activate keras'

```
(tensorflow-gpu) C:\Users\ECE\Downloads>pip install --ignore-installed --upgrade tensorflow-gpu
Collecting tensorflow-gpu
  Downloading tensorflow_gpu-1.4.0-cp35-cp35m-win_amd64.whl (67.6MB)
    100% |################################| 67.6MB 91kB/s
Collecting protobuf>=3.3.0 (from tensorflow-gpu)
  Using cached protobuf-3.4.0-py2.py3-none-any.whl
Collecting wheel>=0.26 (from tensorflow-gpu)
  Using cached wheel-0.30.0-py2.py3-none-any.whl
Collecting enum34>=1.1.6 (from tensorflow-gpu)
  Downloading enum34-1.1.6-py3-none-any.whl
Collecting six>=1.10.0 (from tensorflow-gpu)
  Using cached six-1.11.0-py2.py3-none-any.whl
Collecting tensorflow-tensorboard<0.5.0,>=0.4.0rc1 (from tensorflow-gpu)
  Downloading tensorflow_tensorboard-0.4.0rc2-py3-none-any.whl (1.7MB)
    100% |################################| 1.7MB 136kB/s
Collecting numpy>=1.12.1 (from tensorflow-gpu)
  Using cached numpy-1.13.3-cp35-none-win_amd64.whl
Collecting setuptools (from protobuf>=3.3.0->tensorflow-gpu)
  Downloading setuptools-36.7.2-py2.py3-none-any.whl (482kB)
    100% |################################| 491kB 145kB/s
Collecting markdown>=2.6.8 (from tensorflow-tensorboard<0.5.0,>=0.4.0rc1->tensorflow-gpu)
Collecting bleach==1.5.0 (from tensorflow-tensorboard<0.5.0,>=0.4.0rc1->tensorflow-gpu)
  Using cached bleach-1.5.0-py2.py3-none-any.whl
Collecting html5lib==0.9999999 (from tensorflow-tensorboard<0.5.0,>=0.4.0rc1->tensorflow-gpu)
Collecting werkzeug>=0.11.10 (from tensorflow-tensorboard<0.5.0,>=0.4.0rc1->tensorflow-gpu)
  Using cached Werkzeug-0.12.2-py2.py3-none-any.whl
Installing collected packages: setuptools, six, protobuf, wheel, enum34, markdown, html5lib, bleach, numpy, werkzeug, tensorflow-tensorboard, tensorflow-gpu
Successfully installed bleach-1.5.0 enum34-1.1.6 html5lib-0.9999999 markdown-2.6.9 numpy-1.13.3 protobuf-3.4.0 setuptools-36.7.2 six-1.11.0 tensorflow-gpu-1.4.0 tensorflow-tensorboard-0.4.0rc2 werkzeug-0.12.2 wheel-0.30.0
```

### Step 2: Install Git for Windows
```
https://git-scm.com/download/win
```

### Step 3: Install Keras from the Github source:

#### 3-a Clone keras github (in cmd)
```
git clone https://github.com/fchollet/keras.git
```

#### 3-b Go to keras dir, set-up, and install

```
cd keras
sudo python setup.py installe.g. 
(tensorflow-gpu) D:\workspace\keras>python setup.py install
running install
running bdist_egg
running egg_info
writing top-level names to Keras.egg-info\top_level.txt
writing Keras.egg-info\PKG-INFO
writing dependency_links to Keras.egg-info\dependency_links.txt
writing requirements to Keras.egg-info\requires.txt
reading manifest file 'Keras.egg-info\SOURCES.txt'
reading manifest template 'MANIFEST.in'
writing manifest file 'Keras.egg-info\SOURCES.txt'
installing library code to build\bdist.win-amd64\egg
running install_lib
running build_py
copying keras\losses.py -> build\lib\keras
copying keras\models.py -> build\lib\keras
copying keras\__init__.py -> build\lib\keras
copying keras\applications\inception_resnet_v2.py -> build\lib\keras\applications
copying keras\backend\cntk_backend.py -> build\lib\keras\backend
copying keras\backend\tensorflow_backend.py -> build\lib\keras\backend
copying keras\datasets\boston_housing.py -> build\lib\keras\datasets
copying keras\datasets\cifar10.py -> build\lib\keras\datasets
copying keras\datasets\cifar100.py -> build\lib\keras\datasets
copying keras\datasets\imdb.py -> build\lib\keras\datasets
copying keras\datasets\reuters.py -> build\lib\keras\datasets
copying keras\engine\topology.py -> build\lib\keras\engine
copying keras\engine\training.py -> build\lib\keras\engine
copying keras\layers\cudnn_recurrent.py -> build\lib\keras\layers
copying keras\layers\merge.py -> build\lib\keras\layers
copying keras\layers\recurrent.py -> build\lib\keras\layers
copying keras\layers\wrappers.py -> build\lib\keras\layers
copying keras\preprocessing\image.py -> build\lib\keras\preprocessing
copying keras\utils\data_utils.py -> build\lib\keras\utils
copying keras\utils\generic_utils.py -> build\lib\keras\utils
copying keras\utils\np_utils.py -> build\lib\keras\utils
copying keras\utils\training_utils.py -> build\lib\keras\utils
creating build\bdist.win-amd64\egg
creating build\bdist.win-amd64\egg\keras
copying build\lib\keras\activations.py -> build\bdist.win-amd64\egg\keras
creating build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\imagenet_utils.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\inception_resnet_v2.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\inception_v3.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\mobilenet.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\resnet50.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\vgg16.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\vgg19.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\xception.py -> build\bdist.win-amd64\egg\keras\applications
copying build\lib\keras\applications\__init__.py -> build\bdist.win-amd64\egg\keras\applications
creating build\bdist.win-amd64\egg\keras\backend
copying build\lib\keras\backend\cntk_backend.py -> build\bdist.win-amd64\egg\keras\backend
copying build\lib\keras\backend\common.py -> build\bdist.win-amd64\egg\keras\backend
copying build\lib\keras\backend\tensorflow_backend.py -> build\bdist.win-amd64\egg\keras\backend
copying build\lib\keras\backend\theano_backend.py -> build\bdist.win-amd64\egg\keras\backend
copying build\lib\keras\backend\__init__.py -> build\bdist.win-amd64\egg\keras\backend
copying build\lib\keras\callbacks.py -> build\bdist.win-amd64\egg\keras
copying build\lib\keras\constraints.py -> build\bdist.win-amd64\egg\keras
creating build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\boston_housing.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\cifar.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\cifar10.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\cifar100.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\fashion_mnist.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\imdb.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\mnist.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\reuters.py -> build\bdist.win-amd64\egg\keras\datasets
copying build\lib\keras\datasets\__init__.py -> build\bdist.win-amd64\egg\keras\datasets
creating build\bdist.win-amd64\egg\keras\engine
copying build\lib\keras\engine\topology.py -> build\bdist.win-amd64\egg\keras\engine
copying build\lib\keras\engine\training.py -> build\bdist.win-amd64\egg\keras\engine
copying build\lib\keras\engine\__init__.py -> build\bdist.win-amd64\egg\keras\engine
copying build\lib\keras\initializers.py -> build\bdist.win-amd64\egg\keras
creating build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\advanced_activations.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\convolutional.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\convolutional_recurrent.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\core.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\cudnn_recurrent.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\embeddings.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\local.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\merge.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\noise.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\normalization.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\pooling.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\recurrent.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\wrappers.py -> build\bdist.win-amd64\egg\keras\layers
copying build\lib\keras\layers\__init__.py -> build\bdist.win-amd64\egg\keras\layers
creating build\bdist.win-amd64\egg\keras\legacy
copying build\lib\keras\legacy\interfaces.py -> build\bdist.win-amd64\egg\keras\legacy
copying build\lib\keras\legacy\layers.py -> build\bdist.win-amd64\egg\keras\legacy
copying build\lib\keras\legacy\models.py -> build\bdist.win-amd64\egg\keras\legacy
copying build\lib\keras\legacy\__init__.py -> build\bdist.win-amd64\egg\keras\legacy
copying build\lib\keras\losses.py -> build\bdist.win-amd64\egg\keras
copying build\lib\keras\metrics.py -> build\bdist.win-amd64\egg\keras
copying build\lib\keras\models.py -> build\bdist.win-amd64\egg\keras
copying build\lib\keras\objectives.py -> build\bdist.win-amd64\egg\keras
copying build\lib\keras\optimizers.py -> build\bdist.win-amd64\egg\keras
creating build\bdist.win-amd64\egg\keras\preprocessing
copying build\lib\keras\preprocessing\image.py -> build\bdist.win-amd64\egg\keras\preprocessing
copying build\lib\keras\preprocessing\sequence.py -> build\bdist.win-amd64\egg\keras\preprocessing
copying build\lib\keras\preprocessing\text.py -> build\bdist.win-amd64\egg\keras\preprocessing
copying build\lib\keras\preprocessing\__init__.py -> build\bdist.win-amd64\egg\keras\preprocessing
copying build\lib\keras\regularizers.py -> build\bdist.win-amd64\egg\keras
creating build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\conv_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\data_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\generic_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\io_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\layer_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\np_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\test_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\training_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\vis_utils.py -> build\bdist.win-amd64\egg\keras\utils
copying build\lib\keras\utils\__init__.py -> build\bdist.win-amd64\egg\keras\utils
creating build\bdist.win-amd64\egg\keras\wrappers
copying build\lib\keras\wrappers\scikit_learn.py -> build\bdist.win-amd64\egg\keras\wrappers
copying build\lib\keras\wrappers\__init__.py -> build\bdist.win-amd64\egg\keras\wrappers
copying build\lib\keras\__init__.py -> build\bdist.win-amd64\egg\keras
byte-compiling build\bdist.win-amd64\egg\keras\activations.py to activations.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\imagenet_utils.py to imagenet_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\inception_resnet_v2.py to inception_resnet_v2.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\inception_v3.py to inception_v3.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\mobilenet.py to mobilenet.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\resnet50.py to resnet50.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\vgg16.py to vgg16.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\vgg19.py to vgg19.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\xception.py to xception.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\applications\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\backend\cntk_backend.py to cntk_backend.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\backend\common.py to common.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\backend\tensorflow_backend.py to tensorflow_backend.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\backend\theano_backend.py to theano_backend.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\backend\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\callbacks.py to callbacks.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\constraints.py to constraints.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\boston_housing.py to boston_housing.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\cifar.py to cifar.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\cifar10.py to cifar10.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\cifar100.py to cifar100.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\fashion_mnist.py to fashion_mnist.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\imdb.py to imdb.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\mnist.py to mnist.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\reuters.py to reuters.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\datasets\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\engine\topology.py to topology.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\engine\training.py to training.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\engine\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\initializers.py to initializers.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\advanced_activations.py to advanced_activations.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\convolutional.py to convolutional.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\convolutional_recurrent.py to convolutional_recurrent.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\core.py to core.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\cudnn_recurrent.py to cudnn_recurrent.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\embeddings.py to embeddings.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\local.py to local.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\merge.py to merge.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\noise.py to noise.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\normalization.py to normalization.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\pooling.py to pooling.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\recurrent.py to recurrent.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\wrappers.py to wrappers.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\layers\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\legacy\interfaces.py to interfaces.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\legacy\layers.py to layers.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\legacy\models.py to models.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\legacy\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\losses.py to losses.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\metrics.py to metrics.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\models.py to models.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\objectives.py to objectives.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\optimizers.py to optimizers.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\preprocessing\image.py to image.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\preprocessing\sequence.py to sequence.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\preprocessing\text.py to text.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\preprocessing\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\regularizers.py to regularizers.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\conv_utils.py to conv_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\data_utils.py to data_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\generic_utils.py to generic_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\io_utils.py to io_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\layer_utils.py to layer_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\np_utils.py to np_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\test_utils.py to test_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\training_utils.py to training_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\vis_utils.py to vis_utils.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\utils\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\wrappers\scikit_learn.py to scikit_learn.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\wrappers\__init__.py to __init__.cpython-35.pyc
byte-compiling build\bdist.win-amd64\egg\keras\__init__.py to __init__.cpython-35.pyc
creating build\bdist.win-amd64\egg\EGG-INFO
copying Keras.egg-info\PKG-INFO -> build\bdist.win-amd64\egg\EGG-INFO
copying Keras.egg-info\SOURCES.txt -> build\bdist.win-amd64\egg\EGG-INFO
copying Keras.egg-info\dependency_links.txt -> build\bdist.win-amd64\egg\EGG-INFO
copying Keras.egg-info\requires.txt -> build\bdist.win-amd64\egg\EGG-INFO
copying Keras.egg-info\top_level.txt -> build\bdist.win-amd64\egg\EGG-INFO
zip_safe flag not set; analyzing archive contents...
creating 'dist\Keras-2.1.0-py3.5.egg' and adding 'build\bdist.win-amd64\egg' to it
removing 'build\bdist.win-amd64\egg' (and everything under it)
Processing Keras-2.1.0-py3.5.egg
Copying Keras-2.1.0-py3.5.egg to c:\program files\anaconda3\envs\tensorflow-gpu\lib\site-packages
Removing keras 2.0.8 from easy-install.pth file
Adding Keras 2.1.0 to easy-install.pth file

Installed c:\program files\anaconda3\envs\tensorflow-gpu\lib\site-packages\keras-2.1.0-py3.5.egg
Processing dependencies for Keras==2.1.0
Searching for PyYAML==3.12
Best match: PyYAML 3.12
Adding PyYAML 3.12 to easy-install.pth file

Using c:\program files\anaconda3\envs\tensorflow-gpu\lib\site-packages
Searching for six==1.11.0
Best match: six 1.11.0
Adding six 1.11.0 to easy-install.pth file

Using c:\program files\anaconda3\envs\tensorflow-gpu\lib\site-packages
Searching for scipy==1.0.0
Best match: scipy 1.0.0
Adding scipy 1.0.0 to easy-install.pth file

Using c:\program files\anaconda3\envs\tensorflow-gpu\lib\site-packages
Searching for numpy==1.13.3
Best match: numpy 1.13.3
Adding numpy 1.13.3 to easy-install.pth file

Using c:\program files\anaconda3\envs\tensorflow-gpu\lib\site-packages
Finished processing dependencies for Keras==2.1.0

```
- mkc
