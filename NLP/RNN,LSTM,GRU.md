# RNN,LSTM,GRU
결과값이 출력층으로만 향하지 않고, 출력층과 동시에 다시 은닉층의 다음 계산 입력으로 들어감
### Sequence?
연속적인 입력(Sequential Input)으로부터 연속적인 출력(Sequential Output)을 생성하는 모델

### 기초
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/ef478ec7-8ef7-4909-bd2b-9b6a43a47653)  
- Xt : 입력 벡터  
- Yt : 출력 벡터  
- cell : 은닉층에서 결과를 내보내는 역할을 하는 노드  

RNN에서는 cell이 이전의 값을 기억하는 메모리의 역할 수행. (메모리 셀, RNN 셀)  
  
  
## 순환 신경망(Recurrent Neural Network, RNN)
- 내부에 순환 구조가 포함되어 있는 가장 기본적인 sequence 모델

![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/1c921eca-02c1-4a99-aebd-94965b5820c7)  

### RNN의 Short-term dependencies
![image](https://github.com/mjkim0819/DP_Paper/assets/108729047/2c67b3f1-6509-4b66-8c61-264b9a77dcca)  
RNN을 시간순으로 풀면 t는 t-1에서 전달된 정보에 의존함을 알 수 있음.


- 은닉층에서 출력층 방향으로만 향하지 않

## 장단기 메모리(Long Short-Term Memory, LSTM)
## 게이트 순환 유닛(Gated Recurrent Unit, GRU)
