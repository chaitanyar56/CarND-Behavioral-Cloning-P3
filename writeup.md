**Behavioral Cloning Project**

[//]: # (Image References)

[image1]: ./examples/learningcurve.jpg "Learning Curve"

---
Link to my  [project code](https://github.com/chaitanyar56/CarND-Behavioral-Cloning-P3/blob/master/model.ipynb) and  [Simulator Video](https://github.com/chaitanyar56/CarND-Behavioral-Cloning-P3/blob/master/Simulation.mp4) .
---

**Model Architecture and Training Strategy**

1. Solution Design Approach

* Collect data around the track in the simulator.
* Start with a simple regression network which predicts the steering angles.
* Train and check the performance on simulator.
* Based on the performance do preprocessing and add complexity by adding Convolution layers and fully connected layer.
* repeat the steps till desired result is achieved.

2. Final Model Architecture

I started with LeNet Architecture and proceeded to more powerful architecture used by the Nvidia team ("link").  It was difficult to keep the car on track with LeNet for 10 epochs.

My final model consisted of the following layers:

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Input         		| 160x320x3 RGB image   							|
| lambda Layer      | 160x320x3                           |
| 2d Cropping Layer [(50,20),(0,0)]   | Output 90x320x3                         |
| Convolution 5x5   | 24 filters 2x2 stride, same padding, outputs 43x158x24 	|
| RELU					|												|
| Convolution 5x5   | 36 filters 2x2 stride, same padding, outputs 20x77x36 	|
| RELU					|												|
| Convolution 5x5   | 48 filters 2x2 stride, same padding, outputs 8x37x48 	|
| RELU					|												|
| Convolution 3x3   | 64 filters 1x1 stride, same padding, outputs 6x35x64 	|
| RELU					|												|
| Convolution 3x3   | 64 filters 1x1 stride, same padding, outputs 4x33x64 	|
| RELU					|												|
|	Flatten					|												|
| Fully connected	1	| Input 8448  Output 100     									|
| Fully connected	2	| Input 100  Output 50     									|
| Fully connected	3		| Input 50  Output 10     									|
| Output 		| Input 10  Output 1     									|


3. Creation of the Training Set & Training Process

Simulator video recordings is done by 3 cameras left right and center. By using left and right images with some correction steering angle the dataset can be increased by 3 times. The simulator track is has lot of left turns to remove the network over steering to left, dataset is appended by the images flipped along with updated steering angles. While training the images are cropped to remove the unwanted information from scene which confuses the network and normalized for better performance. Generator are used in training as the dataset is huge which requires lot of memory.


Data used for training the model is used from the set provided by the udacity. It randomly shuffled the data set and put 20% of the data into a validation set.  The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 1 as evidenced by figure 1. I used an adam optimizer so that manually training the learning rate wasn't necessary.

![alt text][image1]
