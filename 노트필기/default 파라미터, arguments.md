자바스크립트 함수 업그레이드하기
==
(default parameter/arguments)
--

### 1. 함수의 default 파라미터 넣기
```js {.line-numbers}
function 더하기(a, b) {
  console.log(a+b);
}
더하기(1);
```
- 파라미터가 2개 들어가는 함수인데 1개만 써도 에러가 안남
```js {.line-numbers}
function 더하기(a, b = 10) {
  console.log(a+b);
}
// b자리에 아무것도 안넣을 경우 10을 넣어주세요~
더하기(1); // 결과값 : 11
더하기(1,2); // 결과값 : 3
더하기(); // 결과값 : NaN
```
- b자리에 파라미터 안넣었을 때만 발동
```js {.line-numbers}
function 더하기(a, b = 2 * 5) {
  console.log(a+b);
}
```
- 수학 연산자도 가능
```js {.line-numbers}
function 더하기(a, b = 2 * a) {
  console.log(a+b);
}
더하기(1); // 1 + (2 * 1) = 3
```
```js {.line-numbers}
function 임시함수() {
  return 10
  // return 함수 실행하고 남길 값
}
임시함수() // 10
function 더하기(a, b = 임시함수()) {
  console.log(a+b);
}
더하기(1); // 11
```
- 연산, 함수 가능하다.
### <span style="color:#348b58">default 파라미터 정리</span>
- 함수를 만들 때 파라미터값을 실수로 안적거나 했을 경우 
- 파라미터에 기본값(default 값)을 줄 수 있음
- 주는 방법은 파라미터 선언할 때 등호로 입력해주면 됨
- 파라미터가 정의 되지 않았을 경우 등호 오른쪽 값이 발동됨
- default 파라미터로 다양한 것들이 들어갈 수 있음
  - 수학연산자
  - 다른 파라미터와 연산도 가능
  - 함수도 가능


### 2. 함수의 arguments
- 파라미터를 `arguments`라고 부름
```javascript {.line-numbers}
function 함수(a, b, c) {
  // console.log(a,b,c);
  console.log(arguments);
}
함수(1,2,3); // array 비슷한 형태로 출력
```
- 모든 파라미터를 한꺼번에 싸잡아서 다루고 싶을 경우 = arguments 사용하세요!
- arguments란?
  - 모든 파라미터를 `[ ]` 안에 넣은 변수
  - `arguments[i]` 식으로 나타낼 수 있음
```javascript {.line-numbers}
function 함수(a, b, c) {
  console.log(a);
  console.log(b);
  console.log(c);
}
// 만약 파라미터가 엄청 많다면?
```
```javascript {.line-numbers}
function 함수(a, b, c) {
  for(var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}
함수(2,3,4);
```
- 입력한 파라미터를 전부 콘솔창에 출력해주는 함수?
  - `arguments`를 활용한 확장성 가득한 코드 사용하기

### <span style="color:#348b58">arguments 정리</span>
- 함수의 모든 파라미터들을 전부 한꺼번에 싸잡아서 다루고 싶은 경우 사용
- 함수 안에서 쓸 수 있는 미리 정의된 키워드 혹은 변수이다.