---
layout: post
title: OpenCV Install from Github (Ubuntu)
---

*excerpts from opencv3 blueprints*

>![_config.yml]({{ site.baseurl}}/images/opencv3_blueprints.jpg)


### Factors that affect the object detection

- **Data collection** 

This step includes collecting the necessary data for building and testing your object detector. The data can be acquired from a range of sources going from video sequences to images captured by a webcam. This step will also make sure that the data is formatted correctly to be ready to be passed to the training stage.

An example of positive and negative training data for an object model:

>![_config.yml]({{ site.baseurl}}/images/opencv3_blueprints_74.jpg)

**Positive** - contains the different presentations of the object you want to detect (this means object instances under different lighting conditions, different scales, different orientations, small shape changes, and so on).

For the positive samples, only use **natural occurring samples**. There are many tools out there that create artificially rotated, translated, and skewed images to turn a small training set into a large training set. However, research has proven that the resulting detector is less performant than simply collecting positive object samples that cover the actual situation of your application. Better use a small set of decent high quality object samples, rather than using a large set of low quality non-representative samples for your case.

**Negatives** - contains everything that you do not want to detect with your model. For the negative samples, there are two possible approaches, but both start from the principle that you collect negative samples in the situation where your detector will be used, which is very different from the normal way of training object detects, where just a large set of random samples not containing the object are being used as negatives.

Either point a camera at your scene and start grabbing random frames to sample negative windows from.
    
Or use your positive images to your advantage. Cut out the actual object regions and make the pixels black. Use those masked images as negative training data. Keep in mind that in this case the ratio between background information and actual object occurring in the window needs to be large enough. If your images are filled with object instances, cutting them will result in a complete loss of relevant background information and thus reduce the discriminative power of your negative training set.

Efficiently collecting data in this way ensures that you will end up with a very robust model for your specific application! However, keep in mind that this also has some consequences. The resulting model will not be robust towards different situations than the ones trained for. However, the benefit in training time and the reduced need of training samples completely outweighs this downside.

Note

Software for negative sample generation based on OpenCV 3 can be found at https://github.com/OpenCVBlueprints/OpenCVBlueprints/tree/master/chapter_5/source_code/generate_negatives/.

You can use the negative sample generation software to generate samples like you can see in the following figure, where object annotations of strawberries are removed and replaced by black pixels.

>![_config.yml]({{ site.baseurl}}/images/opencv3_blueprints_75.jpg)


- **Creating object annotation files for the positive samples**

When preparing your positive data samples, it is important to put some time in your annotations, which are the actual locations of your object instances inside the larger images. Without decent annotations, you will never be able to create decent object detectors.

Software for object annotation based on OpenCV 3 can be found at https://github.com/OpenCVBlueprints/OpenCVBlueprints/tree/master/chapter_5/source_code/object_annotation/.

The OpenCV team was kind enough to also integrate this tool into the main repository under the apps section. This means that if you build and install the OpenCV apps during installation, that the tool is also accessible by using the following command:

/opencv_annotation -images <folder location> -annotations <output file>

>![_config.yml]({{ site.baseurl}}/images/opencv3_apps.png)

- **Model Training**

In this step, you will use the data gathered in the first step to train an object model that will be able to detect that model class. Here, the different training parameters will be investigated and the focus will be on defining the correct settings for the application.

- **Testing and Validation**

Once you have a trained object model, you can use it to try and detect object instances in the given test images. Compare each detection with a manually defined ground truth of the test data.

*Ground truth* means a set of measurements that is known to be much more accurate than measurements from the system you are testing, e.x. manually annotated, problem-specific labels in computer vision.






> "*If you have a positive attitude and constantly strive to give your best effort, eventually you will overcome your immediate problems and find you are ready for greater challenges.*"  
                                       Pat Riley
