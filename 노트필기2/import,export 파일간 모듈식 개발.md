import / export 를 이용한 파일간 모듈식 개발
==

### 1. 기본적인 외부 js 첨부 방식
```
📁 작업폴더명
├── 📁js
│   ├── library.js // .js파일을 만들어서 자바스크립트를 보관함
```
```html {.line-numbers}
<!-- 외부 js파일 불러오고 싶으면 -->
<script src="js/library.js"></script>
```
### 2. export default
- import 방법
  ```html {.line-numbers}
  <script type="module">
    // import 가져올거 from '경로'
    import a from 'js/library.js'
  </script>
  ```
  - `a`라는 특정 변수만 가져올 수 있음
##### export default 문법
```js {.line-numbers}
// 📄 library.js
var a = 10;

// export default 내보낼거
export default a;
```
- `a`라는 특정 변수를 내보내겠습니다~!
```html {.line-numbers}
<script type="module">
  // 변수 ↓ 이 부분은 작명 가능
  import 변수 from 'js/library.js'
  console.log(변수); // 10
</script>
```
- `export default`를 쓰면 `import`할 때 이름 바꿔도 됨
- <span style="color:#f84d3a;">`export default`는 파일당 1회만 사용</spa>

### 3. export
##### 여러개 내보내는 `export` 문법
```js {.line-numbers}
// 📄 library.js
var a = 10;
var b = 20;

// 중괄호 필수, default 키워드 안씀
export {a};
export {b};
// 또는
export {a, b};
// export키워드는 변수/함수 선언 왼쪽에 써도 됨
export var c = 30;
```
- 어떻게 그럼 `import`?
  ```html {.line-numbers}
  <script type="module">
    import {a, b} from 'js/library.js'
  </script>
  ```
  - `export/import`시 변수이름 똑같이 써야함

### 4. export / export default 동시 사용하면?
- 동시에 사용 가능
```js {.line-numbers}
var a = 10;
var b = 20;
var c = 30;

export {a, b};
export default c;
```
- `import`하는 방식이 조금 달라지는 것 뿐
```html {.line-numbers}
<script type="module">
  import {a, b} from 'js/library.js';

  // c를 import 할 땐?
  import 변수 fromm 'js/library.js';
  console.log(변수); // 30

  // 동시에 import는?
  // 순서가 중요 (기본적으로 import하는걸 왼쪽에 적어주기)
  import 작명, {a,b} from 'js/library.js';
</script>
```
### 5. 변수명이 맘에 안들면 새로 지어도 됨
```js {.line-numbers}
var a = 10;
var b = 20;
var c = 30;

export {a, b};
export default c;
```
- `a`라는 변수명이 너무 맘에 안들 땐?
```html {.line-numbers}
<script type="module">
  // import {변수 as 새변수명} from '경로'
  import {a as 별명} from 'js/library.js';
  console.log(별명); // 10
  console.log(a); // 없다고 에러뜸
</script>
```
### 5. `*` 기호로 모두 import 해오기 
- 전부 다 import 해오겠다!
```html {.line-numbers}
<script type="module">
  // import * as 변수들명 from '경로'
  import * as 별명 from 'js/library.js';
  console.log(별명); // Module{~~}
  console.log(별명.a); // 10
  console.log(별명.b); // 20
  console.log(별명.a); // undefined
</script>
```
- `c`가 `undefined`로 출력되는 이유?
  ```html {.line-numbers}
  <script type="module">
  // export default는 그 방식대로 import 해야함
  import 디폴트, * as 별명 from 'js/library.js';
  console.log(디폴트); // 30
  </script> 
  ```

### <span style="color:#348b58">import/export 정리</span>
- 모든 자료 다 export 가능 (함수도 당연히 가능)
- 옛날 방식
  ```html {.line-numbers}
  <script type="module">
    // var 변수 = require('경로')
    var 임포트해온것들 = require('js/library.js');
  </script>
  ```
- IE에서 안됨 => 호환성이 아직 좋지 않음
- 프론트엔드 개발에선 `<script src=""></script>`로 쓰기
- React, Angular 쓸 때 많이 사용하고, JS 파일 나눌 때 사용함
