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

![image](https://user-images.githubusercontent.com/108729047/217146361-d5f4d7a0-3521-40e5-a712-ace0e862ab00.png)

### Flipping
가장 쉬운 데이터 증강 방법.  
이미지를 수평, 수직을 기준으로 뒤집어서 이미지를 늘리는 방식. 주로 horizontal axis (수평 축)이 vertical axis (수직 축)보다 일반적. 

### Rotation  
1~359도로 이미지를 돌리는 방법. 회전 정도에 따라 안전성이 달라짐. 주로 1에서 20, -1에서 -20 정도만 rotation함.



# referance
https://baekchef1215.tistory.com/58
https://deep-learning-study.tistory.com/705
https://hellopotatoworld.tistory.com/5
https://velog.io/@ailab/33l8nmxm
https://www.arxiv-vanity.com/papers/1906.11172/
