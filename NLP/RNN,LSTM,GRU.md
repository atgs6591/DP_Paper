# RNN,LSTM,GRU
결과값이 출력층으로만 향하지 않고, 출력층과 동시에 다시 은닉층의 다음 계산 입력으로 들어감
### Sequence?
연속적인 입력(Sequential Input)으로부터 연속적인 출력(Sequential Output)을 생성하는 모델

![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/ef478ec7-8ef7-4909-bd2b-9b6a43a47653)  
- Xt : 입력 벡터  
- Yt : 출력 벡터  
- cell : 은닉층에서 결과를 내보내는 역할을 하는 노드  

RNN에서는 cell이 이전의 값을 기억하는 메모리의 역할 수행. (메모리 셀, RNN 셀)  
  




## 순환 신경망(Recurrent Neural Network, RNN)
- 내부에 순환 구조가 포함되어 있는 가장 기본적인 sequence 모델

![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/1c921eca-02c1-4a99-aebd-94965b5820c7)  
> t를 순서대로 펼친 그림  

### RNN 구조
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/2c67b3f1-6509-4b66-8c61-264b9a77dcca) ![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/566e5e7e-373f-4c1c-99b7-d0da90eb5f84)
  
- h는 메모리 셀에서의 출력 값
- y는 해당 시점에서의 출력
- u(입력층을 위한 은닉층 가중치), v(출력층 가중치), w(이전 시점에서의 메모리 셀을 위한 은닉층 가중치)는 각각 가중치  

은닉층 : 가중치 2개 사용  
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/844aa8c3-8a19-48c0-aae2-9072e45bfd47)  
출력층 : 한개의 가중치 사용  
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/b03f1058-405a-4235-955c-3b7df91e6d7a)  

  
### RNN의 Short-term dependencies  
t는 t-1에서 전달된 정보에 의존  
=> 비교적 짧은 시퀀스에 대해서만 높은 효과를 보임.

![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/2ca10204-6d7e-466f-b974-7eed83562b9c)  
역방향으로 미분을 곱해 나가면서 계산을 진행하기 때문에, 시간의 크기가 커지면 기울기가 불안해지는 문제 발생.  


## 장단기 메모리(Long Short-Term Memory, LSTM)
- cell state를 추가하여 불필요한 기억은 지우고, 필요한 부분을 기억.  
- 멀리 떨어진 과거의 정보도 현재에 영향을 주도록 고안.  

![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/ceb31a9b-d39b-4e5c-966c-0c45c6b332f9)


## 게이트 순환 유닛(Gated Recurrent Unit, GRU)
