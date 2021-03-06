# Online Invigilation Through Deep Learning

The purpose of this project was to come up with a technique to determine whether a student is performing some act of cheating or not. So, we decided to use deep learning to train a model that could help us achieve this goal. This repository contains the details of model we used and how we collected the dataset for our task.

## Collect Data

In order to achieve this task, we firstly needed a dataset but unfortunately we couldn't find any dataset that could help our cause so, we decided to collect the dataset on our own. One method we used to collect the data was that we asked the students of our university to take a simple test while recording themselves, the test would take at least 3 minutes to solve and while solving the test they were asked to perform any act of cheating if they wanted to. The other method was to record themselves and perform certain acts of cheating deliberately. 

The acts of cheating were:
* Use mobile phone
* Peak into spare register
* Look at sides often
* Stand and leave camera frame
* Sit next to group of people
* Use laptop's keyboard frequently
* Wear headphones/microphones

There were more acts of cheating, but we decided to start with these actions initially. In total we collected 26 videos with each video having 30 frames per second.

### Generate examples
After collecting the dataset, we had to break down the videos into shorter samples that could be fed to our model at once. After reading a few research papers, few found out that a sample with 16 consecutive frames can give us a good context of what might be going on in the video at that particular time so, we decided to keep each sample of 16 frames. The dataset can be found [here](https://drive.google.com/drive/folders/1tuV933sAg5etysC5vAcLyX4LTjf5Bj1b?usp=sharing).

## Model

The model we chose for our task was a hybrid of Inception V3 and LSTM. Although there are several pre-trained models for computer vision tasks, after reading the literature, we decided to choose Inception V3 to process the images because several research papers used Inception V3 to detect the actions in a video. 

Processing a single frame at a time and making inference will only give us spatial information and it is most cases it is not enough to tell the action a person if performing to we in order to get the context of a sequence of frames, we used LSTM to process a sequence of frames, LSTM will give us temporal information which will help the model classify the action better.

So, one pass through our model works in the following way. We take one example of 16 frames and pass one frame through the Inception V3 model, Inception V3 give us a vector of 2048 dimensions. This vector is then passed through one LSTM time step. Each of the 16 frames are processed this way and after 16 LSTM time steps, the output of 16th time step, which is a vector of 256 dimensions, is passed through a classification layer to determine if a student is cheating or not. We trained this model for 34 epochs, the weights of the trained model can be found [here](https://drive.google.com/drive/folders/1K888riyGcYLCS1RpTwiPNDtVwQ0LAKnL?usp=sharing). The settings to train the model can be found in the file "Settings.txt". The plots for the trained model can be found [here](https://drive.google.com/drive/folders/1P6BOC7cqtfyl5M6hSG3tkT8cF4L7BQe4?usp=sharing)

## Inference

Making inference of just 16 frames might not give us the desired outcome when it comes to applying this model in real world as 16 frames are just half a second so, we decided to record video with 160 frames (5.33 seconds) and then take the average output and determine the final outcome. The "Inference" directory contains a notebook that will help you make inference, a sample video is also provided to get you started.
