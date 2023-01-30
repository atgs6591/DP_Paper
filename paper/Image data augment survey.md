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
해당 논문에는 Basic Image Manipulation과 Deep Learning Approaches만 설명  

# Basic Image Manipulation (기본 이미지 조작을 기반으로 한 데이터 확대)
## Geometric transformations (기하 변환)
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
# Deep Learning Approaches (딥러닝 기반의 데이터 증강)
