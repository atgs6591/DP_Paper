# AutoAugment: Learning Augmentation Strategies from Data (2019)
2019년 Google Brain 논문, NAS와 동일한 방식으로 유용한 Data Augmentation을 찾는 과정을 제안.

## Introduction
data augmentation은 dataset에 따라 학습에 효과적인 방식이 모두 다르다. ( CIFAR10과 MNIST의 Vertical Flipping 성능 )
![image](https://user-images.githubusercontent.com/108729047/220165345-ccd9714e-291e-42a5-8474-e31c5780ac59.png)  
Machine Learning과 Computer Vision 분야는 효과적인 모델 아키텍처를 개발하기 위해 노력하지만, 상대적으로 data augmentation 분야는 더 많은 dataset에 효과적인 방식을 찾는데에 큰 노력을 기울이지 않는다.  

모든 데이터에 효과적인 data augmentation 을 찾을수는 없으므로, automatically learned data augmentation을 제안한다.  
- taget 데이터에 적합한 policy를 탐색한다.
- 학습 된 policy룰 다른 데이터로 전이시킨다.  

### 추가로 찾아 본 개념
#### policy란?  
학습 대상(agent)이 할 행동. 강화학습의 개념을 알아야 한다.  
강화학습은 학습 대상이 시행착오를 통해 학습하는 방법으로, 실수와 보상을 통하여 원하는 목표를 찾아가는 알고리즘이다.  




## Related Work




## AutoAugment

### Search space details

### Search algorithm details

### The training of controller RNN

### Architecture of controller RNN and training hyperparameters




## Experiments and Results

### CIFAR10

### CIFAR100

### SVHN

### ImageNet




## Abalation Studies



## Referance
https://creamnuts.github.io/short%20review/autoaug/
https://jjeamin.github.io/posts/AutoAugment/
https://deepkerry.tistory.com/27
