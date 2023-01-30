# A survey on Image Data Augmentation for Deep Learning
### 목차
* Abstract  
* Introduction  
* Background
* Image Data Augmentation techniques
* Design considerations for image Data Augmentation
* Discussion
* Future work
* Conclusion

# 구분
![data augmentation 분류](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJqpuJ%2FbtrNSXgOn6X%2Fhm2RXAj4lReoamU6QtWvf1%2Fimg.png)  

논문에서는 data agumentation이 크게 Basic, Deep Learning, Meta Learning으로 나누어서 분류.  

# Basic Image Manipulation (기본 이미지 조작을 기반으로 한 데이터 확대)
## Geometric transformations (기하 변환)
이미지를 뒤집거나 돌리거나 자르는 등 기하학적으로 변환시켜 증강시키는 방법.  
![기하 변환](https://user-images.githubusercontent.com/108729047/215556559-de52a705-17a7-494b-9f54-159de49816bb.png)

### Flipping
가장 쉬운 데이터 증강 방법.  
이미지를 수평, 수직을 기준으로 뒤집어서 이미지를 늘리는 방식. 주로 horizontal axis (수평 축)이 vertical axis (수직 축)보다 일반적. 

### Rotation  
1~359도로 이미지를 돌리는 방법. 회전 정도에 따라 안전성이 달라짐. 주로 1에서 20, -1에서 -20 정도만 rotation함.

![Mnist 6, 9](https://user-images.githubusercontent.com/108729047/215557795-8da6098f-e294-4285-89d8-78a8886b000b.png)  
MNIST같은 text recoginition은 flipping과 rotation으로 이미지를 증강시키면 레이블이 보존되지 않아 학습에 악영향을 끼침.
예를들면 6과 9가 혼동되면서 레이블이 섞임 => 안전성 하락

### translation
이미지를 한쪽으로 치우치게 만드는 방법.  
![image](https://user-images.githubusercontent.com/108729047/215562774-eebe6a43-3064-46ca-8b30-72b7c2a3d447.png)  
이미지가 채워지고 남은 부분은 0(검정색), 255(하얀색), 랜덤값, 노이즈 등으로 채워짐.

### Cropping
이미지의 size를 줄여 확대시키는 방법. 이미지를 랜덤으로 자르고 특정 부분만 보여준다는 점에서 translation과 비슷하지만, 공간차원이 보존된다는 차이점이 존재.  
공간 차원이 보존되기 때문에 성능이 좋음.


## Color space Transformation (색상 공간 변환)
색상채널 값을 변환시켜 이미지를 증강시키는 방법.  
이미지는 height * width * RGB(높이 * 넓이 * 색상체널)를 가진 차원의 텐서로 데이터가 인식됨.
![image](https://user-images.githubusercontent.com/108729047/215573730-65745b8a-5a3a-4fed-ba25-9e3c63b037c5.png)  
같은 사진이 조명만 달라져도 이미지 인식에 어려움이 있음. RGB 색상 값에 대한 픽셀 값을 조절하면 색 공간이 변환됨.  

R은 그대로 주고 G,B는 0으로 만들거나 값을 조금씩 조절. 혹은 사진을 흑백처리하는 방식 등.
![image](https://user-images.githubusercontent.com/108729047/215572535-2502d995-c02e-4fb7-a8e0-de0dad47d4c4.png)  
대비(Constrast) 조절, Histogram equlization(이미지 픽셀 분포 변경) 적용, White balance (흰색 color 강조), Sharpen(선명하게) 등도 색상 공간 변환에 포함됨  
![image](https://user-images.githubusercontent.com/108729047/215575517-de45445a-6685-4a7c-8d30-c46a902342cd.png)  


## Kernel Filter
픽셀값의 구조를 변경하여 이미지를 선명하거나 흐리게 만들어 이미지를 증강시키는 방법. (잡음 제거)  
가우시안, 엣지 필터는 고전적인 방식.
### Gaussian Filter (가우시안 필터)
이미지를 blurr처리(윤곽선 차단)하여 더 흐려지게 만드는 필터. 해당 방식을 이용하여 훈련하면 흐린이미지를 더 잘 예측할 수 있음.  
![image](https://user-images.githubusercontent.com/108729047/215580567-f2abb603-8f13-4d0c-95ec-a696c1c7100b.png)  
정규 분포를 이용한 필터라 중앙값 근처의 픽셀에는 큰 영향을 주고 멀어질수록 영향을 적게주는 필터. 
n x n으로 영역을 나누고, 영역 내의 행렬을 밀어서 이미지를 더 흐리게 만듦.  

### Edge Filter (엣지 필터)
directional filter라고도 불리며, 이미지를 더 고해상도로 만드는 필터. 픽셀의 밝기가 급격하게 변하는 부분을 찾아서 픽셀값을 조절하는 필터.    
( 소벳 필터, 캐니 엣지 필터, 샤르 필터 등 필터 종류가 많아서 추가로 찾아 볼 예정 )

### 정리
가우시안 필터는 블러링, 엣지 필터는 샤프닝  
두 필터 다 픽셀값의 변화율이 큰 부분(고주파)을 찾아내서 블러링으로 고주파를 없애거나, 샤프닝으로 고주파를 더 강조하는 방법. 
![image](https://user-images.githubusercontent.com/108729047/215592024-3185c95b-2824-4c7a-ae12-0c7764b9c674.png)
![image](https://user-images.githubusercontent.com/108729047/215591975-1b0f7569-a854-4f28-876f-4617afd3c344.png)  



### Patch Shuffle Filter
CNN의 매커니즘과 유사한 커널 layer.   
n x n으로 영역을 나누고, 영역 내의 픽셀을 랜덤하게 섞는 필터.  
![image](https://user-images.githubusercontent.com/108729047/215579707-cb2cd05f-a83c-4255-876a-e0c72fe1d7f9.png)  
해당 그림처럼 필터의 크기가 커질수록 원본의 형태와 더 달라짐.  

## Mixing images  
여러 이미지를 섞어 새로운 이미지 데이터를 만드는 방법.  
Image mixing은 매우 적은 양의 데이터에서 사용할 시 오히려 오버피팅이 될 수도 있음.  
### Linear Methods
한 사진에 가중치를 둔 후 명암을 조절하여 자연스럽게 이미지를 합성하여 새로운 이미지를 생성.(Mix Up 방식이라고도 불림)  
New1 : A 이미지 명암 0.3, B 이미지 명암 0.7  
New2 : A 이미지 명암 0.7, B 이미지 명암 0.3  
명암에 따라 다르게 인식.  
### Non-Linear Methods  
랜덤하게 자른 두 사진을 부분부분 이어붙여 새로운 이미지를 생성.  
사람이 확인해도 class를 정확하게 구별하여 라벨링하기 어려울 수 있으므로 가중치의 역할이 중요.  
![image](https://user-images.githubusercontent.com/108729047/215586252-1a15fba0-1925-4c24-8d28-fa90421777d2.png)  

## Random Erasing
이미지의 특정부분을 랜덤하게 지워서 이미지를 증강시키는 방법.  
![image](https://user-images.githubusercontent.com/108729047/215584873-5ffd60d1-05e4-4f3e-8530-ffafff850b50.png)  
이미지가 가지는 특성의 일부를 없애기 때문에 오버피팅을 줄일 수 있지만, 너무 많이 지워버리면 학습 성능이 낮아짐.  
MNIST 6과 8 상단을 자르면 구분이 안되는 것처럼 레이블을 보존하지 않기 때문에 수동 개입이 필요할 수도 있음.  
![image](https://user-images.githubusercontent.com/108729047/215585203-c6ef7410-3771-4a9d-b7c1-aeaec2404727.png)  
지우고 싶은 범위 (배경, 물체 랜덤 정도, 지우는 크기) 설정 가능.  



# Deep Learning Approaches (딥러닝 기반의 데이터 증강)
## Feature space Augmentation
## Adversarial Training
## GAN based Augmentation
## Neural style Transfer



# Meta Learning Approaches (메타 기반의 데이터 증강)
## Auto Augment
## Population Based Augment
## Fast Auto Augment
## Faster Auto Augment
## Rand Augment
## Uniform Augment
