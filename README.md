
# DDeep3m

A dockerized deep-learning model for image segmentation of light, electron and X-ray microscopy, based on CDeep3M

Thanks for the great work from <a href=https://github.com/CRBS/cdeep3m>CDeep3M</a>!

## Quickstart

0. fully tested with Ubunti 16.04 and Docker version 18.06.1-ce

1. clone the repo to local directory 
```Bash
   git clone https://github.com/cakuba/DDeep3m.git
```
2. to build the Docker image for cuda-9 and cudnn-7
```Bash
   cd cuda-9.0-cudnn7-devel 
   docker build -t cuda9-cudnn7 .
```   
3. to build the DDeep3M image
```Bash
   cd ../ddeep3m
   docker build -t ddeep3m .
```   
4. to check the DDeep3M image
```Bash
   docker image ls
   REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
   ddeep3m                 latest              1dc19b05aa93        24 hours ago        4.4GB
```

**NOTE: 

(1) depending on the bandwidth of network connection, the building process might take up to ~30 mins. So a little patience is appreciated.

(2) if something goes wrong during building the image, the quickest solution is to remove the temporary container and image, and then, re-run the above command.


## How to use the DDeep3M?

0. run the DDeep3M image and obtain an interactive bash prompt in the container
```Bash
   nvidia-docker run -it ddeep3m:latest bash
```
1. enter the default working directory in the container
```Bash
   cd /usr/local/src/cdeep3m-1.6.2
```
2. pre-process the training data with augmentation
```Bash
   ./PreprocessTrainingData.m ./mito_testsample/training/images ./mito_testsample/training/labels ./mito_augmented/ 
```   
3. run the model with training data
```Bash
   ./runtraining.sh --numiterations 10 ./mito_augmented/ ./mito_trainout 
```   
4. predict the test data with trained model
```Bash
   ./runprediction.sh ./mito_trainout ./mito_testsample/testset/ ./mito_predictout/ 
```   
