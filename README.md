# **Traffic Sign Recognition** 

## Writeup

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/rotation.jpg "Rotation"
[image4]: ./simple_signs/1.jpg "Traffic Sign 1"
[image5]: ./simple_signs/2.jpg "Traffic Sign 2"
[image6]: ./simple_signs/3.jpg "Traffic Sign 3"
[image7]: ./simple_signs/4.jpg "Traffic Sign 4"
[image8]: ./simple_signs/5.jpg "Traffic Sign 5"
[image9]: ./simple_signs/6.jpg "Traffic Sign 6"
[image10]: ./examples/outputFeatureMap.jpg "Output Feature Map 1"
[image11]: ./examples/outputFeatureMap_2.jpg "Output Feature Map 2"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799.
* The size of the validation set is 4410.
* The size of test set is 12630.
* The shape of a traffic sign image is (32, 32, 3).
* The number of unique classes/labels in the data set is 43.

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data distributed in train, validation and test data. As you see a number of some traffic sign images quite smaller than others. We will increase these data in the data augmentation part.

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because the edges are better distinguished in grayscale images. That's why I turned all the original and augmented data into grayscale. For converting to grayscale I have used this formula:

        `Gray = 0.2989*R + 0.5870*G + 0.1140*B`
    
Here is an example of a traffic sign image before and after grayscaling.

![alt text][image2]

As a last step, I normalized the image data because normalization ensures that the pixels of the image have a similar distribution. This makes convergence faster during training the network. I have used a quick way to approximately normalize the data.

        `Normalization = (pixel - 128)/ 128`

I decided to generate additional data Because the number of pictures of some traffic signs is quite small. I have produced fake data for traffic signs whose number of pictures is less than 3/5 of the average.

I used rotation techniques to add more data to the data set because the images will seem at different angles while they were taken in the real world.I've turned all the training data by +10 and -10 degrees. Then I added them to the training set

Here is an example of an original image and an augmented image:

![alt text][image3]

The difference between the original data set and the augmented data set is the following :
        
        Each image has a copy that +10 degree rotated in augmented training data.
        
        Each image has a copy that -10 degree rotated in augmented training data.
        
        Low frequent images have extra one copy in augmented training data


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 Grayscale image 						| 
| Convolution 5x5     	| 1x1 stride, no padding, outputs 28x28x64  	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x16 				|
| Convolution 3x3	    | 1x1 stride, 1 padding, outputs 14x14x32		|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 7x7x32   				|
| Convolution 5x5	    | 1x1 stride, no padding, outputs 3x3x64		|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 2x2x64   				|
| Fully connected		| outputs 128  									|
| RELU					|												|
| Fully connected		| outputs 84   									|
| RELU					|												|
| Fully connected		| outputs 43   									|
|						|												|
|						|												|
 
 The model works better when I didn't implement dropout in dense layers.  


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an AdamOptimizer and I have chosen 128 batch size and 0.0015 learning rate.I ran the model for 70 epochs and saved the model for cases where validation accuracy is greater than 0.96. I obtained a test accuracy of 0.94 to 0.956 for different hyperparameters

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* validation set accuracy of 0.98 
* test set accuracy of 0.954

If a well known architecture was chosen:
* What architecture was chosen?
* Why did you believe it would be relevant to the traffic sign application?
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
Initially I used the LeNet architecture of Yan LeCun. I obtained validation accuracy up to 0.9 with the original version of the model. Then, with some changes in the structure of the model 0.93 Validation accuracy was able to go over. These changes are different combinations to the number of Convolutional layers, the number of dens layers, filter, stride, padding, dropout parameters. Validation accuracy increased over 0.95 after data augmentation and preprocessing.

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6] 
![alt text][image7] ![alt text][image8] ![alt text][image9]

The model will not have difficulty in accurately predicting given examples. 

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Priority Road   		| Priority Road									| 
| Road Work    			| Road Work 									|
| Yield					| Yield											|
| 30 km/h	      		| 30 km/h   					 				|
| Keep Riht 			| Keep Right        							|
| No Parking 			| Go straight or right							|


The model was able to correctly guess 5 of the 6 traffic signs, which gives an accuracy of 83.3%. It is not possible to accurately estimate the 6th picture because it is not included in the training set.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 34th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a Priority Road (probability of 1.0), and the image does contain a Priority Road. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Priority road 								| 
| 0.0     				| 20km/h 										|
| 0.0					| 30km/h										|
| 0.0	      			| 50km/h       					 				|
| 0.0				    | 60km/h             							|


For the other 4 images, the results are the same. But just last image is different. Actually no 6th image in the dataset.The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.69         			| Go straight or right  						| 
| 0.30     				| Road work										|
| 0.0					| Right-of-way at the next intersection			|
| 0.0	      			| Ahead only					 				|
| 0.0				    | Yield              							|


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?

Below we see the filter images created in the first and second Convolutional layers for the road works sign. The first layer filters the edges of the mark, while the second layer shows the points at which weights are concentrated.

![alt text][image10]

![alt text][image11]


