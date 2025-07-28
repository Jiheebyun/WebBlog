<details>
  <summary>Elastic Search 동작원리</summary>

#### Elastic Search 동작원리

- 대용량 문서를 인덱스를 만어서 인덱스 기반으로 검색하게 된다.

- 사용 예:
Doc1: 안녕 물고기
Doc2: 안녕 사자
Doc3: 안녕 고양이 사자

안녕: Doc1, Doc2, Doc3
물고기: Doc1,
사자: Doc2, Doc3
고양이: Doc3


#### 인데스 만드는 과정

- 가장 중요한 포인트로 문서를 전처리, 형태소 분석, 후처리 과정을 통해 의미있는 형태소만 인덱스로 만들어 검색이 잘될수 있게 한다

- 문서 -> | 전처리 -> 형태소 분석 -> 후처리 | -> 인텍스 

##### 문서를 엘라스틱서치에 넣었을때
- char_filter - 0개 이상 적용 가능 
- tokenizer - 1개만 적용가능
- token_filter - 0개 이상 적용가능 
- 색인은 3단계를 거친다.

###### char_filter
- HTML strip - HTML 제거 
- Mapping character - 특정문자대치
- Pattern replace - 특정 패턴 대치 (정규식)
###### tokenizer
- ex - this is a test -> this, is, a, test
- 문자열을 토큰화 시키는 단계
- standard -> 한국어 부적합
- nori_tokenizer -> 한국어를 위한 tokenizer
###### token_filter
- lowercase, stop, synonym, stemmer 등이 있다
- 토큰화가 된 토큰에 대해서 후처리 단계


</details>