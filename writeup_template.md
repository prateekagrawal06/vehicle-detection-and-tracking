## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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
[image1]: ./output_images/final1.png
[image2]: ./output_images/final2.png
[image3]: ./output_images/final3.png
[image4]: ./output_images/final4.png
[image5]: ./output_images/final5.png
[image6]: ./output_images/final6.png
[image7]: ./output_images/heatmap1.png
[image8]: ./output_images/heatmap2.png
[image9]: ./output_images/heatmap3.png
[image10]: ./output_images/heatmap4.png
[image11]: ./output_images/heatmap5.png
[image12]: ./output_images/heatmap6.png
[image13]: ./output_images/hog_car.png
[image14]: ./output_images/hog_noncar.png
[image15]: ./output_images/on_window1.png
[image17]: ./output_images/on_window2.png
[image18]: ./output_images/on_window3.png
[image19]: ./output_images/on_window4.png
[image20]: ./output_images/on_window5.png
[image21]: ./output_images/on_window6.png
[image22]: ./output_images/search_area_5.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The extraction of HOG features in the trianing pipeline  takes place in the sixth block of the jupyter notebook n the function get_hog_features. It takes the image orintation pix per cell and cell per bloack as the parameter and retures the hog features and visualization . It uses the hog library from skimage.features.

Below is the example of Hog features for a vehical example with grayscale image
![alt text][image13]

Below is the example of Hof Features for a non vehicle example on grayscale image

![alt text][image14]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried varies combination of color channels (inclusing grayscale), orientation , pixels per cell and cells per block. 
I finaly settled on a cobination that gave me the best testing accuracy on the data to discriminate between vehivle and non vehicle.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I started with creating a feature space as a combination of spatial features, histograms and HOG features. The code for this can be found in block 7 of the jupyter notebook. I tried different combination of all three and trained a simple linear support vector machine on top of it. I choose the best combination bases on the testing accuracy. I divided the whole data in 80% training and 20% testing data. 


### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

The code for sliding window search in implemented in block 29th from the top of the juyter notebook. I implemented 8 different sliding window approach. I tried different combination of these as 8 and settle for one shown in the image below.

![alt text][image22]

Smaller windows were drawn near the horizon in the image . As the window came closer to car they became bigger. Overlap of 0.5 in x axis and 0.75 in y axis gave possibility of less false possitive. 

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Example for window that were classified as vehicle as show below.
![alt text][image15]
![alt text][image17]
![alt text][image18]
![alt text][image19]
![alt text][image20]
![alt text][image21]

Example of the heat map as hown below

![alt text][image7]
![alt text][image8]
![alt text][image9]
![alt text][image10]
![alt text][image11]
![alt text][image12]


Final window was selected by using label library from scipy on the previous images. Outputs are given below.

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

I optimized the performace of the fitting a polynomial svm rather than linear. Also made the search very relevant. That is, No search on the places car cannot appear.
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

To filter out false positive in the code I thresholded the overlapping of the window by minimum of 1. 

### Here are six frames and their corresponding heatmaps:

show above

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
show above

### Here the resulting bounding boxes are drawn onto the last frame in the series:
show above



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

1. Data set given very baised towards black color. It detects anything black as a vehical.
2. I tried some data augmentation by extracting data from autii dataset, but this increased my training time very significantly and did not give decreased by testing accuracy.
3. Number of hyperparameter for this project was really high . I should we huperparamter optimization libraries such as hyperopt or fabolas.
4. It took me a very long time to come up with a good combination of sliding window
5. Instead of using so many combination we could have tried to use deep learning to classify car and non car and use it directly.