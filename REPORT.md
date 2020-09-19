# 목적
- 앞의 실험에서는 본 연구에서 구현한 GAN모델로 생성한 fake face image를 대상으로 ResNet & Xception을 이용하여 classification을 실시한 결과 높은 정확도를 확인함
- 본 실험에서는 기관에서 검증하여 이전 실험에서 사용한 데이터보다 classification이 어려운 데이터를 대상으로 real or fake face image classification 가능여부를 검증함

## 데이터 및 학습방법
- Deepfake Detection Challenge에서 사용된 fake / real face image(1024 x 1024)를 각각 2만장씩 총 4만장을 Kaggle에서 확보함
  * 이미지 생성시 사용된 GAN의 종류는 불명이나, 대회 특성 상 다양한 GAN모델을 사용한 것으로 추정
- ResNet50 및 MovileNet을 이용하여 real or fake 여부를 classification하도록 transfer learning을 실시함
  * Training image number/ validation image number/ batch size/ learning rate: 20000/ 20000/ 20/ 0.0001

### 실험환경
- Google Colab
- GEFORCE GTX 1080 Ti 1개

## 실험결과 및 고찰
- ResNet50/ MovileNet 학습결과: accuracy 0.51/ 0.62
- 상기 2가지 모델로 transfer learning을 실시했으나 결과는 저조함
- 이는 real or fake classification은 형태 및 색깔 등 단순히 사물을 불류하는 weight로는 분리할 수 없으며
- 이에 convolution network을 처음부터 학습이 필요하다고 판단됨
- 추후 


'real'과 'fake'를 판별하는 첫 번째 실험에서는 ImageNet으로 사전 학습된 ResNet-50를 baseline network로 사용하였다. Linear classifier 부분만 binary classification을 하도록 수정하여 fine-tuning한 결과 위의 그림과 같이 99%의 정확도를 보였다. 즉, ImageNet으로 사전 학습된 네트워크를 manipulation detection task에 효과적으로 활용하는 것이 가능하며, image classification에서 얻을 수 있는 정확도 보다 높은 정확도를 얻을 수 있다. 사람에게는 사진 속의 대상이 개인지 고양이인지 알아 맞추는 것이 정교한 가짜 이미지를 보고 진위를 판별해내는 것보다 훨씬 쉬운 일이지만 딥러닝에서는 진짜, 가짜를 판별하는 것이 훨씬 쉬운 일이라고 할 수 있다.



## Multi-Class Classification

앞에서 'real'과 'fake'를 판별하는 것은 쉽게 해결될 수 있음을 확인하였다. 그렇다면 task의 난이도를 높여서 주어진 합성 이미지가 어떤 GAN을 통해 만들어진 것인지 판별하는 것도 가능할까? Binary classification에서와 마찬가지로 baseline network의 classifier를 4개의 class(MSG-GAN, StyleGAN, PGGAN, VGAN)에 대한 logit을 계산하도록 바꾼 뒤 ImageNet에서 사전 학습된 weight를 적용하여 fine-tuning을 하였다. 이번에는 ResNet-50 뿐 아니라 ResNet-101, Xception도 baseline network로 추가하여 실험을 진행하였다.

<p align="center"><img src="images/resnet101_training_log.png" width="500" alt="multi resnet101 chart"></img></p>
<p align="center"><img src="images/xception_training_log.png" width="500" alt="multi xception chart"></img></p>

실험 결과 ResNet과 Xception 모두 binary classification에서와 마찬가지로 99%의 정확도에 도달할 수 있었다. 'real', 'fake'를 판별하는 것 뿐아니라 주어진 이미지가 MSG-GAN, StyleGAN, PGGAN, VGAN 중 어떤 모델에서 생성된 것인지 구분하는 것 역시 완벽에 가깝게 해낼 수 있다. 'real', 'fake'를 판별하는 것은 사람의 눈으로도 어느 정도는 구분이 가능하지만 가짜 이미지가 어떤 모델에서 생성된 것인지 판별하는 것은 사람에게는 거의 불가능한 일이다.

우리는 detection model에서 어떻게 이를 잘 구분해 내는 것인지 확인하기 위해 Grad-CAM을 생성하였다. 아래의 예시들은 특정 GAN에서 생성된 이미지가 입력으로 주어졌을 때 Xception을 baseline으로 한 GAN detection 모델의 12번째 블록에서 뽑은 Grad-CAM의 결과들이다. Grad-CAM에서 색칠된 부분은 모델이 해당 클래스로 이미지를 분류할 때 가장 큰 비중을 둔 곳으로 파란색에서 빨간색으로 갈수록 비중의 값이 크다. 아래의 예시들에서 볼 수 있듯, 각각의 GAN마다 Grad-CAM이 찍히는 부분의 패턴이 존재한다. 실제로 여러 이미지들에 대해 Grad-CAM을 생성한 결과 예시와 같은 패턴이 규칙적으로 발생함을 확인할 수 있었다. 이를 통해 Manipulation detection 시에 dection 모델은 이미지 상에서 어색한 신체 부분(e.g. 눈, 코, 입, 귀)을 통해 진위를 판별하거나 GAN의 종류를 구분해 내는 것이 아니라, GAN의 아키텍처 특징에 따라서 발생하는 노이즈 패턴을 포착하여 판단을 함을 알 수 있다.

**1) MSG-GAN**

<p align="center"><img src="images/msgstylegan/msgstylegan.png" width="800" alt="msgstylegan"></img></p>

**2) StyleGAN**

<p align="center"><img src="images/stylegan/stylegan.png" width="800" alt="stylegan"></img></p>

**3) PGGAN**

<p align="center"><img src="images/pggan/pggan.png" width="800" alt="pggan"></img></p>

**4) VGAN**

<p align="center"><img src="images/vgan/vgan.png" width="800" alt="vgan"></img></p>

# Reference
[1] F. Marra, D. Gragnaniello, L. Verdoliva, and G. Poggi, “Do GANs Leave Artificial Fingerprints?” in Proc. IEEE Conference on Multime- dia Information Processing and Retrieval, 2019, pp. 506–511.
[2] M. Albright and S. McCloskey, “Source Generator Attribution via Inversion?” in Proc. Conference on Computer Vision and Pattern Recognition Workshops, 2019.
