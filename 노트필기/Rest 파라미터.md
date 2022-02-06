함수에서 쓰는 점3개 Rest 파라미터를 알아봅시다
==
### 1. Rest 파라미터
- ES6부터는 약간 더 쉬운 문법을 제공
- `arguments`보다 좀더 쉬운 문법
```js {.line-numbers}
function 함수2(...rest) {
  console.log(rest);
}
함수2(1,2,3,4,5,6,7); 
// 함수 안에 들어온 파라미터를 전부 담은 array 출력
```
- `...` 의 의미
  1. spread operator
  2. rest 파라미터 : 함수 소괄호 안 파라미터 자리, 파라미터 왼쪽에 작성
- 파라미터 자리에 오는 모든 파라미터를 `[ ]`에 보관해줌

#### <span style="color:#f84d3a">Rest 파라미터와 arguments 차이점</span>
- arguments
  - 모든 파라미터를 `[ ]`에 담아줌
- rest 파라미터
  ```js {.line-numbers}
  function 함수2(a, b, ...파라미터들) {
  console.log(파라미터들);
  console.log(파라미터들[1]); // 4
  }
  함수2(1,2,3,4,5,6,7); // [3,4,5,6,7]
  ```
  - `...rest` 이 자리에 오는 모든 파라미터를 `[ ]`에 담아줌
  - `arguments`보다 조금 더 유연하고 매끄럽게 작성 가능

#### <span style="color:#f84d3a">Rest 파라미터와 Spread Operator 구분</span>
1. 함수 파라미터 자리에 붙으면 rest
2. 나머지는 spread operator

- 모든 파라미터들을 하나씩 콘솔창에 출력해주는 함수?
```js {.line-numbers}
function 함수2(...rest){
  console.log(rest[0]);
  console.log(rest[1]);
  console.log(rest[2]);
  console.log(rest[3]);
}
함수2(1,2,3,4)
```
```js {.line-numbers}
function 함수2(...rest){
  for(var i = 0; i < rest.length; i++) {
  console.log(rest[i]);
  }
}
함수2(1,2,3,4, 24, 242, 13, 91);
```
- `...rest`는 파라미터가 몇개 들어올지 미리 지정안해줘도 됨
```javascript {.line-numbers}
function 함수(a, b, c) {
  for(var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}
함수(2,3,4);
```
- `arguments`는 파라미터의 개수 정해줘야함
#### <span style="color:#f84d3a">Rest 파라미터 주의점</span>
1. 가장 뒤에 써야함
```js
function 함수2(...rest, a){
  for(var i = 0; i < rest.length; i++) {
  console.log(rest[i]);
  }
}
// 에러남
```
- rest라는 뜻 자체가 "여기 뒤에 있는 모든 파라미터"라는 뜻 
2. 두번 이상 사용 금지
```js {.line-numbers}
function 함수2(a, ...rest, ...rest1){
  for(var i = 0; i < rest.length; i++) {
  console.log(rest[i]);
  }
}
// 에러남
```

### <span style="color:#348b58">Rest 파라미터 정리</span>
- 원하는 파라미터 왼쪽에 ... 기호를 붙여주면
"이 자리에 오는 모든 파라미터를 `[ ]` 중괄호로 감싸준 파라미터" 라는 뜻