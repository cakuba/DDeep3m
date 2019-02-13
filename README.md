
# DDeep3m

A dockerized deep-learning model for image segmentation of light, electron and X-ray microscopy, based on CDeep3M

Thanks for the great work from <a href=https://github.com/CRBS/cdeep3m>CDeep3M</a>!

## Quickstart

0. tested with Ubunti 16.04 and Docker version 18.06.1-ce

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
