## BERT(논문)

트랜스포머의 양방향 인코더 표현을 의미

사전 훈련된 BERT 모델은 작업별 구조 수정을 크게 할 필요 없이 질문 답변과 언어 추론과 같은 넓은 분야의 작업에서 state-of-the-art 모델을 만들기 위해 하나의 출력 레이어만 추가만 해도 파인튜닝될 수 있음

#### Pre Training

Multi Layered Perceptron에서 Weight와 Bias를 잘 초기화 시키는 방법

* fine-tuning : 기존에 학습되어져 있는 모델을 기반으로 목적에 맞게 학습을 업데이트하는 방법
  * ex. 고양이와 개 분류기를 만드는데 다른 데이터로 학습된 모델을 가져다 쓰는 경우
* feature based : 특정 task를 수행하는 network에 pre-trained language representation을 추가적인 feature로 제공

둘 차이점 : fine-tuning은 임베딩까지 업데이트, feature based는 임베딩은 그대로고 레이어만 업데이트



