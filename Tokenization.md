# Tokenization



### Framework and library

* Tensorflow
* Keras
* Gensim : 머신 러닝을 사용하여 토픽 모델링과 자연어 처리 등을 수행할할 수 있게 해줌
* Scikit-learn



### 자연어 처리를 위한 NLTK와 KoNLPy

* NLTK : 자연어 처리를 위한 파이썬 패키지, 실습에 필요한 코퍼스 제공
* KoNLPy : 한국어 자연어 처리를 위한 형태소 분석기 패키지



### EDA 패키지(Pandas 프로파일링)

```shell
import pandas_profiling

ex.
data = pd.read_csv('spam.csv 파일의 경로',encoding='latin1')
pr = data.profile_report
pr.to_file(./pr_report.html) #html로 저장도 가능
```



### Text preprocessing

#### 1. 토큰화(tokenization)

* 단어 토큰화 : 단어 당 토큰을 생성함, Don't Jone's 같은 경우 애매한데 기존에 공개된 도구들을 사용하여 토큰화 가능.

* nltk의 word_tokenize

  ```shell
  from nltk.tokenize import word_tokenize #[Do, n't] 로 토큰화
  
  word_tokenize(text)
  ```

* nltk의 WordPunctTokenizer

  ```shell
  from nltk.tokenize import WordPunctTokenizer #구두점을 별도로 분류, [Don, "'", t]로 토큰화
  
  WordPunctTokenizer().tokenize(text)
  ```

* Keras의 text_to_word_sequence

  ```shell
  from tensorflow.keras.preprocessing.text import text_to_word_sequence #모든 알파벳을 소문자로 바꾸면서 온점이나 컴마, 느낌표 등의 구두점을 제거, '는 보존, [Don't]
  
  text_to_word_sequence(text)
  ```



#### 2. 토큰화에서 유의사항

* 구두점, 특수 문자를 단순 제외하면 안됨
  * .은 문장의 경계를 알 수 있는데 도움이 되므로 제외하지 않을 수 있음
  * $45.55 같은 경우 $, . 모두 의미있는 기호임

* 표준 토큰화(Penn Treebank Tokenization)

  * 하이픈으로 구성된 단어는 하나로 유지
  * doesn't와 같이 '로 접어가 함께하는 단어는 분리

  ```shell
  from nltk.tokenize import TreebankWordTokenizer
  
  tokenizer = TreebankWordTokenizer()
  tokenizer.tokenize(text)
  ```

  

#### 3. Sentence Tokenization

. ! 등을 이용하여 문장을 토큰화할 수 있지만 완벽하지는 않음

* 영어 문장의 토큰화 : nltk의 sent_tokenize

  ```
  from nltk.tokenize import sent_tokenize
  
  sent_tokenize(text)
  ```

* 한국어 문장 토큰화 : kss이용

  ```
  import kss
  
  kss.split_sentences(text)
  ```

  

#### 4. 이진 분류기

문장 토큰화에서 예외 사항을 발생시키는 온점(.) 처리를 위해 사용, 두개의 클래스가 있음

* 약어로 쓰이는 경우
* 문장의 구분자일 경우



#### 5. 한국어에서의 토큰화

한국어의 경우 띄어쓰기 단위가 되는 단위를 '어절'이라고 하는데 어절 토큰화는 한국어 NLP에서 지양되고 있음. 한국어는 조사, 어미 등을 붙여서 말을 만드는 교착어이기 때문이다. 따라서 형태소 토큰화를 수행해야함

* 형태소(morpheme) : 뜻을 가진 가장 작은 말의 단위
  * 자립 형태소 : 조사, 어미, 조사와 상관없이 자립하여 사용할 수 있는 형태소. 명사, 대명사, 수사, 수식언, 감탄사 등
  * 의존 형태소 : 접사, 어미, 조사, 어간 등 다른 형태소와 결합하여 사용되는 형태소



#### 6. 품사 태깅(Part-of-speech tagging)

해당 단어가 어떤 품사로 쓰였는지 보는 것이 단어의 의미 파악에 주요 지표가 될 수 있음

* 영어. NLTK의 Penn Treebank POS Tags

  ```shell
  from nltk.tokenize import word_tokenize
  from nltk.tag import pos_tag
  
  x = word_tokenize(text)
  pos_tag(x)
  ```

* 한국어.  KoNLPy의 Okt, Mecab, Komoran, Hannanum, Kkma

  ```shell
  from konlpy.tag import Okt #Okt 사용
  okt = Okt()
  okt.morphs(text) #형태소로 토큰화
  
  okt.pos(text) #품사 태깅
  
  okt.nouns(text) #명사 추출
  ```

  ```shell
  from konlpy.tag import Kkma #Kkma 사용
  kkma = Kkma()
  kkma.morphs(text) #형태소 토큰화
  
  kkma.nouns(text) #명사 추출
  ```