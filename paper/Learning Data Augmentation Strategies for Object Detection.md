# Learning Data Augmentation Strategies for Object Detection(2019)

# Abstract
추가 작성하기

# Introduction
data augmentation은 학습 데이터가 부족하거나, 데이터 증강을 사용하여 모델 성능을 개선할 때 사용한다.  
![image](https://user-images.githubusercontent.com/108729047/217146961-4697ac2f-fa57-41e6-9b73-d2cf53fbf770.png)  
이미지 분류에서는 그냥 원하는대로 이미지를 증강시킬 수 있지만, Object detection에서는 객체의 위치를 bounding box에 기반해서 검출해야하기 때문에 고려해야할 것이 더 많다.

# Method
+ Color operations : 이미지의 color 값을 변환하되, bounding box의 위치 좌표값에는 변화를 주지 않는다
ex) Equalize, Contrast, Brightness...  

+ Geometric operations : 이미지의 위치정보를 바꾸며, bounding box annotation의 위치, 사이즈를 같이 변화시킨다
ex) Rotate, ShearX, TranslationY...  

+ Bounding box operations : 이미지 내에세 bounding box annotation이 있는 부분의 픽셀만 변화시킨다
ex) BBox_Only_Equalize, BBox_Only_Rotate, BBox_Only_FlipLR ...


## 계산 
L : transformation이 적용될 크기 (회전 각도, 리사이즈 크기 등)
M : transformation이 적용될 확률
K : 학습 할 sub-policy의 개수


![image](https://user-images.githubusercontent.com/108729047/217147431-a015cc44-6fd8-456f-a647-586513d73538.png)  

각각 N개의 순차적인 transformation operands를 갖는 K개의 sub-policy를 학습하고 training 과정에서 각 이미지에 적용될 policy가 랜덤으로 선택된다. Figure3는 K=5, N=2일 때의 Search Space이고, 각각의 operands는 다음 내용에 해당하는 총 3개의 파라미터(predictions)를 가진다.  
![image](https://user-images.githubusercontent.com/108729047/217146361-d5f4d7a0-3521-40e5-a712-ace0e862ab00.png)  


# referance
https://baekchef1215.tistory.com/58
https://deep-learning-study.tistory.com/705
https://hellopotatoworld.tistory.com/5
https://velog.io/@ailab/33l8nmxm
https://www.arxiv-vanity.com/papers/1906.11172/
