# AutoAugment: Learning Augmentation Strategies from Data (2019)
2019년 Google Brain 논문, NAS와 동일한 방식으로 유용한 Data Augmentation을 찾는 과정을 제안.

## Introduction
data augmentation은 dataset에 따라 학습에 효과적인 방식이 모두 다르다. ( CIFAR10과 MNIST의 Vertical Flipping 성능 )
![image](https://user-images.githubusercontent.com/108729047/220165345-ccd9714e-291e-42a5-8474-e31c5780ac59.png)  
Machine Learning과 Computer Vision 분야는 효과적인 모델 아키텍처를 개발하기 위해 노력하지만, 상대적으로 data augmentation 분야는 더 많은 dataset에 효과적인 방식을 찾는데에 큰 노력을 기울이지 않는다.  

해당 논문의 목표는 효과적인 data augmentation 방식을 자동화하는 것이며, 강화학습을 사용한다.   
- taget 데이터에 적합한 policy를 탐색한다.
- 학습 된 policy룰 다른 데이터로 전이시킨다.  

### 추가로 찾아 본 개념
#### policy란?  
어떤 상황에서 학습 대상(agent)이 취할 행동을 확률로 명시한 값. 강화학습의 개념을 알아야 한다.  
강화학습은 학습 대상이 시행착오를 통해 학습하는 방법으로, 실수와 보상을 통하여 원하는 목표를 찾아가는 알고리즘이다.  
좋은 policy를 찾는 것이 강화학습의 목표이며, Value function(가치 함수)는 policy를 찾아가는 길잡이 역할을 한다.  


## AutoAugment
![image](https://user-images.githubusercontent.com/108729047/220226851-354bdb9a-d8e4-4ef9-a218-cf80d46b248d.png)  
AutoAugmentation은 크게 Search Space와 Search Algorithm으로 구성된다. (탐색 공간, 탐색 알고리즘)  
탐색알고리즘이 적합한 policy 탐색 -> 해당 policy로 network 학습 -> validation set에 대한 accuracy 정보가 RNN Controller로 전달 -> 보상에 맞게 강화학습하여 효과적인 augmentation policy 탐색  

(1) Augmentation 기법을 출력하는 RNN controller 생성.  

(2) 이를 통해 얻은 augmentation 기법을 학습 데이터에 적용.  

(3) 모델의 학습 및 성능을 평가하여 reward(R)를 얻음.  

(4) 계산된 reward를 통해 RNN controller 학습.  


### 추가로 찾아 본 개념
#### NAS란?
![image](https://user-images.githubusercontent.com/108729047/220226198-4c313805-60ed-49cd-9958-dbaa92dc1f6b.png)  


### Search space details (알고리즘이 탐색을 수행하는 공간)
search space에서는 5개의 sub-policy가 존재하고, 하나의 sub-policy는 2개의 이미지 작업을 구성한다. 
subpolicy : 2개의 augmentation 조합을 찾고, 두 augmentation을 몇 퍼센트의 확률로 적용할지와, magnitude를 가진다.  

![image](https://user-images.githubusercontent.com/108729047/220226478-47b80eaa-3cec-49ff-8c76-102191200f32.png) 
#### sub-policy 내의 이미지 작업을 위한 hyper-parameter
- 이미지 작업을 적용할 확률 : probability  
- 이미지 작업이 적용되는 크기 (정도) : magnitude  

hyper-parameter로 인해 같은 sub-policy라도 다 다르게 작업을 수행한다.  

Sub-Policy1 : ShearX(가로로 기울이기)가 0.9의 확률로 7/10의 크기로 적용되고, 이후 Invert(반전)가 0.8의 확률로 적용.
(두번째로 적용되는 이미지 작업은 1-x의 확률을 가지며 크기는 적용되지 않음)


### Search algorithm details

#### 탐색 알고리즘을 위해 필요한 두 가지
- RNN Controller (순환 신경망)
- Proximal Policy Optimization training algorithm (훈련 알고리즘)
  
각 단계에서 컨드롤러가 softmax에 의해 결정을 예측한다.  
controller는 각각 2개의 operation을 가진 5개의 sub-policy를 결정하기 위해 30번의 softmax prediction을 수행하게된다.  
5(하위 정책) x ( 2(보강 방법) + 2(확률) + 2(크기) ) = 30  
 

### The training of controller RNN
어떤 policy가 childmodel의 일반화를 개선하는데 도움이 되는지 학습하는 과정이다.  

validation set에 하나의 mini-batch마다 5개의 sub-policy 중 하나가 무작위로 적용되며, 이 validation set으로 childmodel을 훈련하고 test set으로 측정하여 accuracy를 구한다.  

controller는 이때의 accuracy를 보상 신호로 받으며 더 좋은 policy를 찾기 위해 훈련한다.  (controller의 data = 각 sub-policy의 accuracy)    

child model이 좋은 경우 controller에서 높은 확률을 할당하고, child model이 좋지 않은 경우 controller에서 낮은 확률을 할당하여 controller의 RNN의 기울기를 조정한다.  


## Experiments and Results

### AutoAugment-direct
AutoAugment를 dataset에 직접적으로 적용했을때 기존에 알려진 기초적인 augmentation 방법을 적용한 경우와의 성능 차이를 비교하는 task라고 할 수 있다.  
CIFAR-10, CIFAR-100, SVHN, ImageNet dataset을 사용했다.  
![image](https://user-images.githubusercontent.com/108729047/220237060-886081c3-5c01-4715-8528-5e5c7e0aed7d.png)


#### CIFAR10
best policy : color-based transformation  
ShearX, ShearY는 적합하지 않고, Invert는 제외되었다.  
Shake-drop 모델이 error rate가 1.5%까지 되는 매우 좋은 성능을 가진다. (기존 sota보다 0.6%을 넘음)  

reduce CIFAR10(데이터 양이 더 적음)이 더 높은 성능을 가진다. (CIFAR-10은 성능 차 0.8, reduce CIFAR-10은 성능 차 3.4)
=> 학습셋의 크기가 작을수록 data augmentation이 잘됨.  

#### CIFAR100
CIFAR10과 마찬가지로 color-based transformation이 best policy, 학습 셋이 작을 수록 성능이 좋음.  
error rate : 10.7%  


#### SVHN (글자 이미지 데이터)
best policy : Invert, Equalize, ShearX/Y, Rotate  
숫자의 색상이 분류에 중요한 영향을 미치지 않기 때문에 policy가 잘 선정되었다는 것을 알 수 있다.  
=> AutoAugmentation이 policy 선정을 잘 하고 있으며, 한 데이터에만 특정되지 않고 general한 feature를 잘 표현하고 있음.

#### ImageNet
best policy : color-based transformation  
AmeobaNet-C model이 83.5 / 96.5(%)로 sota를 달성.  

### AAutoAugment-transfer
과적합 여부를 판단하기 위해 전이 가능성을 확인해보는 실험이다.  
![image](https://user-images.githubusercontent.com/108729047/220239381-b5a8428a-cea1-4bcb-a437-23a0c0fed8c7.png)  

비슷한 도메인의 데이터셋에서도 효과가 있는지 확인하기 위해, ImageNet에 선정되었던 best policy를 매우 작은 규모의 dataset에 적용했다.  

모두 baseline보다 우수한 결과를 얻었으며 Stanford Cars dataset은 sota를 달성했다.  

결론적으로 이 논문에서 중요한점은 Data augmentation 기법의 전이가 가능함을 보인 것이다.

## Conclusion
- 다른 기존의 기본적인 기법들보다 성능이 우수하다.  
- 동일한 search space에서 무작위로 선정한 policy와 비교한 결과 AutoAugmentation이 더 우수하다.
- 직관적인 DA 기법에서 벗어나 효율적인 DA를 찾는 방법을 만들었다.  

- best policy를 선정하기 위해 적어도 80-120 epoch를 학습해야한다. (시간이 오래 걸린다)  
- 몇 천 GPU 시간이 걸린다.  
![image](https://user-images.githubusercontent.com/108729047/220240823-493d5c2d-73ec-4e1b-9af6-0440746bcfa8.png)  





## Referance
### 논문 분석
https://creamnuts.github.io/short%20review/autoaug/
https://jjeamin.github.io/posts/AutoAugment/
https://deepkerry.tistory.com/27

### 강화학습
https://wordbe.tistory.com/entry/RL-%EA%B0%95%ED%99%94%ED%95%99%EC%8A%B5-part1-policy-value-function






## 다음 논문
https://arxiv.org/abs/1909.13719
