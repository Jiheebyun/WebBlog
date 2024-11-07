<details>
  <summary>Gulp란?</summary>

  ##### Gulp란?

  Gulp는 자바스크립트 기반의 자동화 도구이다. 반복적인 작업을 자동으로 처리하여 개발자가 더 빠르고 효율적으로 작업할 수 있도록 도와준다. Gulp는 주로 빌드 작업을 자동화하는 데 사용된다. 예를 들어, CSS 파일을 컴파일하거나, 자바스크립트 파일을 압축하거나, 이미지를 최적화하는 등의 작업을 자동으로 처리한다.

##### Gulp의 특징
1. 스트림 기반 처리이다.
Gulp는 파일을 스트림으로 처리한다. 이는 메모리 사용을 효율적으로 하고 작업 속도를 빠르게 만든다.
2. 플러그인 중심이다.
Gulp는 다양한 플러그인을 통해 여러 작업을 처리한다. 예를 들어, gulp-sass 플러그인을 사용하여 Sass를 CSS로 컴파일하고, gulp-uglify 플러그인을 사용하여 자바스크립트를 압축한다.
3. 비동기 작업 처리이다.
Gulp는 비동기 방식으로 작업을 처리한다. 이를 통해 여러 작업을 동시에 병렬로 실행할 수 있다. 이로 인해 빌드 시간이 단축된다.
4. 간결하고 직관적인 코드이다.
Gulp는 설정이 간단하고, 코드가 직관적이다. 작업 흐름을 쉽게 이해하고 작성할 수 있다.
##### Gulp로 할 수 있는 일
- CSS 전처리기 처리: Sass나 LESS 파일을 자동으로 컴파일한다.
- 자바스크립트 압축: 여러 자바스크립트 파일을 하나로 합치고, 압축하여 파일 크기를 줄인다.
- 이미지 최적화: 이미지를 압축하여 웹사이트의 로딩 속도를 빠르게 만든다.
- 파일 복사: HTML, CSS, 이미지 파일 등을 다른 폴더로 자동으로 복사한다.


#####  Gulp 기본 설정
```javasript
const gulp = require('gulp');  // Gulp 모듈 불러오기

// 예시 작업: 'hello'라는 작업 정의
gulp.task('hello', (done) => {
  console.log('Hello, Gulp!');
  done(); // 작업이 끝났음을 Gulp에 알려줍니다.
});

```

##### Gulp 작업 설정
```javasript
const gulp = require('gulp');  // Gulp 모듈 불러오기

// 예시 작업: 'hello'라는 작업 정의
gulp.task('hello', (done) => {
  console.log('Hello, Gulp!');
  done(); // 작업이 끝났음을 Gulp에 알려줍니다.
});

```
##### Gulp 작업 정의 
```javasript
// npm install --save-dev gulp-sass
const gulp = require('gulp');
const sass = require('gulp-sass')(require('sass'));

gulp.task('sass', () => {
  return gulp.src('src/sass/**/*.scss')  // Sass 파일 경로
    .pipe(sass().on('error', sass.logError))  // Sass 컴파일
    .pipe(gulp.dest('dist/css'));  // 결과 파일 저장 경로
});

```

</details>