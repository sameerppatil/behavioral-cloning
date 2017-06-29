# **Behavioral Cloning**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/placeholder.png "Model Visualization"
[image2]: ./examples/placeholder.png "Grayscaling"
[image3]: ./examples/placeholder_small.png "Recovery Image"
[image4]: ./examples/placeholder_small.png "Recovery Image"
[image5]: ./examples/placeholder_small.png "Recovery Image"
[image6]: ./examples/placeholder_small.png "Normal Image"
[image7]: ./examples/placeholder_small.png "Flipped Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* P3.ipnyb containing the python code to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network
* writeup.md summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The P3.ipnyb file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

For my model, I choose to use the NVIDIA End-to-End Self Driving car model as mentioned in lessons. I started off with a simple CNN as put down in lessons along with the sample data.

But using that model along with data, car running under Autonomous mode would simply drive off couple of times.
The most consistent problem I have seen with NVIDIA model is that at the end of bridge, car would go off the road. I had no idea what might be causing this problem. So I gathered lot of data. 

Following are the types of laps I recorded to train the network:
* Two regular laps with focus on center lane
* One reverse lap with focus on center lane
* One lap with recovery on turns
* One lap with focus on smooth curves
* Several small iterations of going over the bridge one way and other.

#### 2. Attempts to reduce over fitting in the model

Since network provided a validation loss of ~98%, I didn't take dedicated attempt to reduce over fitting in the model.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line ??). Adam optimizer helped reducing the loss in each epoch by using running average of gradients.

#### 4. Appropriate training data

As described in section 1, I recorded several laps of Track 1.

### Model Architecture and Training Strategy

#### 1. Final Model Architecture

I started off by reading the input images into two arrays of image and measurements.
For each center image and its corresponding measurement, I added 0.25 from steering angle from left camera image and subtracted 0.25 from steering angle of right camera image.

I also randomly flipped images horizontally to augment the data and balance the steering orientation over time.

The final model architecture (model.py lines 18-24) consisted of a convolution neural network with the following layers and layer sizes ...

| Layer                 |     Description                               |
|:---------------------:|:---------------------------------------------:|
| Input                 | 160x320x3 image                               |
| Convolution 5x5       | 2x2 stride, valid padding, output 24          |
| RELU                  |                                               |
| Convolution 5x5       | 2x2 stride, valid padding, outputs 36         |
| RELU                  |                                               |
| Convolution 5x5       | 2x2 stride, valid padding, outputs 48         |
| RELU                  |                                               |
| Convolution 3x3       | 1x1 stride, valid padding, outputs 64         |
| RELU                  |                                               |
| Convolution 3x3       | 1x1 stride, valid padding, outputs 64         |
| RELU                  |                                               |
| Fully connected       | Output 1000                                   |
| Fully connected       | Output 50                                     |
| Fully connected       | Output 10                                     |
| Fully connected       | Output 1                                      |

![alt text][image1]

#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![alt text][image2]

I then recorded the vehicle recovering from the left side and right sides of the road back to center so that the vehicle would learn to .... These images show what a recovery looks like starting from ... :

![alt text][image3]
![alt text][image4]
![alt text][image5]

Then I repeated this process on track two in order to get more data points.

To augment the data sat, I also flipped images and angles thinking that this would ... For example, here is an image that has then been flipped:

![alt text][image6]
![alt text][image7]


After the collection process, I had 60k number of data points. I then preprocessed this data by normalizing the values. I also changed the color space to YUV space to get any benefit but it didn't help.


I finally randomly shuffled the data set and put 20% of the data into a validation set.

And then this data was fed to model with loss function of mean square error and using adam optimizer.