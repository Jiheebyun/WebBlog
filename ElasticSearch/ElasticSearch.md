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

##### nori tokenizer 사용자 사전
- nori 토크나이저를 사용하면 특정한 단어가 원하지 않게 분해되는 경우가 있습니다. 이를 대비하여 사용자 사전을 등록해 특정 단어는 토크나이징이 되지 않도록 설정할 수 있다.
- 사용자 사전은 2가지 방식으로 설정할 수 있습니다.
1. 직접 JSON에 명시
2. txt 파일 경로 명시 (대게 이 방법 사용)
```json
"tokenizer": {
    "my_nori_tokenizer": {
        "type": "nori_tokenizer",
        "decompound_mode": "mixed",
        "discard_punctuation": "true",
        "user_dictionary": "userdict_ko.txt",
        "lenient": true
    }
}
```

#### 동의어 사전
-  “char_filter → tokenizer → token_filter” 과정에서 token_filter는 분해한 토큰을 후처리

- 동의어 필터는 특정 단어를 의미가 비슷한 단어로 치환, 확장  (ex) 책 = 서적 = book
```json
"settings": {
    "analysis": {
        "analyzer": {
            "my_custom_analyzer": {
                "type": "custom",
                "char_filter": [],
                "tokenizer": "my_nori_tokenizer",
                "filter": [
                    "lowercase_filter",
                    "synonym_filter" // 필터 추가
                ]
            }
        },
        "char_filter": {},
        "tokenizer": {
            "my_nori_tokenizer": {
                "type": "nori_tokenizer",
                "decompound_mode": "mixed",
                "discard_punctuation": "true",
                "user_dictionary": "dict/userdict_ko.txt",
                "lenient": true
            }
        },
        "filter": {
            "lowercase_filter": {
                "type": "lowercase"
            },
            "synonym_filter": { // 동의어 필터
                "type": "synonym",
                "synonyms_path": "dict/synonym-set.txt",
                "lenient": true
            }
        }
    }
}

```
##### 동의어 사전 작성 방법
```bash
# "synonyms_path": "dict/synonym-set.txt",
# 확장 - 특정 토큰에 대해 의미가 비슷한 토큰을 추가로 넣어주는 것
ipod, i-pod, i pod
computer, pc, laptop
# 치환 - 특정 토큰에 대해 의미가 비슷한 토큰을 추가로 넣어주는 것
personal computer => pc
sea biscuit, sea biscit => seabiscuit

```


#### 한글 복합어 동의어 에러처리 

##### 한국어 복합어 문제
- nori tokenizer 단계에서 복합어를 mixed(discard)처리를 진행했는데, 이는 복합어를 분리합니다.
- 문제는 tokenizer에서 "왓"을 "오", "앗"으로 쪼개 버리기때문에 동의어 사전에서 "왓"이라는 단어를 찾을수 없을 -> 매핑 불일치 에러 발생
- 즉, 동의어 사전에 "왓"을 올려도 실제 분석당께에서는 "왓"이라는 토큰이 존재하지 않으니 필터 적용이 불가능해져서 에러가 발생
```bash
# userdict_ko.txt (nori tokenizer)
신사역 -> 신사 역
입생로랑 -> 입 생 로랑
왓 -> 오 앗

# synonym-set.txt
왓, what

```

##### 해결방향

1. 복합어 처리 방식을 바꾸기
`decompound_mode`를 `mixed` 대신 `none`으로 설정하여  
원형을 유지하도록 한다.

```json
"tokenizer": {
  "type": "nori_tokenizer",
  "decompound_mode": "none"
}
```

2. keyword_marker 필터사용
- 지정된 단어를 분리하지 않고 원형 그래도 유지.
```json
"analysis": {
  "analyzer": {
    "custom_korean": {
      "tokenizer": "nori_tokenizer",
      "filter": ["protect_keyword", "my_synonyms"]
    }
  },
  "filter": {
    "protect_keyword": {
      "type": "keyword_marker",
      "keywords": ["왓"]
    },
    "my_synonyms": {
      "type": "synonym",
      "synonyms": ["왓, what"]
    }
  }
}
```
3. Multi-field 매핑
-  두 개의 방식으로 동시에 색인
- ex)
1. title.nori: ["입", "생", "로랑", "오", "앗", "신사", "역"]
2. title.raw: ["입생로랑 왓 신사역"]

```json
"mappings": { // 인덱스가 어떤 구조로 문서를 저장할지를 정의하는 부분
  "properties": { //각 문서(Document)가 가질 필드 목록.
    "title": {
      "type": "text",
      "fields": {
        "nori": {
          "type": "text",
          "analyzer": "nori_mixed"
        },
        "raw": {
          "type": "text",
          "analyzer": "keyword"
        }
      }
    }
  }
}
```


#### CRUD
- 엘라스틱서치는 모든 작업을 REST하게 API로 제공하고 있기 때문에 API 요청으로 인텍스에 문서를 CURD 할수 있다 

###### POST : https://아이피:9200/인덱스명/_doc

```json
{
    "id": "1",
    "title": "문서1",
    "content": "문서1 내용",
    "created": "2025-03-07T00:00:00Z"
}

```

##### GET : https://아이피:9200/인덱스명/_doc/번호
##### POST : https://아이피:9200/인덱스명/_doc/번호
##### DELETE : https://아이피:9200/인덱스명/_doc/번호




#### Bulk API

-  게시판 DB에 담겨 있는 문서를 주기적으로 엘라스틱서치 인덱스에 밀어 넣는 상황이 자주 발생한다. (전체 색인)
-  이 경우 개별 API를 수천 번씩 실행할 경우 색인이 제대로 생기지 않는 문제가 발생한다.
-  이 문제를 해결하기 위해서 엘라스틱서치에서는 수천만건의 데이터를 조금씩 묶어서 처리할수 있는 Bulk API를 제공하고 있다.

##### POST : https://아이피:포트/_bulk

##### Body 작성 방법
- 추가 : index (_id 값이 : 없다면 추가, 있다면 수정)

```json
{ "index" : { "_index" : "my_index" } }
{ "id": "1", "title" : "제목1", "content": "내용1", "created": "2025-03-09T00:00:00Z" }
```
- 삭제 
```json
{ "delete" : { "_index" : "my_index", "_id" : "" } }
```
- 수정
```json
{ "update" : { "_id" : "1", "_index" : "my_index" } }
{ "doc" : { "id": "1", "title" : "제목1", "content": "내용1", "created": "2025-03-09T00:00:00Z" } }
```

##### NDJSON
- body는 단순 JSON이 아니라 Newline Delimits JSON 이다.
- - 일반 JSON은 배열([])이나 객체({}) 형태로 묶어서 표현하지만,
NDJSON은 각 JSON 객체를 줄바꿈(개행)으로 구분하는 형식
```json
Content-Type: application/x-ndjson
```
-  _bulk API는 Read를 지원하지는 않는다. (_mget 사용)
- bulk 최대 값은 HTTP 요청 한계 값인 100MB입니다. 다만 100MB를 다 사용하지 않고 1000 ~ 2000개씩 끊어서 요청


#### 전체 색인
- **색인(Indexing)**: 문서를 검색 가능한 형태로 변환해 Elasticsearch에 저장하는 과정
- 단순 저장이 아니라, **역색인(Inverted Index)** 구조로 만들어야 검색이 가능
- **부분 색인(Partial Update)**이 발생하면 Elasticsearch는 내부적으로 **문서 전체를 다시 색인**한다.
- 전체 색인이 필요 -> 사용자/동의어 사전 변경, 색인 누락
##### 동작 원리
1. 부분 색인 요청 발생 (`_update` API 사용)
2. Elasticsearch가 기존 문서를 조회
3. 변경된 필드를 반영하여 **새 문서 전체를 생성**
4. 새 문서를 색인
5. 기존 문서는 **삭제 마킹(mark delete)** 처리  
   → 실제 디스크 삭제는 나중에 segment merge 시점에 수행

##### 왜 그런가?
- Elasticsearch 문서는 **불변(Immutable)** 구조
- 역색인(Inverted Index) 때문에 문서 일부만 수정 불가능
- 따라서 항상 **문서 단위로 교체**해야 함

##### 비유
> 문서의 한 필드만 고치고 싶어도,  
> Elasticsearch는 기존 문서를 지우고  
> 새 문서 전체를 다시 작성한다.

##### 만약 전체 색일을 하는동안 사용자에 의해서 DB에 CRUD가 발생하면?
- 1. 전체 색인을 시작하는 시점"에 DB가 가진 데이터만 전체 색인을 하도록 설정 (전체 색인 시점 DB 마지막 id 값 이후로 새로 들어온 데이터는 전체 색인 배치에서 제외 함.)
- 2. 전체 색인 과정에서 발생하는 Create, delete, update는? 전체 색인 과정을 수행하는 도중 신규로 DB에 추가되는 데이터는 WAS 단에서 DB와 ES에 모두에 넣는 방법이 있다. (전체 색인 시작시, 기존 데이터의 마지막 id 값 까지만 진행하도록 설계하여 중복을 방지)


##### 매 전체 색인시 색인명이 변경되면, WAS에서 색인명을 변경해야 할까요?
- 다행이 alias라는게 존재
- alias는 별칭으로 대표가 되는 이름을 정해두고, 내부적으로 redirect를 진행할 수 있다.
- API: POST : https://아이피:9200/_aliases
```json
// alias 설정
{
  "actions": [
    {
      "add": {
        "index": "my-index-날짜",
        "alias": "my-index"
      }
    }
  ]
}
```

```json
// alias 제거
{
  "actions": [
    {
      "remove": {
        "index": "my-index-날짜",
        "alias": "my-index"
      }
    }
  ]
}
```

##### 검색과 CUD alias 분리
- 전체 색인을 진행하는 동안 CUD는 새로운 색인, 검색은 신규 색인이 다 만들어지지 않았기 떄문에 기존 색인에 대해 진행
-  이 문제는 alias를 통해 해결 -> 검색용 alias, CUD용 alias
##### Elasticsearch Alias를 활용한 무중단 색인 교체 과정 
-Elasticsearch에서 인덱스 매핑 변경이나 구조 수정을 위해 새로운 인덱스를 생성해야 할 때, 검색(Search)과 CUD(Create, Update, Delete) 작업을 중단하지 않고 전환하는 방법은 alias를 활용하는 것이다. 이 과정을 단계별로 살펴보면 다음과 같다.

1. **검색용 alias와 CUD용 alias가 모두 기존 색인을 가리킴**  
- 현재 상태에서 검색(alias)인 `search_alias`와 쓰기(alias)인 `write_alias`는 모두 기존 인덱스 `old_index`를 참조한다.  

2. **신규 색인 생성**  
- 새로운 매핑과 설정을 적용한 `new_index`를 생성한다. 아직 alias와 연결되지 않았으므로 서비스에는 영향이 없다.  

3. **CUD용 alias를 신규 색인으로 전환**  
- `write_alias`를 `new_index`로 변경하여 이후 생성·수정·삭제 작업이 모두 `new_index`에 반영되도록 한다.  

4. **신규 색인 완료**  
- 기존 `old_index`의 데이터를 `new_index`로 백필(backfill)하여 최신 상태의 데이터를 준비한다. 이 시점에서 `new_index`에는 최신 데이터가 모두 들어있다.  

5. **검색용 alias를 신규 색인으로 전환**  
- `search_alias`도 `new_index`를 가리키도록 변경한다. 이제 검색과 쓰기 모두 `new_index`를 사용하게 되며 무중단 전환이 완료된다.  



#### 검색 API
- 검색을 위해 인덱스를 만들고 문서를 넣는다 그 결과에 검색을 할수 있다
- 검색 또한 API로 할수 있고, 다양한 기능을 제공하고 있다.

- POST : https://IP:9200/인덱스/_search -> 인덱스만 검색
- POST : https://IP:9200/_search -> 전체 인텍스를 검색

##### 검색종류
- match 쿼리 -> 형태소 분석된 필드에 대해 검색어도 형태소 분석을 진행하여 검색 (문장들을 tokenizer)
- term 쿼리 -> 형태소 분석된 인덱스 필드에 대해 검색어는 입력 그대로 사용하여 검색 
- ex) 문서 인텍싱 : : (제목 왓더헬 → tokenizer ->  제목, 왓, 더, 헬 (토큰생성))
- 사용자가 "제목"이라는 단어를 검색했을경우에, term 쿼리를 사용해서 "제목 왓더헬"을 찾는다면 "제목"이라는 단어가 "제목 왓더헬"에 매칭 되지 않지만
- match 쿼리를 사용해서 "제목 왓더헬"을 찾는다면 제목, 왓, 더, 헬로 tokenizer됬지 때문에 "제목"을 입력해도 매칭이 된다.

- bool 쿼리 -> match와 term 쿼리를 할께 사용할수 있다. 여러 쿼리를 조합하고, 논리 연산 추가를 사용하여 검색 
  - must : 속한 match, term이 AND와 같이 모두 있어야 함
  - should : 속한 match, term이 OR과 같이 하나 이상 있어야 함
  - filter : 속한 match, term이 AND로 모두 있어야 하지만, score는 계산하지 않음
  - must_not :  match, term이 하나라도 속해 있다면 검색되지 않음

```json
// match 쿼리 ->  title에 "제목"이 포함되어 있는 데이터를 검색
{
    "query": {
      "match": {
        "title": "제목"
      }
    }
}
```
```json
// term 쿼리 -> title이 정확히 "제목 왓"으로 되어있는 데이터를 검색
{
    "query": {
      "term": {
        "title": "제목 왓"
      }
    }
}
```
```json
// bool 쿼리  
{
    "query": {
        "bool": {
            "must": [
                {
                    "match": {
                        "title": "제목 왓"
                    }
                },
                {
                    "match": {
                        "title": "제목 왓"
                    }
                }
            ],
            "filter": [],
            "should": [],
            "must_not": []
        }
    }
}
```

#### 검색 API 옵션

##### 페이지 네이션
```json
// /my_index/_search
{
  "query": {
    "term":{
      "title":"제목"
    }
  },
  "from" : 0,
  "size" : 2,
  "min_score": 0.2 // elasticsearch는 검색에 대해서 스코어 값을 가지게 되는데 낮게 설정하게 되면 검색결과가 없을 수 있다
  // BM25 min score 
  // 검색을 하려는 단어가 제목에 많이 들어 갈수록 더 높은 가충치를 두도록 설정한 
  // 제목에 1개 내용에 10개와 제목에 2개 내용에 1개가 있을때, 
  // 기본적으로 BM25기반에 가중치가 없다면 제목 + 내용의 합이 많은 문서가 선정되지만 제목에 가중치를 준다면 내용이 
}
// "제목" 포함 되어진 데이터를 모두 가져오지만 그중에 2개만 출력 
```
##### BM25 최소 점수 & 제목 가중치 설명

- 1. BM25 기본 동작
- 검색어가 **제목**과 **내용**에 등장한 횟수를 합산하여 점수를 계산
- 기본 상태에서는 제목과 내용의 중요도(가중치)가 동일
- 예시:
  - **문서 A**: 제목(1개) + 내용(10개) → 총 11회
  - **문서 B**: 제목(2개) + 내용(1개) → 총 3회
  - **결과**: 기본 BM25에서는 문서 A가 더 높은 점수를 받음  
    *(전체 빈도 합계가 많기 때문)*

- 2. 제목 가중치 적용 시
- 제목에 포함된 검색어에 더 높은 비중을 부여
- 예시:
  - 가중치 설정: 제목 1개 = 내용 3개 가치
  - **문서 A 점수**: 제목(1 × 3) + 내용(10 × 1) = 3 + 10 = 13
  - **문서 B 점수**: 제목(2 × 3) + 내용(1 × 1) = 6 + 1 = 7  
    → 여전히 문서 A 점수가 높음
  - **만약 제목 가중치를 더 높이면** (예: 제목 1개 = 내용 10개 가치)
    - 문서 A: 1 × 10 + 10 × 1 = 20
    - 문서 B: 2 × 10 + 1 × 1 = 21  
      → 문서 B가 역전 가능

- 3. 활용 포인트
- 제목에 검색어가 포함되면 사용자의 검색 의도에 더 부합한다고 가정
- 제목 가중치는 `BM25` 계산식에서 **필드별 boost** 값을 조정하여 적용
- Elasticsearch 예시:
  ```json
  {
    "query": {
      "multi_match": {
        "query": "검색어",
        "fields": ["title^3", "content"] //title^3은 title 필드의 검색 점수를 3배로 곱해서 계산
        //["title", "content"] -  title과 content의 점수 비중이 같음
      }
    }
  }


</details>