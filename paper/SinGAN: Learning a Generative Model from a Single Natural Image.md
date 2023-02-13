# SinGAN: Learning a Generative Model from a Single Natural Image(2019)

## Introduction
GAN 기법으로 하나의 이미지를 사용하여 실제와 비슷한 새로운 이미지를 생성하는 data augmentation.
- 단 하나의 이미지 데이터로 unconditional한 이미지 생성이 가능하게 하자.
- 추가적인 네트워크 학습 없이 Image manipulation이 가능하게 하자. ( Super Resolution, Paint to Image, Harmonization, Editing, Single Image Animation )  

### 이전 연구
- 성능
이전에도 하나의 이미지를 사용한 GAN은 있었지만, texture image에서만 결과가 좋고 non-texture image에서는 결과가 좋지 않았다.  
![image](https://user-images.githubusercontent.com/108729047/218533680-023667d3-8122-45cb-b576-deac45f1c424.png)

- 기능
( Image Editing, Sketch to Image, Image to image translation ... )  
이전에는 각각의 목적마다 서로 다른 모델들이 존재하였지만, sinGAN은 하나의 모델로 모든 목적을 수행 가능하다.  
unconditional을 기반으로 하지만 conditional 접근 방식도 사용 가능하다.  

### + 추가 자료
#### unconditional GAN이란?  
![image](https://user-images.githubusercontent.com/108729047/218511771-df5906d9-b000-48af-a87e-c0a6957b2daf.png)  
conditional GAN은 데이터에 라벨링을 해야하지만, unconditional GAN은 라벨링이 필요없이 별도의 제어가 필요없이 랜덤으로 데이터 분포를 학습한다.  
+ Conditional GAN은 내가 원하는 조건으로 데이터를 생성할 수 있다는 장점, Unconditional GAN은 별도의 제어가 필요없다는 장점이 있다.  



## Method
### Generator와 Discriminator의 구조
N번의 upsampling을 진행하며, 위로 올라갈수록 세밀한 정보가 추가된다.  
Generator와 Discriminator가 여러개 존재한다.
![image](https://user-images.githubusercontent.com/108729047/218534990-51ce3fc5-b90c-4b83-8474-0bb61f4185a4.png)
가장 아래 Gn에서 기존 이미지에 아주 약간의 노이즈를 추가해서 이미지를 생성하고, 그걸 기반으로 노이즈를 추가해서 디테일을 추가하는 등을 반복한다.  
Discriminator는 진짜인지 가짜인지 구분하는게 아니라, 패치 단위 이미지를 구분한다.  

### 잔여학습
Generator는 잔여학습 (residual learning)을 사용하기 때문에 난이도가 낮다.  
![image](https://user-images.githubusercontent.com/108729047/218544218-488bd2ce-0edd-4e9d-82e7-a7d4b94fe330.png)  
#### 잔여학습이란?
이전의 정보를 계속 사용하고 디테일만 추가하여 학습하는 방법.   
Conv(3x3)-BachNorm-LeakyReLu로 이루어진 5개의 블록으로 구성되어 있다.  
32개의 kernels / block으로 시작하고 4 scales 마다 kernel을 2배로 늘려준다. ( 가벼운 구조라서

### Loss Function
+ Adversarial Loss
![image](https://user-images.githubusercontent.com/108729047/218548078-1c929a09-d42c-4aee-b6c6-1d9859ff36f2.png)  
  + 생성한 이미지와 기존의 이미지의 패치들 사이의 관계가 유사하도록 만드는 역할을 한다.   
  + 일반적인 advercarial loss인 WGAN-GP를 사용한다.
  + 패치 단위의 이미지 분포를 비교하며 loss를 계산한다.  
+ Reconstruction Loss
![image](https://user-images.githubusercontent.com/108729047/218548189-d47e6cce-a90f-4a64-803a-41478e7d0b41.png)
  + 실제 이미지처럼 정확하게 학습하게 만드는 역할을 한다.  
  + 가장 아래층에 있는 scale의 노이즈만 값을 갖고 나머지 스케일의 노이즈는 0으로 계산한다.  
  + 이전 스케일에서의 이미지와 노이즈를 통해 생성한 것과 해당 스케일의 실제 이미지와 비교하여 loss를 계산한다.  

## Result

### SinGAN을 이용하여 만든 사진들
![image](https://user-images.githubusercontent.com/108729047/218549015-edaa1631-c749-4d6c-8076-b95364a28881.png)  
실제 사진처럼 그럴싸하게 만들어지는게 특징이다.  
CNN을 이용하기 때문에 size를 조정할 수 있어 다양한 크기의 이미지들을 자연스럽게 만들 수 있다.  

### 

![image](https://user-images.githubusercontent.com/108729047/218549541-c9c1b7f2-c548-427e-977e-49f2d325d6e0.png)  
풍경 사진에서는 큰 문제가 없지만, 동물이나 얼굴 데이터 등에서는 문제가 생기는 경우가 발생한다.  

### Test
#### AMT
실험 조건  
1) 원본과 SinGAN이 생성한 이미지를 보여주고 원본 찾기  
2) 원본이나 SinGAN 중 하나만 보여주고 원본인지 아닌지 판단하기  




## Applications


## Conclution


## Referance
https://www.youtube.com/watch?v=UMZZz-Yom9g  
https://kozistr.tech/SinGAN/  
https://rueki.tistory.com/242  
