# Padding

컴퓨터는 길이가 전부 동일한 문서들에 대해서는 하나의 행렬로 보고 한꺼번에 자연어처리 가능. 여러 문장의 길이를 임의로 동일하게 맞춰주는 작업을 padding이라함



### 1. Numpy로 패딩하기

```shell
import numpy as np
from tensorflow.keras.preprocessing.text import Tokenizer

sentences = [['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]

tokenizer = Tokenizer()
tokenizer.fit_on_texts(sentences) # fit_on_texts()안에 코퍼스를 입력으로 하면 빈도수를 기준으로 단어 집합을 생성한다.

encoded = tokenizer.texts_to_sequences(sentences)
print(encoded)

max_len = max(len(item) for item in encoded)
print(max_len) #최대 길이 출력

for item in encoded: # 각 문장에 대해서
    while len(item) < max_len:   # max_len보다 작으면
        item.append(0) # 0을 추가

padded_np = np.array(encoded)
padded_np
```



### Keras로 padding

```shell
from tensorflow.keras.preprocessing.sequence import pad_sequences

encoded = tokenizer.texts_to_sequences(sentences)
print(encoded)

padded = pad_sequences(encoded)
#뒤쪽에 0을 채우고 싶으면 padding='post'를 인자로 작성
#패딩의 길이를 제한하고 싶다면 maxlen ='number' 인자 추가
#0말고 다른 숫자로 패딩하고 싶으면 value = number 인자 추가
padded
```

