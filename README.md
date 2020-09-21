# End-to-End Deep Learning for AV's Control (Keras/Tensorflow)


The goal of this project is to train an End-to-End deep learning model based on Convolutional Neural Networks (CNN) that would let a car drive agent on the tracks of Udacity’s car simulator environment. Then, we train different deep network after applying the required data pre-processings, then, the performance of each model are compared. Results show that our developed model with filter leads to better autonomous driving in the Udacity simulator. <br /> <br />

<p align="center">
<img width="400" height="200" alt="dnn" src="https://user-images.githubusercontent.com/71558720/93715730-26bf4b80-fb39-11ea-8373-2fa331c9d0ae.png">
</p> <br />

## General Method
 The network is designed and trained with an End-to-End (E2E) format based on a Unity3D simulator track. Here, a convolutional neural network (CNN) is used to map raw pixels from a single front-facing camera directly to steering commands. The E2E learning was applied whereby the network will learn how to control a vehicle only based on an input signal from the camera during real-time inference. It is a supervised regression problem between the car steering angles and the road images in real-time from the cameras records which are installed on a car. Our main goal is to train in an E2E framework such that the trained vehicle is able to drive by itself around the track in a driving simulator.<br />  The driving Udacity simulator saves the frames from three front-facing “cameras”. It records data from the car’s point of view so that we can use it as our dataset.  This data does not contain enough samples for training the network, therefore, different augmentaiton techniques are used for image processing which provides proper dataset for training.

<p align="center">
    <img  width="700" height="230" alt="E2EAV" src="https://user-images.githubusercontent.com/71558720/93715363-ca5b2c80-fb36-11ea-87a6-a78c64677d8f.png">
</p> <br /> 


## Dataset Collection
The Udacity has released a simulator as an open source software (Fig left) which provides a tool to teach a car how to drive using only camera images and deep learning by mimic  from human driving behavior in the training mode on the track. That means a dataset is generated in the simulator by user-driven car in training mode (Track 1) option (Fig right). After implementing our experiment on the trained E2E network, we can check if the car drives in the path by choosing Autonomous Mode in the simulator (Fig left). <br /> 

<p align="center">
   <img width="350" height="250" hspace="20" alt="ss" src="https://user-images.githubusercontent.com/71558720/93714984-53249900-fb34-11ea-8b64-b53478ad68ce.png"/> 
   <img width="350" height="250" hspace="20" alt="train udicity" src="https://user-images.githubusercontent.com/71558720/93675645-ffee1000-fa79-11ea-942c-8c3f01992592.PNG"/>
</p> <br /> <br /> 



<p align="center">
   <img width="700" height="300" alt="dataset" src="https://user-images.githubusercontent.com/71558720/93675577-fa90c580-fa79-11ea-9e04-f1f3a4668b10.PNG"> 
</p>

<p align="center">
   <em>CSV file of dataset sample</em>
</p> <br /> 


## Dataset Balance
The Fig below shows the data distribution chart of the steering wheel angle values in the Udacity dataset. According to this figure, we see the steering angle does not change a lot except a spike in some frames; this shows that there are many straight lines in the Track 1 and the car is driving straight most of the time.

<p align="center">
   <img width="700" height="300" alt="distrobution" src="https://user-images.githubusercontent.com/71558720/93675598-fbc1f280-fa79-11ea-85c3-896ae64bfa09.PNG">
</p> <br /> 

 So, we need to balance the data by reducing straight line angles.
 
 #### 1) Model 1
The first network consists of 9 layers, including a normalization layer, 5 convolutional layers and 3 fully connected layers. CNN layers are organized in series; various combinations of Time-Distributed Convolution layers, Subsampling, Flatten, Dense, etc. are used in architecture.<br /> The network architecture of Model 1 (Base model) is shown in Fig below.

<p align="center">
<img width="870" height="400" alt="aksarc" src="https://user-images.githubusercontent.com/71558720/93675564-f9f82f00-fa79-11ea-9bed-a6827dabfbed.PNG">
</p> <br /> 
<p align="center">
   <em>Model 1</em>
</p> <br /> 

Dataset balance in Model 1 is shown as Fig below:<br />

<p align="center">
  <img width="350" height="250" hspace="20" alt="1" src="https://user-images.githubusercontent.com/71558720/93786036-d14c7280-fbfc-11ea-8bd4-f3eaeab87311.PNG"> <em>Before</em>
  <img width="350" height="290" hspace="20" alt="2" src="https://user-images.githubusercontent.com/71558720/93786046-d4476300-fbfc-11ea-8785-ae67f133e5ef.PNG"> <em>After</em>
</p> <br /> 


#### 2) Model 2 +  Savgol Filter
This network differs from the first model with some modifications. The modifications include the addition of a normalization layer (Lambda λ),  3 dropouts (0.4) and 5 batch-normalization. Besides, in each 5 convolution layers, we added a kernel-regularizer(0.001), kernel-constraint, and bias-constraint for preventing overfitting in our model. <br /> Further modifications includes changing the angular changes of steering from 0.15 in main model to 0.25 in our model as well as splitting the training and validation data to %75 and %25. we also apply a Savitzky-Golay filter to the Steering angle and Speed columns. A Savitzky–Golay filter is a digital filter that can be applied to a set of digital data points to smooth the data, that is, to increase the precision of the data without distorting the signal tendency. The Savitzky-Golay filter removes high-frequency noise from data. The network architecture of Model 2 is shown in Fig below. <br /> 


<p align="center">
<img width="870" height="400" alt="model2" src="https://user-images.githubusercontent.com/71558720/93675606-fc5a8900-fa79-11ea-9d55-f3376d558c14.PNG">
</p> <br /> 
<p align="center">
   <em>Model 2</em>
</p> <br />

We apply the filter to the two components of our dataset in Python client as follow: <br />

*signal.savgolfilter(column”steering”.values.tolist(),windowlength= 51,polyorder= 11)*<br />
*signal.savgolfilter(column”speed”.values.tolist(),windowlength= 25,polyorder= 15)* <br /> <br /> <br />

Dataset balance in Model 2 + Savgol Filter is shown as Fig below:

<p align="center">
   <img width="350" height="250" hspace="20" alt="before" src="https://user-images.githubusercontent.com/71558720/93786262-196b9500-fbfd-11ea-9d20-76df78493c4f.PNG"> <em>Before</em>
   <img width="350" height="280" hspace="20" alt="after" src="https://user-images.githubusercontent.com/71558720/93786254-1670a480-fbfd-11ea-8489-7b8e42431af0.PNG">   <em>After</em>
</p> <br />


## DataAugmentation
It is a technique to artificially create new training data from existing training data. This is done by applying domain-specific techniques such as artificial translational, zoom, brightness, and flipping images to teach the network how to recover from a poor position or orientation. The magnitude of these perturbations is chosen randomly from a normal distribution. <br />


## Implementation and Results

 #### 1) Model 1
 
 First, we implement the base model but with four different activation functions. We are interested to see the effect of activation function on the network performance and decide if it is better to replace the initial activation (Elu) function with a better one. The training and validation loss changes over epochs for four candidates activation functions: Elu, Relu, Sigmoid, and Swish. A comparison between these figures points out that the Relu activation function is a better candidate to work on for the base Model 1 in which the  Elu function had been used before. <br />
 
The video of this simulation is available at :[Model 1 - link](https://youtu.be/FllLgjnSRo8) <br />  <br /> <br /> 
 
 
<p align="center">
<img width="450" height="350" alt="relu" src="https://user-images.githubusercontent.com/71558720/93675619-fcf31f80-fa79-11ea-8903-b9ad28033def.PNG"></p> <br /> 
<p align="center">
   <em>Relu</em>
</p> <br />  <br /> 
 
 
#### 2) Model 2 +  Savgol Filter
 
 The result in figures below demonstrates that both validation and training loss and MSE noticeably get improved compared to the previous case. As we mention before filter helps reduce the training loss because the original signal for steering angle input is via the very discrete keyboard. This improvement is more sensible in the last epochs showing that the network is finally learning well with fewer fluctuations. However, the results are similar to the base Model 1 except in this case we observe better stability and the car drives smoother.<br />  <br /> 
 
 
<p align="center">
<img width="450" height="350" alt="b2" src="https://user-images.githubusercontent.com/71558720/93794501-28a31080-fc06-11ea-8015-df99f628cb58.PNG">
<img width="450" height="350" alt="b1" src="https://user-images.githubusercontent.com/71558720/93794519-2d67c480-fc06-11ea-85db-c57b9611ee09.PNG">
</p> <br />  <br />   


 We also can find the tested car in the Udacity simulator in the following video which is available at: [Model 2+ Savgol Filter - link](https://youtu.be/spUJNUPLZZY) <br /> <br /> 
 
 
 
 
 ** Mina R **
