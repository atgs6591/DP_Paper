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
# 목적
![image](https://user-images.githubusercontent.com/108729047/215593836-9917d0d9-f106-4bbf-83d1-b91674064ac3.png)  
Epoch가 증가할수록 Accuracy도 증가해야하는데, 어느 순간 Accuracy가 줄어드는 순간 존재. (오버피팅)  
오버피팅의 근본적인 문제는 training set이 작다는 것, 이를 해결하기 위해 data augmentation이 필요.

# 주의
data augmentation을 과하게 진행하면 데이터의 본질이 흐려질 수 있음.  
데이터의 양을 늘리는 것도 중요하지만 데이터의 중요 특징이 사라지면 안됨.  

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
특징을 추출하여 이미지를 증강시키는 방법.  
![image](https://user-images.githubusercontent.com/108729047/215595436-d51c64aa-4f5f-43ab-b0f2-ed9b201d17b0.png)  
기존 데이터를 encoder를 이용하여 저차원 벡터로 표현하고 decoder를 이용하여 다시 원래 이미지로 재구성. 이 과정에서 이미지의 feature 추출.    
해당 이미지의 feature를 재조합하여 새로운 이미지 구성.  
SMOTE(K-NN)도 해당 기법 이용.  
이미지 픽셀을 섞는게 아니라 feature space에서 값을 섞어서 이미지의 특징을 더 잘 보존할 수 있음.  

## Adversarial Training
훈련을 방해하는 노이즈를 추가하여 모델의 약점을 강화하시키기 위한 목적으로 데이터를 증강시키는 방법.  
![image](https://user-images.githubusercontent.com/108729047/215598912-9e2c6265-c91c-485b-b486-b3012f59623b.png)  
원본 이미지에 손실 함수로 만든 노이즈를 적용. 사람의 눈에는 노이즈가 보이지 않지만 오분류(adversarial attack)가 일어남.  
해당 모델을 train data에 넣어 학습시킴.  
모델의 정확도 향상에 도움을 주지는 않지만, 모델의 오분류를 막는데에는 도움을 줌.  

## GAN based Augmentation
Adversarial Training의 확장판.  
GAN은 그럴듯한 가짜를 만드는 모델이다. 실제 있을법한 이미지를 생성.  
Generator과 Discriminator를 통해 가짜를 만드는 기술과 가짜를 구별하는 기술을 동시에 발전시키는 기술.  
Generator(생성자)가 실제 데이터에 노이즈를 적용하여 Fake Data를 만들어내며, Discriminator(구분자)가 실제 데이터와 생성자가 생성해낸 가짜 데이터를 구별.  
![image](https://user-images.githubusercontent.com/108729047/215658065-e5585f71-8602-4852-82ec-bab32933fe12.png)  


## Neural style Transfer
CNN을 이용하여 이미지 P를 A의 스타일을 가진 새로운 X로 만드는 방법.  
![image](https://user-images.githubusercontent.com/108729047/217041422-8eb6d5b1-e0c1-413c-8482-c5d21c5796df.png)  

CNN을 이용하여 이미지의 특성을 표현하는 feature map을 생성하고 이를 이용하여 두 이미지를 합성.  
transfer을 이용하여 이미지를 심층 신경망을 거쳐 새로운 스타일로 변형한 후 다른 이미지를 합성하여 새로운 이미지를 생성.  

![image](https://user-images.githubusercontent.com/108729047/217042342-1f7757cc-a620-4698-bfcc-4ca2e328c724.png)  

이미지는 크게 Content Image와 Style Image로 나뉘며, Content는 변경할 이미지 Style은 특성을 가져올 이미지.  
layer 층이 낮으면 기존의 이미지와 비슷하고 layer 층이 깊어질수록 style의 feature가 더 잘 적용되어 색다른 이미지가 생성됨.  



# Meta Learning Approaches (메타 기반의 데이터 증강)
신경망으로 신경망을 최적화하는 개념을 의미.  신경망을 이용하여 이미지 혼합, neuran transfer, 기하학적 변환 등을 통해서 스스로 데이터 증강하는 방법을 학습  

## Neural augmentation
![image](https://user-images.githubusercontent.com/108729047/216520735-4c6e3319-9753-4202-99be-211d8e56ccef.png)  
스타일과 콘텐츠 손실의 가중치에 댇한 매개변수 
동일한 클래스에서 두 개의 이미지를 가져오고  Neural Style Transfer를 통해 다른 임의의 이미지로 변환
만들어진 이미지가 분류모델에 입력되고 오류를 가져와서 가중치를 수정하여 역전파에 넣어줌
서로 다른 이미지 간의 콘텐츠 및 스타일 이미지에 대한 최적의 가중치와 CNN의 이미지 간의 매핑을 학습.   기존의 증강 기술과 뉴런스타일 접근 방식을 결합한 meta learning이 가장 가능성 있는 증강 기술이라고 생각

## Smart Augmentation
Neural style transfer가 아닌 cnn의 학습된 매개변수만 기존의 증강 기술에 적용한 방법.  


## Auto Augment
강화학습 기반의 알고리즘 이미지 증강.
