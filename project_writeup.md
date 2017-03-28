##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/car_notcar.png
[image2]: ./output_images/Hog_feature.png
[image3]: ./output_images/sliding_window.png
[image4]: ./output_images/output.png
[image5]: ./output_images/multiple_frame_heat.png
[image6]: ./output_images/integrated_heat.png
[image7]: ./output_images/output.png
[video1]: ./project_video_output.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I extracted the HOG, spatial and histogram features from the training image in 1 and 2nd code cell of Vehicle-Detection-Pipeline.ipynb.
I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)` (code cell 1-2)


![alt text][image2]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and 'YCrCb' color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2) gave me the best performance.
Also spacial size of (32,32) and histogram bin of 32 worked the best for me.
####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I used all three features and after scaling them and splitting the data into training and test set I trained an SVM classifier in the code cell 3.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I first implemented a simple sliding window search but with its slow speed and low accuracy I decided to go with the HOG sub-sampling window search by calling it multiple times with three different scales for three parts of the image for which 2 cells per step value worked best.(code cell 14)

![alt text][image3]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on three scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I saved my heatmaps in a deque class of the collections library and applied the threshold on the sum of the deque instead of a single frame. (code cell 13)

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?
The main parts of my approach were using HOG subsampling for features, using 3 scales and using a collection to sum the heatmaps to remove false positives. Mostly I had some difficulty optimizing the features as to build the best classifier as it took some time. This approach can cause some problems when we are going up or down hill, during rain or bad weather conditions and at night or under tunnels when the change in light contrasts. Whenever doing classification more data can always help and using Deep Learning techniques can also imporve the classifier.

