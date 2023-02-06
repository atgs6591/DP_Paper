# Learning Data Augmentation Strategies for Object Detection(2019)

# Abstract
data augmentation을 과하게 진행하면 데이터의 본질이 흐려질 수 있음.  
데이터의 양을 늘리는 것도 중요하지만 데이터의 중요 특징이 사라지면 안됨.  

# Introduction

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
