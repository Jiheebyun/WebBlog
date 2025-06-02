<details>
  <summary>Drag And Drop</summary>

## Drag And Drop API

- 브라우저에서 어플리케이션이 드래그 앤 드롭 기능을 사용하게 해준다
- 브라우저에서 요소를 드래그를 할때, dragstart → drag(반복) → dragenter(잠재적 드롭 대상 요소) → dragover(반복) → dragleave / drop → dragend 순으로 흐른다.

순번	이벤트 이름	발생 대상	언제 실행되나?	주요 목적 / 개발자가 자주 하는 일
1	dragstart	
  - 이벤트 이름
   - 	사용자가 요소를 눌러 끌기 시작할 때 딱 한 번	
  - 발생 대상
   - 드래그 원본(소스) 요소
  - 언제 실행되나?
   - 사용자가 요소를 눌러 끌기 시작할 때 딱 한 번
  - 주요 목적 / 개발자가 자주 하는 일
   - e.dataTransfer.setData() 로 전송할 데이터 등록
   - e.dataTransfer.effectAllowed 로 허용 동작 지정
   - 필요하면 커스텀 Ghost 이미지를 설정


```javascript

```


```javascript

```




```javascript

```



</details>