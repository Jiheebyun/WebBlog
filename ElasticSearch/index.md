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


#### nori tokenizer 

```json
{

    "settings": {
        "analysis": {
            "analyzer": {
                "my_custom_analyzer": {
                    "type": "custom",
                    "char_filter": [],
                    "tokenizer": "my_nori_tokenizer",
                    "filter": ["lowercase_filter"]
                }
            },
            "char_filter": {},
            "tokenizer": {
                "my_nori_tokenizer": {
                    "type": "nori_tokenizer",
          		    "decompound_mode": "mixed",
          		    "discard_punctuation": "true",
          		    "lenient": true
                }
            },
            "filter": {
                "lowercase_filter": {
                    "type": "lowercase"
                }
            }
        }
    }
}
```

- decompound_mode : mixed 설정은 합성어 처리 방법으로 mixed로 설정하면 합성어를 분해하고 원본 단어도 유지합니다. (가곡역 → 가곡, 역, 가곡역)
- discard_punctuation : true 설정은 구두점 제거 여부입니다. (반가워! → 반가워)
- lenient : true 설정은 형태소 분석 과정에서 오류 발생시 skip 여부

</details>