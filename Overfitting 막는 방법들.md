# Overfitting 막는 방법들

* 데이터의 양을 늘리기

* 모델의 복잡도 줄이기

  * hidden layer, 파라미터 줄이기

* Regularization

  * L1 : 비용 함수에 가중치w들의 절대값 합계를 추가 λ∣w∣
  * L2 : 비용 함수에 가중치w들의 제곱합을 추가 1/2λw^2

  비용 함수를 최소화하기 위해서는 가중치 w들의 값이 작아져야 한다는 특징이 있음. 람다가 크다면 모델이 훈련 데이터에 대해서 적합한 파라미터를 찾는 것보다 규제를 위해 추가된 항들을 작게 유지하는 것을 우선한다는 의미

* Dropout

  ```shell
  model = Sequential()
  model.add(Dense(256, input_shape=(max_words,), activation='relu'))
  model.add(Dropout(0.5)) # 드롭아웃 추가. 비율은 50%
  model.add(Dense(128, activation='relu'))
  model.add(Dropout(0.5)) # 드롭아웃 추가. 비율은 50%
  model.add(Dense(num_classes, activation='softmax'))
  ```

  