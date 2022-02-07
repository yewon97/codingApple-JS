동기/비동기처리와 콜백함수라는 용어 깔끔하게 정리
==

### 1. 동기/비동기 처리 
- **<span style="color:#7244e7;">JS는 동기식처리(Synchronus)</span>**
  - 한번에 코드 한줄씩 차례로 실행
  - 병렬처리가 가능하다? -> <span style="color:#f84d3a;">아님!!!</spa>
  ```js {.line-numbers}
  console.log(1);
  console.log(2);
  console.log(3);
  // 1 → 2 → 3
  // 코드 한줄씩 차례로 출력됨
  ```
- 비동기식치리 (Asynchronous)
  ```js {.line-numbers}
  console.log(1);
  setTimeout(()=>{ console.log(2); }, 1000);
  console.log(3);
  // 1 → 3 → 1초 후에 → 2
  ```
  - `setTimeout`는 자바스크립트가 비동기식처리할 수 있게 도와주는 함수임
  - 비동기식이란? 오래걸리는 작업이 있으면 제껴두고 다른거부터 처리하는 방식
  (JS가 아니라 자바스크립트 실행하는 브라우저 덕분에 가능)
  ```js {.line-numbers}
  setTimeout(()=>{ console.log(2); }, 1000);
  element.addEventListener('click', function(){});
  $.ajax()
  
  // 오래걸리거나 실행까지 오래걸리는 함수들
  ```
  - <strong>Web API</strong>라는 실행대기실로 코드를 보내서 해결이되길 기다린 다음 코드를 실행시켜줌
- Web API 덕분에 오래걸리는 작업이 있으면 제껴두고 다른거부터 처리하는 비동기식 처리가능
  - 원래 JS는 오래걸리는 연산 만나면 멈춤<span style="color:#FDAC53;">(= 동기식 처리, Synchronus)</span>
  - Web API와 연관된 특수한 함수들을 쓰면 작업이 오래걸릴 때 다른거부터 실행가능<span style="color:#FDAC53;">(= 비동기식 처리, Asynchronous)</span>
  - 웹브라우저의 특수성 때문에

### 2. 콜백함수
- JS를 순차적으로 실행하고 싶으면?
  ```js {.line-numbers}
  console.log(1);
  setTimeout(()=>{}, 1000);
  console.log(2);
  ```
  - 이렇게 쓴다고 해서 1초 쉬고 2를 출력하지 않음 (`setTimeout`함수 쓰면 비동기적)
  - 자바스크립트 실행머신인 웹브라우저는 이런 특수한 코드들을 발견하면 약간 제쳐두고 다른 코드부터 실행하려고 함 (<u>언어 자체의 기능이 아니라</u> 자바스크립트 실행을 도와주는 <u>웹브라우저 덕분에</u> 해낼 수 있는 것)
- 그럼 순차적으로 실행하게 하려면? <span style="color:#f84d3a;">=> 콜백함수를 사용함</span>
  ```js {.line-numbers}
  console.log(1);
  setTimeout(()=>{ console.log(2); }, 1000);
  // 1 -> 1초 쉬고 -> 2 출력

  addEventListener('click', function() {});
  // addEventListener의 저기 함수 부분도 콜백함수임
  ```
- 콜백함수 : 함수 안에 들어가는 함수
  ```js {.line-numbers}
  addEventListener('click', function() {});
  addEventListener('click', () => {}); // arrow function
  addEventListener('click', 함수); // 함수의 이름만 적어줌
  // 함수() <- 소괄호는 함수를 실행시켜주세요~ 라는 의미임
  ```
### <span style="color:#348b58">콜백함수를 이용한 함수 디자인</span>
```js {.line-numbers}
function 첫째함수() {

}

function 둘째함수() {

}

// 첫째함수() 다음에 둘째함수()를 실행하고 싶음 
첫째함수();
둘째함수();
// -> 실패할 수 있음 
// 첫째함수가 오래걸리는 코드라면 다른 곳으로 보내질 수 있음 (Web API라는 공간)
```
- `첫째함수()` 다음에 `둘째함수()`를 실행하고 싶음 -> 위와 같은 방식 실패할 수 있음 
- 그래서 콜백함수를 이용해서 작성함
```js {.line-numbers}
function 첫째함수(구멍) {
  console.log(1);
  구멍(); // 소괄호는 함수를 실행해주세요!
}

function 둘째함수() {
  console.log(2);
}

첫째함수(둘째함수); // <- 파라미터 사용하려면 파라미터 구멍을 뚫어줘야함
// 첫째함수를 실행한 다음에 둘째함수를 실행해주세요~!
```
- 콜백함수는 비동기 동기와 상관이 없음
  - `setTimeout`, `addEventListener` 문법 사용해야 비동기처리 가능한 것이고
- <span style="color:#7244e7">그냥 콜백함수는 함수 디자인 패턴일 뿐</span>
```js {.line-numbers}
첫째함수(둘째함수); // 미리 만들어놓은 함수를 집어넣을 수도 있고

첫째함수(function() { // 이렇게 직접 함수선언문을 집어넣을 수도 있음
  console.log(2); 
}); 
```

### <span style="color:#f84d3a">콜백함수의 문제점</span>
- 순차적으로 실행하려고 콜백함수를 여러개 사용하면 단점이 조금 있음. 
- 코드가 옆으로 길어짐 (아래 예시 참조)
```js {.line-numbers}
첫째함수(function() {
  둘째함수(function() {
    셋째함수(function() {
      ...
    })
  })
}); 
```
- 더 쉽게 쓰기 위한 Promise 패턴
  - ES6 신문법인 Promise라는 기계를 만들어 사용
  ```js {.line-numbers}
  첫째함수().then(function(){

  }).then(function(){
    
  })
  ```
  - 옆으로 길어지지 않고 `then`이라는 키워드 덕분에 그나마 뭘 하는지도 파악이 쉬워짐
