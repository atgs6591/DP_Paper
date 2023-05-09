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
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/2c67b3f1-6509-4b66-8c61-264b9a77dcca)  
- h는 메모리 셀에서의 출력 값
- y는 해당 시점에서의 출력
- u(입력층을 위한 가중치), v(편향), w(이전 시점에서의 메모리 셀을 위한 가중치)는 각각 가중치  
은닉층에서는 가중치가 2개 사용
출력층에서는 한개의 가중치 사용
  
### RNN의 Short-term dependencies  
t는 t-1에서 전달된 정보에 의존  
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/20468995-f43d-43f1-a897-b35f3f152e6d)  
> 직관적인 RNN 계층  

**예시**  
RNN에서 h1은 출발점 h0과 input x1에 신경망의 weight를 곱하고, 활성함수의 넣은 값을 사용해 생성된다.  
이것이 중첩되면 ht에서 h0의 영향은 활성함수와 weight를 t번 거친 값을 갖게 된다.  
이때 활성함수로 ReLU를 사용한다면, 양수 W가 계속 곱해져서 W의 t제곱을 h0에 곱하게 된다. 즉 h0가 지수적으로 커져 exploding gradient(쉽게 말해 network가 폭발하는 것)가 발생한다.  
반면에 활성함수로 sigmoid(실수 입력값을 0~1 사이의 미분 가능한 수로 변환)를 사용한다면, h0은 매우 작아져 의미가 없어지게 된다.  



## 장단기 메모리(Long Short-Term Memory, LSTM)
## 게이트 순환 유닛(Gated Recurrent Unit, GRU)
