## Behavioral Cloning Project

The goal of this project is to develop a CNN model that learns and predict steering angles form the video images.

[Link](https://github.com/chaitanyar56/CarND-Behavioral-Cloning-P3/blob/master/Simulation.mp4) to output of the network on simulator track.

[//]: # (Image References)

[image1]: ./examples/learningcurve.jpg "Learning Curve"

## Model Architecture and Training Strategy

1. **Solution Design Approach**

  * Collect data around the track in the simulator.
  * Start with a simple regression network which predicts the steering angles.
  * Train and check the performance on simulator.
  * Based on the performance do preprocessing and add complexity by adding Convolution layers and fully connected layer.
  * repeat the steps till desired result is achieved.

2. **Final Model Architecture**

  * I started with LeNet Architecture and proceeded to more powerful architecture used by the Nvidia team ("link").  It was difficult to keep the car on track with LeNet for 10 epochs.
  * My final model consisted of the following layers:

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

* **Creation of the Training Set & Training Process**

  * Simulator video recordings is done by 3 cameras left right and center. By using left and right images with some correction steering angle the dataset can be increased by 3 times. The simulator track is has lot of left turns to remove the network over steering to left, dataset is appended by the images flipped along with updated steering angles. While training the images are cropped to remove the unwanted information from scene which confuses the network and normalized for better performance. Generator are used in training as the dataset is huge which requires lot of memory.
  * Data used for training the model is used from the set provided by the udacity. It randomly shuffled the data set and put 20% of the data into a validation set.  The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 1 as evidenced by figure 1. I used an adam optimizer so that manually training the learning rate wasn't necessary.

![alt text][image1]

## Details About Files In This Directory

### `drive.py`

Usage of `drive.py` requires you have saved the trained model as an h5 file, i.e. `model.h5`. See the [Keras documentation](https://keras.io/getting-started/faq/#how-can-i-save-a-keras-model) for how to create this file using the following command:
```sh
model.save(filepath)
```

Once the model has been saved, it can be used with drive.py using this command:

```sh
python drive.py model.h5
```

The above command will load the trained model and use the model to make predictions on individual images in real-time and send the predicted angle back to the server via a websocket connection.

Note: There is known local system's setting issue with replacing "," with "." when using drive.py. When this happens it can make predicted steering values clipped to max/min values. If this occurs, a known fix for this is to add "export LANG=en_US.utf8" to the bashrc file.

#### Saving a video of the autonomous agent

```sh
python drive.py model.h5 run1
```

The fourth argument, `run1`, is the directory in which to save the images seen by the agent. If the directory already exists, it'll be overwritten.

```sh
ls run1

[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_424.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_451.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_477.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_528.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_573.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_618.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_697.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_723.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_749.jpg
[2017-01-09 16:10:23 EST]  12KiB 2017_01_09_21_10_23_817.jpg
...
```

The image file name is a timestamp of when the image was seen. This information is used by `video.py` to create a chronological video of the agent driving.

### `video.py`

```sh
python video.py run1
```

Creates a video based on images found in the `run1` directory. The name of the video will be the name of the directory followed by `'.mp4'`, so, in this case the video will be `run1.mp4`.

Optionally, one can specify the FPS (frames per second) of the video:

```sh
python video.py run1 --fps 48
```

Will run the video at 48 FPS. The default FPS is 60.
