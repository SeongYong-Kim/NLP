# Cleaning and Normalization

* 정제(cleaning) : 코퍼스로부터 노이즈 데이터를 제거
* 정규화(normalization) : 표현 방법이 다른 단어들을 통합시켜 같은 단어로 만듦



### 1. 규칙에 기반한 표기가 다른 단어들의 통합

USA, US 통합 등

* 표제어 추출(lemmatization) : 기본 사전형 단어. ex. are, is의 표제어는 be

  표제어 추출을 하는 방법 : 어간과 접사를 분리를 먼저 진행해야 함

  * 형태소
    * 어간(stem) : 단어의 의미를 담고 있는 단어의 핵심
    * 접사(affix) : 단어에 추가적인 의미를 주는 부분

  ```shell
  from nltk.stem import WordNetLemmatizer
  
  words = [토큰화된 단어들]
  WordNetLemmatizer().lemmatize(w) for w in words
  
  #stemming과 달리 단어의 형태가 원래의 형태로 보존됨, 본래 단어의 품사 정보를 알아야만 정확한 결과를 얻을 수 있음
  
  WordnetLemmatizer().lemmatize('dies', 'v') #이런식으로 품사 알려주기 가능
  ```

* 어간 추출(stemming) :  품사정보가 보존되지 않음

  ```shell
  # 포터 알고리즘 사용
  from nltk.stem import PorterStemmer
  from nltk.tokenize import word_tokenize
  
  words=word_tokenize(text)
  
  PorterStemmer().stem(w) for w in words
  ```

  포터 알고리즘 :  ALIZE → AL
  							ANCE → 제거
  							ICAL → IC

  ```
  # 스태머 알고리즘 사용
  from nltk.stem import LancasterStemmer
  
  words=word_tokenize(text)
  
  LancasterStemmer().stem(w) for w in words
  ```

* 한국어에서의 어간 추출
  * 용언에 해당되는 동사와 형용사는 어간(stem)과 어미(ending)의 결합으로 구성(활용(conjugation)이라고 부름)
  * 활용
    * 규칙활용 : 어간이 어미를 취할 때, 어간의 모습이 일정
    * 불규칙 활용 : 어간이 어미를 취할 때, 어간의 모습이 바뀌거나 취하는 어미가 특수한 어미일 경우



### 2. 대, 소문자 통합



### 3. 불필요한 단어의 제거

#### (1) 등장 빈도가 적은 단어

#### (2) 길이가 짧은 단어

* 영어에서는 효과가 좋음
* 한 글자에 내포된 의미가 많은 한국어에서는 효과가 별로임

```
import re

shortword = re.compile(r'\W*\b\w{1,2}\b') #정규표현식을 통하여 길이가 1~2 인 단어들 제거
```

#### (3) 불용어(stopword) 제거

자주 등장하지만 분석에 의미가 없는 단어, 직접 정의도 가능

* 영어. NTLK에서 불용어 확인

  ```shell
  from nltk.corpus import stopwords
  stopwords.words('english')[:10] #불용어 10개 추출
  ```

* 불용어 제거

  ```shell
  from nltk.corpus import stopwords 
  from nltk.tokenize import word_tokenize 
  
  example = "Family is not an important thing. It's everything."
  stop_words = set(stopwords.words('english')) 
  
  word_tokens = word_tokenize(example)
  
  result = []
  for w in word_tokens: 
      if w not in stop_words: 
          result.append(w) 
  
  print(word_tokens) 
  print(result) 
  ```

* 한국어 불용어제거. 사용자가 직접 불용어 사전을 만들게 되는 경우가 많음

  ```shell
  from nltk.corpus import stopwords 
  from nltk.tokenize import word_tokenize 
  
  example = "고기를 아무렇게나 구우려고 하면 안 돼. 고기라고 다 같은 게 아니거든. 예컨대 삼겹살을 구울 때는 중요한 게 있지."
  stop_words = "아무거나 아무렇게나 어찌하든지 같다 비슷하다 예컨대 이럴정도로 하면 아니거든"
  # 위의 불용어는 명사가 아닌 단어 중에서 저자가 임의로 선정한 것으로 실제 의미있는 선정 기준이 아님
  stop_words=stop_words.split(' ')
  word_tokens = word_tokenize(example)
  
  result = [] 
  for w in word_tokens: 
      if w not in stop_words: 
          result.append(w) 
  # 위의 4줄은 아래의 한 줄로 대체 가능
  # result=[word for word in word_tokens if not word in stop_words]
  
  print(word_tokens) 
  print(result)
  ```

  

### 4. 정규 표현식

정규 표현식 모듈 re의 사용



* NLTK에서는 정규 표현식을 사용해서 단어 토큰화를 수행하는 RegexpTokenizer를 지원

  ```shell
  import nltk
  from nltk.tokenize import RegexpTokenizer
  
  tokenizer=RegexpTokenizer("[\w]+")
  
  print(tokenizer.tokenize("Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop"))
  
  #구두점을 제외하고 단어들만을 가지고 토큰화
  ```

  ```shell
  import nltk
  from nltk.tokenize import RegexpTokenizer
  
  tokenizer=RegexpTokenizer("[\s]+", gaps=True) #gaps = True는 해당 정규표현식을 토큰으로 나누기 위한 기준으로 사용한다는 의미
  
  print(tokenizer.tokenize("Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop"))
  
  #공백으로 토큰화
  ```

  

