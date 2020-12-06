## Transformer

seq2seq의 구조인 인코더-디코더를 따르면서도, 논문의 이름처럼 어텐션(Attention)만으로 구현한 모델



#### seq2seq의 한계

* context vector를 만드는 과정에서 정보 손실

이를 보정하기 위해 어텐션이 사용, 어텐션을 RNN의 보정을 위한 용도가 아니라 아예 어텐션으로 인코더와 디코더를 만들어보면 어떨까?



#### Transformer의 하이퍼파라미터

* d_model = 512 : 인코더와 디코더에서의 정해진 입력과 출력의 크기
* num_layers = 6 : 인코더와 디코더가 총 몇 층으로 구성되어 있는지
* num_heads = 8 : 몇개로 분할해서 병렬로 어텐션을 수행할 것인지
* d_ff = 2048 : 트랜스포머 내부에는 피드 포워드 신경망이 존재, 이때 은닉층의 크기를 의미



#### Transformer

![img](https://wikidocs.net/images/page/31379/transformer2.PNG)

이미지 출처 : https://wikidocs.net/45609



#### Positional Encoding

RNN이 자연어 처리에서 유용했던 이유는 단어의 위치에 따라 단어를 순차적으로 입력받아서 처리하는 RNN의 특성으로 인해 각 단어의 위치 정보(position information)를 가질 수 있다는 점에 있음

트랜스포머는 단어 입력을 순차적으로 받는 방식이 아니므로 단어의 위치 정보를 다른 방식으로 알려줄 필요가 있음

각 단어의 임베딩 벡터에 위치 정보들을 더하여 모델의 입력으로 사용하는데, 이를 포지셔널 인코딩(positional encoding)이라고 한다.

![img](https://wikidocs.net/images/page/31379/transformer6_final.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

#### Attention

![img](https://wikidocs.net/images/page/31379/attention.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 



![img](https://wikidocs.net/images/page/31379/transformer_attention_overview.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

#### Encoder

![img](https://wikidocs.net/images/page/31379/transformer9_final_ver.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

* 멀티 헤드 셀프 어텐션은 셀프 어텐션을 병렬적으로 사용하였다는 의미

* 포지션 와이즈 피드 포워드 신경망은 우리가 알고있는 일반적인 피드 포워드 신경망



#### 인코더의 셀프 어텐션

**Attention(Q, K, V) = Attention Value**

* Q : 입력 문장의 모든 단어 벡터들
* K : 입력 문장의 모든 단어 벡터들
* V : 입력 문장의 모든 단어 벡터들



##### 1) Q,K,V 벡터 얻기

d_model의 차원을 가졌던 각 단어 벡터들을 d_model / num_heads 차원의 벡터들로 변환함

##### 2) Scaled dot-product Attention

$$
score(q,k)=q⋅k/sqrt(n)
$$

##### 3)행렬 연산으로 일괄 처리하기

![img](https://wikidocs.net/images/page/31379/transformer12.PNG)

이미지 출처 : https://wikidocs.net/45609

#####  

![img](https://wikidocs.net/images/page/31379/transformer15.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

![img](https://wikidocs.net/images/page/31379/transformer16.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 4) 멀티 헤드 어텐션

한 번의 어텐션을 하는 것보다 여러번의 어텐션을 병렬로 사용하는 것이 더 효과적이라고 판단

따라서 d_model을 num_heads개로 나누어 병렬 어텐션을 수행

각각의 어텐션 값 행렬을 어텐션 헤드라고 부름

어텐션을 병렬로 수행하여 다른 시각으로 정보들을 수집하겠다는 의미임

병렬 어텐션을 모두 수행하였다면 모든 어텐션 헤드를 연결(concatenate),  크기는 (seq_len, d_model)

어텐션 헤드를 모두 연결한 행렬은 또 다른 가중치 행렬 Wo을 곱함, 이렇게 나온 결과 행렬이 멀티-헤드 어텐션의 최종 결과물

![img](https://wikidocs.net/images/page/31379/transformer18_final.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

##### 5) 패딩 마스크

입력 행렬에서 필요없는 위치에 아주 작은 값을 넣어주는 것을 의미함



##### 6) 포지션-와이즈 피드 포워드 신경망

![img](https://wikidocs.net/images/page/31379/positionwiseffnn.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

x는 앞서 멀티 헤드 어텐션의 결과로 나온 (seq_len, dmodel)의 크기를 가지는 행렬을 말함

가중치 행렬 W1은 (dmodel, dff)의 크기를 가지고, 가중치 행렬 W2은 (dff, dmodel)의 크기를 가짐



![img](https://wikidocs.net/images/page/31379/transformer20.PNG)

이미지 출처 : https://wikidocs.net/45609



##### 7) 잔차 연결과 층 정규화

* 잔차 연결 : 입력과 출력을 더하는 것을 말함

  ![img](https://wikidocs.net/images/page/31379/transformer22.PNG)

  이미지 출처 : https://wikidocs.net/45609

  ##### H(x)=x+Multi head Attention(x)

* 층 정규화

  ![img](https://wikidocs.net/images/page/31379/layer_norm_new_1_final.PNG)
  
  이미지 출처 : https://wikidocs.net/45609
  
  ##### 

![image-20201121171708825](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20201121171708825.png)

#### Decoder

![img](https://wikidocs.net/images/page/31379/%EB%94%94%EC%BD%94%EB%8D%94.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

##### 1) Masked Multi-head Self-Attention

encoder에서 넘어오는 정보에 과거 시점뿐만 아니라 미래 시점의 단어들도 참조 가능함

이를 막기 위해 look-ahead mask를 도입

미리보기에 대한 마스크라고 할 수 있음

![img](https://wikidocs.net/images/page/31379/%EB%A3%A9%EC%96%B4%ED%97%A4%EB%93%9C%EB%A7%88%EC%8A%A4%ED%81%AC.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 

자신보다 미래에 있는 단어들은 참고x



##### 2) encoder-decoder attention

![img](https://wikidocs.net/images/page/31379/%EB%94%94%EC%BD%94%EB%8D%94%EB%91%90%EB%B2%88%EC%A7%B8%EC%84%9C%EB%B8%8C%EC%B8%B5%EC%9D%98%EC%96%B4%ED%85%90%EC%85%98%EC%8A%A4%EC%BD%94%EC%96%B4%ED%96%89%EB%A0%AC_final.PNG)

이미지 출처 : https://wikidocs.net/45609

##### 