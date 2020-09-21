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
</p>



















 
