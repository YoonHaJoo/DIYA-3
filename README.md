# VERY DEEP CONVOLUTIONAL NETWORKS  
# FOR LARGE-SCALE IMAGE RECOGNITION  
  
### VGGNet 요약
* VGGNet은 옥스포드 대학의 연구팀 VGG에 의해 개발된 모델
* 모든 필터 커널의 사이즈를 줄이고 대신 네트워크의 깊이를 깊게 하여 computation time을 줄이고 accuracy를 향상시킴을 증명함
* 2014년 ImageNet 인식 대회에서 준우승  
  
  
### 1. INTRODUCTION
* Objective: investigate the effect of the convolutional network depth on its accuracy in the large-scale image recognition setting
  
  
### 2 CONVNET CONFIGURATIONS
### 2.1 ARCHITECTURE

* Dataset: ILSVRC-2012 dataset
* Preprocessing: subtracting the mean RGB value, computed on the training set, from each pixel
* Convolutional (conv.) layers with a very small receptive field: 3 × 3    * which is the smallest size to capture the notion of left/right, up/down, center

### 2.2 CONFIGURATIONS

