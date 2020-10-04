# Artificial Neural Network



### Feed-Forward Neural Network, FFNN

input layer에서 output layer로 연산이 전개되는 신경망을 의미함



### Fully-connected layer

어떤 층의 모든 뉴런이 이전 층의 모든 뉴런과 연결되어 있는 층



### Activation Function

#### 1. 활성화 함수의 특징

* 비선형 함수. 선형 함수로는 hidden layer를 추가하더라도 1회 추가한 것과 차이를 줄 수 없음

  ex. 계단 함수

  ```shell
  import numpy as np
  import matplotlib.pyplot as plt
  
  def step(x):
      return np.array(x > 0, dtype=np.int) # 입력이 0보다 크면 True, 0보다 작으면 False. 그 후 int값으로 바꿈
  x = np.arange(-5.0, 5.0, 0.1) # -5.0부터 5.0까지 0.1 간격 생성
  y = step(x)
  plt.title('Step Function')
  plt.plot(x,y)
  plt.show()
  ```

  ex. 시그모이드 함수

  ```shell
  def sigmoid(x):
      return 1/(1+np.exp(-x))
  x = np.arange(-5.0, 5.0, 0.1)
  y = sigmoid(x)
  
  plt.plot(x, y)
  plt.plot([0,0],[1.0,0.0], ':') # 가운데 점선 추가
  plt.title('Sigmoid Function')
  plt.show()
  ```

  시그모이드 함수는 Vanishing Grandient 문제가 발생

  ex. 하이퍼볼릭탄젠트 함수

  ```shell
  x = np.arange(-5.0, 5.0, 0.1) # -5.0부터 5.0까지 0.1 간격 생성
  y = np.tanh(x)
  
  plt.plot(x, y)
  plt.plot([0,0],[1.0,-1.0], ':')
  plt.axhline(y=0, color='orange', linestyle='--')
  plt.title('Tanh Function')
  plt.show()
  ```

  시그모이드와 마찬가지로 Vanishing 문제 발생. 하지만 중심값이 0으로 시그모이드 함수에 비해 반환값의 변화폭이 더 큼. 따라서 시그모이드 함수보다는 Vanishing 문제가 덜 발생.

  ex. ReLU

  ```shell
  def relu(x):
      return np.maximum(0, x)
  
  x = np.arange(-5.0, 5.0, 0.1)
  y = relu(x)
  
  plt.plot(x, y)
  plt.plot([0,0],[5.0,0.0], ':')
  plt.title('Relu Function')
  plt.show()
  ```

  연산속도 빠름. 성능도 꽤 좋음. 근데 입력이 음수면 0으로 죽어버림. leaky ReLU로 보완

  ex. Leaky ReLU

  ```shell
  def leaky_relu(x):
      return np.maximum(a*x, x)
  
  x = np.arange(-5.0, 5.0, 0.1)
  y = leaky_relu(x)
  
  plt.plot(x, y)
  plt.plot([0,0],[5.0,0.0], ':')
  plt.title('Leaky ReLU Function')
  plt.show()
  ```

  ex. Softmax

  ```shell
  x = np.arange(-5.0, 5.0, 0.1) # -5.0부터 5.0까지 0.1 간격 생성
  y = np.exp(x) / np.sum(np.exp(x))
  
  plt.plot(x, y)
  plt.title('Softmax Function')
  plt.show()
  ```

  소프트맥스는 시그모이드 함수와 함께 output layer에서 주로 사용됨. 시그모이드는 이진 소프트맥스는 다중 클래스 분류에 주로 사용됨



### Forward Propagation

Keras 사용

```shell
from keras.models import Sequential
from keras.layers import Dense

model = Sequential() # 층을 추가할 준비
model.add(Dense(8, input_dim=4, init='uniform', activation='relu'))
# 입력층(4)과 다음 은닉층(8) 그리고 은닉층의 활성화 함수는 relu
model.add(Dense(8, activation='relu')) # 은닉층(8)의 활성화 함수는 relu
model.add(Dense(3, activation='softmax')) # 출력층(3)의 활성화 함수는 softmax
```

입력층 : 4개의 입력과 8개의 출력
은닉층1 : 8개의 입력과 8개의 출력
은닉층2 : 8개의 입력과 3개의 출력
출력층 : 3개의 입력과 3개의 출력