# Basic Logistic Regression of HDF5 Data using Caffe by Yangqing Jia

While Caffe is made for deep networks it can likewise represent "shallow" models like logistic regression for classification. 

We'll do simple logistic regression on synthetic data that we'll generate and save to HDF5 to feed vectors to Caffe. 

Once that model is done, we'll add layers to improve accuracy. 

That's what Caffe is about: 

1. define a model 
2. experiment
3. deploy.


    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # Make sure that caffe is on the python path:
    caffe_root = '../'  # this file is expected to be in {caffe_root}/examples
    import sys
    sys.path.insert(0, caffe_root + 'python')
    
    import caffe
    
    import os
    import h5py
    import shutil
    import tempfile
    
    # You may need to 'pip install scikit-learn'
    import sklearn
    import sklearn.datasets
    import sklearn.linear_model

# Data

Synthesize a dataset of 10,000 4-vectors for binary classification with 2 informative features and 2 noise features.


    X, y = sklearn.datasets.make_classification(
        n_samples=10000, n_features=4, n_redundant=0, n_informative=2, 
        n_clusters_per_class=2, hypercube=False, random_state=0
    )
    
    # Split into train and test
    X, Xt, y, yt = sklearn.cross_validation.train_test_split(X, y)
    
    # Visualize sample of the data
    ind = np.random.permutation(X.shape[0])[:1000]
    df = pd.DataFrame(X[ind])
    _ = pd.scatter_matrix(df, figsize=(9, 9), diagonal='kde', marker='o', s=40, alpha=.4, c=y[ind])


![_config.yml]({{ site.baseurl}}/images/hdf5_classification-mirror_3_0.png)


# Learn and Evaluate in scikit-learn

Learn and evaluate scikit-learn's logistic regression with stochastic gradient descent (SGD) training. Time and check the classifier's accuracy.


    # Train and test the scikit-learn SGD logistic regression.
    clf = sklearn.linear_model.SGDClassifier(
        loss='log', n_iter=1000, penalty='l2', alpha=1e-3, class_weight='auto')
    
    %timeit clf.fit(X, y)
    yt_pred = clf.predict(Xt)
    print('Accuracy: {:.3f}'.format(sklearn.metrics.accuracy_score(yt, yt_pred)))

    1 loops, best of 3: 471 ms per loop
    Accuracy: 0.760


Save the dataset to HDF5 for loading in Caffe.


    # Write out the data to HDF5 files in a temp directory.
    # This file is assumed to be caffe_root/examples/hdf5_classification.ipynb
    dirname = os.path.abspath('./hdf5_classification/data')
    if not os.path.exists(dirname):
        os.makedirs(dirname)
    
    train_filename = os.path.join(dirname, 'train.h5')
    test_filename = os.path.join(dirname, 'test.h5')
    
    # HDF5DataLayer source should be a file containing a list of HDF5 filenames.
    # To show this off, we'll list the same data file twice.
    with h5py.File(train_filename, 'w') as f:
        f['data'] = X
        f['label'] = y.astype(np.float32)
    with open(os.path.join(dirname, 'train.txt'), 'w') as f:
        f.write(train_filename + '\n')
        f.write(train_filename + '\n')
        
    # HDF5 is pretty efficient, but can be further compressed.
    comp_kwargs = {'compression': 'gzip', 'compression_opts': 1}
    with h5py.File(test_filename, 'w') as f:
        f.create_dataset('data', data=Xt, **comp_kwargs)
        f.create_dataset('label', data=yt.astype(np.float32), **comp_kwargs)
    with open(os.path.join(dirname, 'test.txt'), 'w') as f:
        f.write(test_filename + '\n')

# Learn and Evaluate in Caffe

Learn and evaluate logistic regression in Caffe.


    def learn_and_test(solver_file):
        caffe.set_mode_cpu()
        solver = caffe.get_solver(solver_file)
        solver.solve()
    
        accuracy = 0
        test_iters = int(len(Xt) / solver.test_nets[0].blobs['data'].num)
        for i in range(test_iters):
            solver.test_nets[0].forward()
            accuracy += solver.test_nets[0].blobs['accuracy'].data
        accuracy /= test_iters
        return accuracy
    
    %timeit learn_and_test('hdf5_classification/solver.prototxt')
    acc = learn_and_test('hdf5_classification/solver.prototxt')
    print("Accuracy: {:.3f}".format(acc))

    10 loops, best of 3: 132 ms per loop
    Accuracy: 0.759


Do the same through the command line interface for detailed output on the model and solving.


    # # Troubleshooting for shared library path.
    # from pprint import pprint as p
    # sys.path.insert(0, '/root/anaconda/lib')
    # p(sys.path)

Troubleshooting for shared library path.


    import os
    LD_PATH = os.environ['LD_LIBRARY_PATH']
    print(LD_PATH)

    :/usr/local/lib:/usr/local/lib:/usr/local/lib:/usr/local/lib:/usr/local/lib:/usr/local/lib


If using Anaconda Python distribution; Add '/root/anaconda/lib/' to shared library path: LD_LIBRARY_PATH 


    os.environ['LD_LIBRARY_PATH']='/root/anaconda/lib:LD_PATH'


    !../build/tools/caffe train -solver hdf5_classification/solver.prototxt

    ../build/tools/caffe: /root/anaconda/lib/libtiff.so.5: no version information available (required by /usr/local/lib/libopencv_imgcodecs.so.3.0)
    I0529 17:21:44.545850 25389 caffe.cpp:117] Use CPU.
    I0529 17:21:44.546200 25389 caffe.cpp:121] Starting Optimization
    I0529 17:21:44.546303 25389 solver.cpp:32] Initializing solver from parameters: 
    test_iter: 250
    test_interval: 1000
    base_lr: 0.01
    display: 1000
    max_iter: 10000
    lr_policy: "step"
    gamma: 0.1
    momentum: 0.9
    weight_decay: 0.0005
    stepsize: 5000
    snapshot: 10000
    snapshot_prefix: "hdf5_classification/data/train"
    solver_mode: CPU
    net: "hdf5_classification/train_val.prototxt"
    I0529 17:21:44.546399 25389 solver.cpp:70] Creating training net from net file: hdf5_classification/train_val.prototxt
    I0529 17:21:44.546592 25389 net.cpp:287] The NetState phase (0) differed from the phase (1) specified by a rule in layer data
    I0529 17:21:44.546617 25389 net.cpp:287] The NetState phase (0) differed from the phase (1) specified by a rule in layer accuracy
    I0529 17:21:44.546695 25389 net.cpp:42] Initializing net from parameters: 
    name: "LogisticRegressionNet"
    state {
      phase: TRAIN
    }
    layer {
      name: "data"
      type: "HDF5Data"
      top: "data"
      top: "label"
      include {
        phase: TRAIN
      }
      hdf5_data_param {
        source: "hdf5_classification/data/train.txt"
        batch_size: 10
      }
    }
    layer {
      name: "fc1"
      type: "InnerProduct"
      bottom: "data"
      top: "fc1"
      param {
        lr_mult: 1
        decay_mult: 1
      }
      param {
        lr_mult: 2
        decay_mult: 0
      }
      inner_product_param {
        num_output: 2
        weight_filler {
          type: "gaussian"
          std: 0.01
        }
        bias_filler {
          type: "constant"
          value: 0
        }
      }
    }
    layer {
      name: "loss"
      type: "SoftmaxWithLoss"
      bottom: "fc1"
      bottom: "label"
      top: "loss"
    }
    I0529 17:21:44.546926 25389 layer_factory.hpp:74] Creating layer data
    I0529 17:21:44.546948 25389 net.cpp:90] Creating Layer data
    I0529 17:21:44.546965 25389 net.cpp:368] data -> data
    I0529 17:21:44.546999 25389 net.cpp:368] data -> label
    I0529 17:21:44.547031 25389 net.cpp:120] Setting up data
    I0529 17:21:44.547046 25389 hdf5_data_layer.cpp:80] Loading list of HDF5 filenames from: hdf5_classification/data/train.txt
    I0529 17:21:44.547124 25389 hdf5_data_layer.cpp:94] Number of HDF5 files: 2
    I0529 17:21:44.549160 25389 net.cpp:127] Top shape: 10 4 (40)
    I0529 17:21:44.549201 25389 net.cpp:127] Top shape: 10 (10)
    I0529 17:21:44.549216 25389 layer_factory.hpp:74] Creating layer fc1
    I0529 17:21:44.549247 25389 net.cpp:90] Creating Layer fc1
    I0529 17:21:44.549260 25389 net.cpp:410] fc1 <- data
    I0529 17:21:44.549283 25389 net.cpp:368] fc1 -> fc1
    I0529 17:21:44.549302 25389 net.cpp:120] Setting up fc1
    I0529 17:21:44.549724 25389 net.cpp:127] Top shape: 10 2 (20)
    I0529 17:21:44.549751 25389 layer_factory.hpp:74] Creating layer loss
    I0529 17:21:44.549767 25389 net.cpp:90] Creating Layer loss
    I0529 17:21:44.549779 25389 net.cpp:410] loss <- fc1
    I0529 17:21:44.549790 25389 net.cpp:410] loss <- label
    I0529 17:21:44.549803 25389 net.cpp:368] loss -> loss
    I0529 17:21:44.549820 25389 net.cpp:120] Setting up loss
    I0529 17:21:44.549836 25389 layer_factory.hpp:74] Creating layer loss
    I0529 17:21:44.549866 25389 net.cpp:127] Top shape: (1)
    I0529 17:21:44.549878 25389 net.cpp:129]     with loss weight 1
    I0529 17:21:44.549904 25389 net.cpp:192] loss needs backward computation.
    I0529 17:21:44.549917 25389 net.cpp:192] fc1 needs backward computation.
    I0529 17:21:44.549929 25389 net.cpp:194] data does not need backward computation.
    I0529 17:21:44.549939 25389 net.cpp:235] This network produces output loss
    I0529 17:21:44.549952 25389 net.cpp:482] Collecting Learning Rate and Weight Decay.
    I0529 17:21:44.549964 25389 net.cpp:247] Network initialization done.
    I0529 17:21:44.549974 25389 net.cpp:248] Memory required for data: 284
    I0529 17:21:44.550119 25389 solver.cpp:154] Creating test net (#0) specified by net file: hdf5_classification/train_val.prototxt
    I0529 17:21:44.550144 25389 net.cpp:287] The NetState phase (1) differed from the phase (0) specified by a rule in layer data
    I0529 17:21:44.550202 25389 net.cpp:42] Initializing net from parameters: 
    name: "LogisticRegressionNet"
    state {
      phase: TEST
    }
    layer {
      name: "data"
      type: "HDF5Data"
      top: "data"
      top: "label"
      include {
        phase: TEST
      }
      hdf5_data_param {
        source: "hdf5_classification/data/test.txt"
        batch_size: 10
      }
    }
    layer {
      name: "fc1"
      type: "InnerProduct"
      bottom: "data"
      top: "fc1"
      param {
        lr_mult: 1
        decay_mult: 1
      }
      param {
        lr_mult: 2
        decay_mult: 0
      }
      inner_product_param {
        num_output: 2
        weight_filler {
          type: "gaussian"
          std: 0.01
        }
        bias_filler {
          type: "constant"
          value: 0
        }
      }
    }
    layer {
      name: "loss"
      type: "SoftmaxWithLoss"
      bottom: "fc1"
      bottom: "label"
      top: "loss"
    }
    layer {
      name: "accuracy"
      type: "Accuracy"
      bottom: "fc1"
      bottom: "label"
      top: "accuracy"
      include {
        phase: TEST
      }
    }
    I0529 17:21:44.550461 25389 layer_factory.hpp:74] Creating layer data
    I0529 17:21:44.550477 25389 net.cpp:90] Creating Layer data
    I0529 17:21:44.550489 25389 net.cpp:368] data -> data
    I0529 17:21:44.550506 25389 net.cpp:368] data -> label
    I0529 17:21:44.550521 25389 net.cpp:120] Setting up data
    I0529 17:21:44.550531 25389 hdf5_data_layer.cpp:80] Loading list of HDF5 filenames from: hdf5_classification/data/test.txt
    I0529 17:21:44.550555 25389 hdf5_data_layer.cpp:94] Number of HDF5 files: 1
    I0529 17:21:44.551985 25389 net.cpp:127] Top shape: 10 4 (40)
    I0529 17:21:44.552009 25389 net.cpp:127] Top shape: 10 (10)
    I0529 17:21:44.552022 25389 layer_factory.hpp:74] Creating layer label_data_1_split
    I0529 17:21:44.552038 25389 net.cpp:90] Creating Layer label_data_1_split
    I0529 17:21:44.552050 25389 net.cpp:410] label_data_1_split <- label
    I0529 17:21:44.552063 25389 net.cpp:368] label_data_1_split -> label_data_1_split_0
    I0529 17:21:44.552078 25389 net.cpp:368] label_data_1_split -> label_data_1_split_1
    I0529 17:21:44.552093 25389 net.cpp:120] Setting up label_data_1_split
    I0529 17:21:44.552108 25389 net.cpp:127] Top shape: 10 (10)
    I0529 17:21:44.552119 25389 net.cpp:127] Top shape: 10 (10)
    I0529 17:21:44.552129 25389 layer_factory.hpp:74] Creating layer fc1
    I0529 17:21:44.552144 25389 net.cpp:90] Creating Layer fc1
    I0529 17:21:44.552155 25389 net.cpp:410] fc1 <- data
    I0529 17:21:44.552167 25389 net.cpp:368] fc1 -> fc1
    I0529 17:21:44.552182 25389 net.cpp:120] Setting up fc1
    I0529 17:21:44.552201 25389 net.cpp:127] Top shape: 10 2 (20)
    I0529 17:21:44.552217 25389 layer_factory.hpp:74] Creating layer fc1_fc1_0_split
    I0529 17:21:44.552230 25389 net.cpp:90] Creating Layer fc1_fc1_0_split
    I0529 17:21:44.552242 25389 net.cpp:410] fc1_fc1_0_split <- fc1
    I0529 17:21:44.552253 25389 net.cpp:368] fc1_fc1_0_split -> fc1_fc1_0_split_0
    I0529 17:21:44.552268 25389 net.cpp:368] fc1_fc1_0_split -> fc1_fc1_0_split_1
    I0529 17:21:44.552280 25389 net.cpp:120] Setting up fc1_fc1_0_split
    I0529 17:21:44.552292 25389 net.cpp:127] Top shape: 10 2 (20)
    I0529 17:21:44.552304 25389 net.cpp:127] Top shape: 10 2 (20)
    I0529 17:21:44.552314 25389 layer_factory.hpp:74] Creating layer loss
    I0529 17:21:44.552326 25389 net.cpp:90] Creating Layer loss
    I0529 17:21:44.552337 25389 net.cpp:410] loss <- fc1_fc1_0_split_0
    I0529 17:21:44.552355 25389 net.cpp:410] loss <- label_data_1_split_0
    I0529 17:21:44.552366 25389 net.cpp:368] loss -> loss
    I0529 17:21:44.552379 25389 net.cpp:120] Setting up loss
    I0529 17:21:44.552392 25389 layer_factory.hpp:74] Creating layer loss
    I0529 17:21:44.552409 25389 net.cpp:127] Top shape: (1)
    I0529 17:21:44.552420 25389 net.cpp:129]     with loss weight 1
    I0529 17:21:44.552434 25389 layer_factory.hpp:74] Creating layer accuracy
    I0529 17:21:44.552449 25389 net.cpp:90] Creating Layer accuracy
    I0529 17:21:44.552460 25389 net.cpp:410] accuracy <- fc1_fc1_0_split_1
    I0529 17:21:44.552471 25389 net.cpp:410] accuracy <- label_data_1_split_1
    I0529 17:21:44.552484 25389 net.cpp:368] accuracy -> accuracy
    I0529 17:21:44.552497 25389 net.cpp:120] Setting up accuracy
    I0529 17:21:44.552515 25389 net.cpp:127] Top shape: (1)
    I0529 17:21:44.552525 25389 net.cpp:194] accuracy does not need backward computation.
    I0529 17:21:44.552536 25389 net.cpp:192] loss needs backward computation.
    I0529 17:21:44.552547 25389 net.cpp:192] fc1_fc1_0_split needs backward computation.
    I0529 17:21:44.552557 25389 net.cpp:192] fc1 needs backward computation.
    I0529 17:21:44.552582 25389 net.cpp:194] label_data_1_split does not need backward computation.
    I0529 17:21:44.552594 25389 net.cpp:194] data does not need backward computation.
    I0529 17:21:44.552604 25389 net.cpp:235] This network produces output accuracy
    I0529 17:21:44.552614 25389 net.cpp:235] This network produces output loss
    I0529 17:21:44.552629 25389 net.cpp:482] Collecting Learning Rate and Weight Decay.
    I0529 17:21:44.552640 25389 net.cpp:247] Network initialization done.
    I0529 17:21:44.552650 25389 net.cpp:248] Memory required for data: 528
    I0529 17:21:44.552680 25389 solver.cpp:42] Solver scaffolding done.
    I0529 17:21:44.552700 25389 solver.cpp:226] Solving LogisticRegressionNet
    I0529 17:21:44.552711 25389 solver.cpp:227] Learning Rate Policy: step
    I0529 17:21:44.552723 25389 solver.cpp:270] Iteration 0, Testing net (#0)
    I0529 17:21:44.554693 25389 solver.cpp:319]     Test net output #0: accuracy = 0.5252
    I0529 17:21:44.554721 25389 solver.cpp:319]     Test net output #1: loss = 0.692741 (* 1 = 0.692741 loss)
    I0529 17:21:44.554754 25389 solver.cpp:189] Iteration 0, loss = 0.689532
    I0529 17:21:44.554772 25389 solver.cpp:204]     Train net output #0: loss = 0.689532 (* 1 = 0.689532 loss)
    I0529 17:21:44.554792 25389 solver.cpp:467] Iteration 0, lr = 0.01
    I0529 17:21:44.562496 25389 solver.cpp:270] Iteration 1000, Testing net (#0)
    I0529 17:21:44.564299 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7456
    I0529 17:21:44.564323 25389 solver.cpp:319]     Test net output #1: loss = 0.610764 (* 1 = 0.610764 loss)
    I0529 17:21:44.564350 25389 solver.cpp:189] Iteration 1000, loss = 0.561665
    I0529 17:21:44.564379 25389 solver.cpp:204]     Train net output #0: loss = 0.561665 (* 1 = 0.561665 loss)
    I0529 17:21:44.564393 25389 solver.cpp:467] Iteration 1000, lr = 0.01
    I0529 17:21:44.571682 25389 solver.cpp:270] Iteration 2000, Testing net (#0)
    I0529 17:21:44.573421 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7228
    I0529 17:21:44.573443 25389 solver.cpp:319]     Test net output #1: loss = 0.617058 (* 1 = 0.617058 loss)
    I0529 17:21:44.573467 25389 solver.cpp:189] Iteration 2000, loss = 0.343875
    I0529 17:21:44.573483 25389 solver.cpp:204]     Train net output #0: loss = 0.343875 (* 1 = 0.343875 loss)
    I0529 17:21:44.573495 25389 solver.cpp:467] Iteration 2000, lr = 0.01
    I0529 17:21:44.580531 25389 solver.cpp:270] Iteration 3000, Testing net (#0)
    I0529 17:21:44.582217 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7532
    I0529 17:21:44.582244 25389 solver.cpp:319]     Test net output #1: loss = 0.603834 (* 1 = 0.603834 loss)
    I0529 17:21:44.582767 25389 solver.cpp:189] Iteration 3000, loss = 0.627111
    I0529 17:21:44.582792 25389 solver.cpp:204]     Train net output #0: loss = 0.627111 (* 1 = 0.627111 loss)
    I0529 17:21:44.582818 25389 solver.cpp:467] Iteration 3000, lr = 0.01
    I0529 17:21:44.591498 25389 solver.cpp:270] Iteration 4000, Testing net (#0)
    I0529 17:21:44.593157 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7456
    I0529 17:21:44.593185 25389 solver.cpp:319]     Test net output #1: loss = 0.610764 (* 1 = 0.610764 loss)
    I0529 17:21:44.593209 25389 solver.cpp:189] Iteration 4000, loss = 0.561665
    I0529 17:21:44.593225 25389 solver.cpp:204]     Train net output #0: loss = 0.561665 (* 1 = 0.561665 loss)
    I0529 17:21:44.593236 25389 solver.cpp:467] Iteration 4000, lr = 0.01
    I0529 17:21:44.599835 25389 solver.cpp:270] Iteration 5000, Testing net (#0)
    I0529 17:21:44.601410 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7228
    I0529 17:21:44.601430 25389 solver.cpp:319]     Test net output #1: loss = 0.617058 (* 1 = 0.617058 loss)
    I0529 17:21:44.601451 25389 solver.cpp:189] Iteration 5000, loss = 0.343875
    I0529 17:21:44.601467 25389 solver.cpp:204]     Train net output #0: loss = 0.343875 (* 1 = 0.343875 loss)
    I0529 17:21:44.601479 25389 solver.cpp:467] Iteration 5000, lr = 0.001
    I0529 17:21:44.607870 25389 solver.cpp:270] Iteration 6000, Testing net (#0)
    I0529 17:21:44.609457 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7592
    I0529 17:21:44.609498 25389 solver.cpp:319]     Test net output #1: loss = 0.603448 (* 1 = 0.603448 loss)
    I0529 17:21:44.609912 25389 solver.cpp:189] Iteration 6000, loss = 0.610655
    I0529 17:21:44.609935 25389 solver.cpp:204]     Train net output #0: loss = 0.610655 (* 1 = 0.610655 loss)
    I0529 17:21:44.609946 25389 solver.cpp:467] Iteration 6000, lr = 0.001
    I0529 17:21:44.616145 25389 solver.cpp:270] Iteration 7000, Testing net (#0)
    I0529 17:21:44.617668 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7588
    I0529 17:21:44.617687 25389 solver.cpp:319]     Test net output #1: loss = 0.602623 (* 1 = 0.602623 loss)
    I0529 17:21:44.617707 25389 solver.cpp:189] Iteration 7000, loss = 0.565282
    I0529 17:21:44.617730 25389 solver.cpp:204]     Train net output #0: loss = 0.565282 (* 1 = 0.565282 loss)
    I0529 17:21:44.617741 25389 solver.cpp:467] Iteration 7000, lr = 0.001
    I0529 17:21:44.623797 25389 solver.cpp:270] Iteration 8000, Testing net (#0)
    I0529 17:21:44.625267 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7608
    I0529 17:21:44.625285 25389 solver.cpp:319]     Test net output #1: loss = 0.603558 (* 1 = 0.603558 loss)
    I0529 17:21:44.625304 25389 solver.cpp:189] Iteration 8000, loss = 0.405249
    I0529 17:21:44.625326 25389 solver.cpp:204]     Train net output #0: loss = 0.40525 (* 1 = 0.40525 loss)
    I0529 17:21:44.625339 25389 solver.cpp:467] Iteration 8000, lr = 0.001
    I0529 17:21:44.630926 25389 solver.cpp:270] Iteration 9000, Testing net (#0)
    I0529 17:21:44.632241 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7584
    I0529 17:21:44.632263 25389 solver.cpp:319]     Test net output #1: loss = 0.60319 (* 1 = 0.60319 loss)
    I0529 17:21:44.632691 25389 solver.cpp:189] Iteration 9000, loss = 0.611094
    I0529 17:21:44.632710 25389 solver.cpp:204]     Train net output #0: loss = 0.611094 (* 1 = 0.611094 loss)
    I0529 17:21:44.632721 25389 solver.cpp:467] Iteration 9000, lr = 0.001
    I0529 17:21:44.640231 25389 solver.cpp:337] Snapshotting to hdf5_classification/data/train_iter_10000.caffemodel
    I0529 17:21:44.640470 25389 solver.cpp:345] Snapshotting solver state to hdf5_classification/data/train_iter_10000.solverstate
    I0529 17:21:44.640553 25389 solver.cpp:252] Iteration 10000, loss = 0.565219
    I0529 17:21:44.640573 25389 solver.cpp:270] Iteration 10000, Testing net (#0)
    I0529 17:21:44.641927 25389 solver.cpp:319]     Test net output #0: accuracy = 0.7588
    I0529 17:21:44.641960 25389 solver.cpp:319]     Test net output #1: loss = 0.602603 (* 1 = 0.602603 loss)
    I0529 17:21:44.641970 25389 solver.cpp:257] Optimization Done.
    I0529 17:21:44.641978 25389 caffe.cpp:134] Optimization Done.


If you look at output or the `train_val.prototxt`, you'll see that the model is simple logistic regression.
We can make it a little more advanced by introducing a non-linearity between weights that take the input and weights that give the output -- now we have a two-layer network.
That network is given in `train_val2.prototxt`, and that's the only change made in `solver2.prototxt` which we will now use.

The final accuracy of the new network be higher than logistic regression!


    def learn_and_test(solver_file):
        caffe.set_mode_cpu()
        solver = caffe.get_solver(solver_file)
        solver.solve(). Once that model is done, we'll add layers to improve accuracy. That's what Caffe is about: define a model, experiment, and then deploy.
    
        accuracy = 0
        test_iters = int(len(Xt) / solver.test_nets[0].blobs['data'].num)
        for i in range(test_iters):
            solver.test_nets[0].forward()
            accuracy += solver.test_nets[0].blobs['accuracy'].data
        accuracy /= test_iters
        return accuracy
    
    %timeit learn_and_test('hdf5_classification/solver2.prototxt')
    acc = learn_and_test('hdf5_classification/solver2.prototxt')
    print("Accuracy: {:.3f}".format(acc))


      File "<ipython-input-12-47c9d0811986>", line 4
        solver.solve(). Once that model is done, we'll add layers to improve accuracy. That's what Caffe is about: define a model, experiment, and then deploy.
                                ^
    SyntaxError: invalid syntax



Do the same through the command line interface for detailed output on the model and solving.


    !../build/tools/caffe train -solver hdf5_classification/solver2.prototxt


    # Clean up (comment this out if you want to examine the hdf5_classification/data directory).
    shutil.rmtree(dirname)
