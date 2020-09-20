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
- ResNet50/ MovileNet accuracy: 0.51/ 0.68

![result](https://user-images.githubusercontent.com/52662915/93710328-b76d4a00-fb80-11ea-846e-dc4aa3423b50.png)


- 상기 2가지 모델로 transfer learning을 실시했으나 결과는 저조함
- 이는 real/fake face image의 경우 structure 및 texture 등 ImageNet 사물을 불류하는 weight로는 classification이 불가함
- 이에 convolution network을 처음부터 학습이 필요하다고 판단
