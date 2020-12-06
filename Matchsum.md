## Matchsum



#### 핵심 아이디어

좋은 요약은 좋지 않은 요약에 비해서 의미적으로 원본 문서 전체에 더 유사



sentence score가 높은 요약문이라고 반드시 best summary는 아님

sentence-level에서 더 나아가 summary-level 추출식 요약이 필요



#### 두 문서의 유사도

 f(D, C) = cosine(R_D, R_C)

* R_D는 문서의 표현, R_C는 후보 요약문의 표현
* 