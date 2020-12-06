## Attention Mechanism

기존 seq2seq 모델의 문제점

* context vector(압축된 정보)를 고정된 크기의 벡터로 표현하려니까 정보 손실이 발생

* RNN의 고질적인 문제인 Vanishing Gradient 문제가 존재



#### Attention의 아이디어

* 디코더에서 출력 단어를 예측하는 매 시점(time step)마다, 인코더에서의 전체 입력 문장을 다시 한 번 참고

* 전체 입력 문장을 전부 다 동일한 비율로 참고하는 것이 아니라, 해당 시점에서 예측해야할 단어와 연관이 있는 입력 단어 부분을 좀 더 집중(attention)

* 함수로 표현하면 **Attention(Q, K, V) = Attention Value**
  * Q = Query : t 시점의 디코더 셀에서의 은닉 상태 

  * K = Keys : 모든 시점의 인코더 셀의 은닉 상태들 
  * V = Values : 모든 시점의 인코더 셀의 은닉 상태들

#### 

#### Dot-Product Attention

![img](https://wikidocs.net/images/page/22893/dotproductattention1_final.PNG)



##### 1) Attention Score를 구한다.

* 인코더의 모든 은닉 상태 각각이 디코더의 현 시점의 은닉 상태와 얼마나 유사한지를 판단하는 스코어값

$$
score(s_t, h_i)=s^T_th_i
$$

$$
e^t=[s^T_th_1,...,s^T_th_N]
$$

##### 2) softmax를 이용하여 Attention Distribution 구하기

$$
α^t=softmax(e^t)
$$

##### 3) Attention Value를 구한다

$$
a_t=∑_i^Nα^t_ih_i
$$

 Attention Value는 종종 인코더의 문맥을 포함하고 있다고하여, **컨텍스트 벡터(context vector)**라고도 불림, seq2seq에서의 context vector와는 다름



##### 4) 어텐션 값과 디코더의 t시점의 은닉 상태를 연결(Concatenate)

$$
a_t를 s_t와 결합, 이를 v_t라고 정의
$$

그리고 이 vt를 y 예측 연산의 입력으로 사용하므로써 인코더로부터 얻은 정보를 활용하여 y를 좀 더 잘 예측할 수 있게 된다



##### 5) 출력층의 연산에 입력이 되는 st를 계산

![img](https://wikidocs.net/images/page/22893/st.PNG)
$$
s^t=tanh(W_c[α_t;s_t]+b_c)
$$
Wc는 학습 가능한 가중치 행렬, bc는 편향