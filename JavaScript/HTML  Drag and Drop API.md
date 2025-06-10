<details>
  <summary>Drag And Drop</summary>

## Drag And Drop API

- 브라우저에서 어플리케이션이 드래그 앤 드롭 기능을 사용하게 해준다
- 브라우저에서 요소를 드래그를 할때, dragstart → drag(반복) → dragenter(잠재적 드롭 대상 요소) → dragover(반복) → dragleave / drop → dragend 순으로 흐른다.

## 브라우저 Drag & Drop 이벤트 정리

### 1. `dragstart`

- **이벤트 이름:**  
  사용자가 요소를 **눌러 끌기 시작**할 때 *한 번* 발생

- **발생 대상:**  
  드래그 **원본(소스) 요소**

- **언제 실행되나?:**  
  사용자가 요소를 드래그하기 위해 마우스(또는 터치)를 눌러 움직이기 *시작*할 때

- **주요 목적 / 개발자가 자주 하는 일:**  
  - `e.dataTransfer.setData()`로 드롭 대상에게 전달할 데이터 등록  
  - `e.dataTransfer.effectAllowed`로 허용 동작(예: `move`, `copy`) 지정  
  - 필요하다면 커스텀 Ghost(프리뷰) 이미지 설정


### 2. `drag`

- **이벤트 이름:**  
  드래그 중 **연속적으로** 발생

- **발생 대상:**  
  드래그 **원본 요소**

- **언제 실행되나?:**  
  마우스(또는 손가락)가 움직일 때마다, 드래그 과정이 **지속되는 동안 내내**

- **주요 목적 / 개발자가 자주 하는 일:**  
  - 실시간 위치 데이터 로깅 또는 시각적 효과 적용  
  - 드래그 경로에 따른 커스텀 안내선·도움말 표시


### 3. `dragenter`

- **이벤트 이름:**  
  드래그 객체가 **잠재적 드롭 대상** 영역 안으로 *처음* 진입할 때 발생

- **발생 대상:**  
  잠재적 **드롭 대상 요소**

- **언제 실행되나?:**  
  커서가 대상 요소의 경계를 **처음 통과**하는 순간

- **주요 목적 / 개발자가 자주 하는 일:**  
  - 드롭 가능함을 강조(하이라이트, 테두리 등)  
  - 내부 상태 초기화(예: 드롭 위치 계산 준비)


### 4. `dragover`

- **이벤트 이름:**  
  드래그 객체가 대상 위를 **지나는 동안 반복** 발생

- **발생 대상:**  
  잠재적 **드롭 대상 요소**

- **언제 실행되나?:**  
  커서가 대상 요소 위에 머무르는 동안 계속

- **주요 목적 / 개발자가 자주 하는 일:**  
  - `e.preventDefault()` 호출로 **드롭을 허용** (필수)  
  - `e.dataTransfer.dropEffect` 설정으로 사용자 피드백 제어(아이콘 변화 등)  
  - 실시간 드롭 위치(예: 삽입 가이드라인) 계산


### 5. `dragleave`

- **이벤트 이름:**  
  드래그 객체가 대상 요소 **경계 밖으로 벗어날 때** 발생

- **발생 대상:**  
  잠재적 **드롭 대상 요소**

- **언제 실행되나?:**  
  커서가 대상 영역 밖으로 이동하는 순간

- **주요 목적 / 개발자가 자주 하는 일:**  
  - `dragenter`에서 적용한 강조(하이라이트) 제거  
  - 내부 임시 상태 / 가이드라인 정리


### 6. `drop`

- **이벤트 이름:**  
  사용자가 **마우스 버튼을 놓는 순간** 발생

- **발생 대상:**  
  실제 **드롭이 이루어지는 대상 요소**

- **언제 실행되나?:**  
  드래그가 끝나고 객체가 대상 위에 놓일 때

- **주요 목적 / 개발자가 자주 하는 일:**  
  - `e.preventDefault()`로 기본 동작 차단(필수)  
  - `e.dataTransfer.getData()`를 통해 전송된 데이터 수신  
  - DOM 변경, 파일 업로드, 정렬 갱신 등 **드롭 후 로직** 실행


### 7. `dragend`

- **이벤트 이름:**  
  드래그 **작업이 완료**될 때(성공 ↔ 취소 관계없이) 발생

- **발생 대상:**  
  드래그 **원본 요소**

- **언제 실행되나?:**  
  `drop` 이벤트 직후 또는 사용자가 드래그를 취소했을 때

- **주요 목적 / 개발자가 자주 하는 일:**  
  - 원본 요소 스타일·클래스 복원  
  - 로딩 스피너·상태 플래그 등 **최종 정리 작업**


```javascript
<style>
  #box  { width:120px; height:120px; background:#4fa; cursor:grab; }
  #drop { width:200px; height:140px; border:2px dashed #888; margin-top:12px; }
  .hover { background:#ffe; }
</style>

<div id="box" draggable="true">Drag me</div>
<div id="drop">Drop here</div>

<script>
const box  = document.getElementById('box');
const drop = document.getElementById('drop');
const log  = (...msg) => console.log(...msg);

/* 1) dragstart — 끌기 시작 */
box.addEventListener('dragstart', e => {
  log('dragstart');
  e.dataTransfer.setData('text/plain', 'BOX_1');
  e.dataTransfer.effectAllowed = 'move';
});

/* 2) drag — 끌리는 동안 반복 */
box.addEventListener('drag', () => log('drag (source)'));

/* 3) dragenter — 드롭 대상 진입 */
drop.addEventListener('dragenter', () => {
  log('dragenter');
  drop.classList.add('hover');
});

/* 4) dragover — 대상 위에서 반복  */
drop.addEventListener('dragover', e => {
  e.preventDefault();          // **드롭 허용 필수**
  drop.classList.add('hover');
  log('dragover');
});

/* 5) dragleave — 대상 이탈 */
drop.addEventListener('dragleave', () => {
  log('dragleave');
  drop.classList.remove('hover');
});

/* 6) drop — 드롭 완료 */
drop.addEventListener('drop', e => {
  e.preventDefault();
  log('drop');
  drop.textContent = 'Dropped: ' + e.dataTransfer.getData('text/plain');
  drop.classList.remove('hover');
});

/* 7) dragend — 작업 종료(성공·취소 공통) */
box.addEventListener('dragend', () => log('dragend'));
</script>

```
## 주의 포인트 !

1. **`dragover`에서 `e.preventDefault()` 잊지 않기**  
   - 이 한 줄이 없으면 _드롭 금지_ 상태가 되어 `drop` 이벤트가 **절대 발생하지 않는다**.

2. **`dragenter` ↔ `dragleave` 깜빡임(플리커) 현상**  
   - 자식 노드를 밟을 때마다 이벤트가 다시 트리거됨.  
   - `enterCount++ / --` 카운터 패턴이나 `relatedTarget` 검사를 사용해 **경계 통과**만 감지.

3. **모바일 브라우저 기본 지원 미비**  
   - 터치 환경에서는 네이티브 Drag & Drop이 거의 동작하지 않는다.  
   - React DND, interact.js 등 **포인터 이벤트 기반** 라이브러리로 폴백을 제공.

4. **파일 드롭 vs 텍스트/URL 드롭 구분**  
   - 파일은 `e.dataTransfer.files`(FileList)로 읽고,  
   - 텍스트·URL 등은 `e.dataTransfer.getData()`로 받아야 한다.

5. **크로스-브라우저 보안 제한**  
   - `dataTransfer`를 통해 **텍스트만** 전송 가능(바이너리는 차단).  
   - 서로 다른 도메인의 iframe 간에는 데이터 전달이 막힐 수 있다.

6. **접근성(Accessibility) 대응**  
   - 마우스 의존 UI는 키보드·스크린리더 이용자가 사용할 수 없다.  
   - `role="listbox"`, `aria-dropeffect`, `aria-grabbed` 등을 추가하고  
     Space/Enter 키 기반 **키보드 대안**을 구현할 것.

7. **성능: `dragover` 연속 호출 부담**  
   - 60 fps 기준 16 ms마다 실행 → 무거운 계산·DOM 갱신은 **`requestAnimationFrame` 또는 throttle** 적용.

8. **`dragend` 누락 케이스**  
   - ESC 취소, 탭 전환, 네비게이션 중단 등으로 `dragend`가 호출되지 않을 수 있다.  
   - 정리(clean-up) 로직을 `drop`에도 한 번 더 넣어 두기.

9. **React(합성 이벤트) 사용 시 주의**  
   - 컴포넌트마다 `onDragOver={e => e.preventDefault()}`를 **반드시** 호출해야 드롭 허용.  
   - 일부 라이브러리에서 `currentTarget`이 예상과 다를 수 있으므로 콘솔로 구조 확인.

10. **커스텀 Drag 이미지 품질 문제**  
    - `setDragImage(node, x, y)` 사용 시 Hi-DPI(레티나) 환경에서 흐릿하게 보일 수 있다.  
    - 2× 크기의 캔버스나 **오프스크린 캔버스**에 렌더링한 뒤 지정하면 선명도 유지.




```javascript

```




```javascript

```




```javascript

```


</details>