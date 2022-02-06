Spread, rest 파라미터 연습문제 8개
==

1. spread 문제 1

```js {.line-numbers}
var a = [1, 2, 3];
var b = '김밥';
var c = [...b, ...a];
console.log(c);
```

- 답 :
  ```console
  [ '김', '밥', 1, 2, 3 ]
  ```

2. spread 문제 2

```js {.line-numbers}
var a = [1, 2, 3];
var b = ['you', 'are'];
var c = function (a, b) {
  console.log([[...a], ...[...b]][1]);
};
c(a, b);
```

- 답 :
  ```console
  [ [1, 2, 3], 'you', 'are' ]의 [1] 데이터
  [ 'you' ]
  ```

3. default 파라미터 문제 1

```js {.line-numbers}
function 함수(a = 5, b = a * 2) {
  console.log(a + b);
  return 10;
}
함수(3);
```

- 답 :
  ```console
  9
  ```

4. default 파라미터 문제 2

```js {.line-numbers}
function 함수(a = 5, b = a * 2) {
  console.log(a + b);
}
함수(undefined, undefined);
```

- 답 :
  - default 파라미터가 다 발동(파라미터에 아무것도 안집어넣었다고 판단 중) 
  - `undefined` 는 정의 안된 파라미터
  ```console
  15
  ```

5. array를 만들어주는 함수를 제작하고 싶습니다.

```js {.line-numbers}
function 어레이(){
  (여기 어떤코드가 들어가면 될까요?)
}

var newArray = 어레이(1,2,3,4,5);
console.log(newArray); // [1,2,3,4,5]
```

- 답 :
  ```js {.line-numbers}
  function 어레이(...rest) {
    return rest;
  }
  ```

6. 최댓값 구하기

```js {.line-numbers}
Math.max(5, 6, 4, 3); // 6
```

```js {.line-numbers}
var numbers = [2, 3, 4, 5, 6, 1, 3, 2, 5, 5, 4, 6, 7];
```

- 답 :
  ```js {.line-numbers}
  Math.max(...numbers);
  ```

7. 글자를 알파벳순으로 정렬해주는 함수를 만들고 싶습니다.

```js {.line-numbers}
console.log(['b', 'c', 'a'].sort());

//[ 'a', 'b', 'c' ] 출력됨
```
- array 자료형 글자순 정렬은 `.sort()` 사용해서 함
```js {.line-numbers}
function 정렬(){
  (여기 어떤 코드가 들어가야할까요?)
}
정렬('bear');
```

- 답 :
  ```js {.line-numbers}
  function 정렬(글자){
    // 1. 
    [...글자].sort();
    // 2. 
    [...글자].sort().join();
    // 3. 
    ...[...글자].sort().;
  }
  ```

8. 데이터마이닝 기능 만들기

- 데이터분석 하는 사람들이 자주 만들어 쓰는 함수가 있습니다.
  알파벳들의 출현 갯수를 세어주는 함수입니다. 우리도 한번 만들어봅시다.
  글자세기('aacbbb') 라고 입력하면 콘솔창에
  { a : 2, b : 3, c : 1 }
  ▲ 이렇게 출력해주는 글자세기() 라는 함수를 만들고 싶습니다.
  쉽게말하자면 입력한단어에 들어있는 알파벳의 갯수를 세어서 오브젝트에 기록해주고 출력까지 해주는 함수입니다.
  글자세기라는 함수를 어떻게 만들면 될까요?

- 답 :
  - `forEach`는 array에서만 반복문 돌릴 수 있음
  ```js {.line-numbers}
  function 글자세기(글자){
    var 결과 = {};
    [...글자].forEach(function(a) {
      // 만약에 결과에 a가 있으면 +1를 해주시고,
      // 없으면 a : 1 집어넣으세요
      if(결과[a] > 0) {
        결과[a] = 결과[a] + 1;
      } else {
        결과[a] = 1;
      }
    });
    console.log(결과);
  }
  ```


#### <span style="color:#348b58">객체 식별자 네이밍 규칙</span>
- `.` 또는 `[]`
- *프로퍼티 Key*는 *프로퍼티 Value*에 접근할 수 있는 이름으로서 식별자 역할을 함
- 프로퍼티 키로는 주로 문자열을 사용 (심벌 값도 가능) -> 문자열이라서 `''` `""`로 감싸야함
- 대신 식별자 네이밍 규칙을 준수하는 이름, JS에서 사용 가능한 유효한 이름은 따옴표 생략 가능
- 식별자 네이밍 규칙을 따르지 않는 이름만 반드시 따옴표 사용
- 식별자 네이밍 규칙이란 <strong>카멜표기법</strong>을 말함

#### <span style="color:#348b58">객체의 프로퍼티 참조</span>
> 1. object.property (Dot Notation) 
> 2. object['property'] (Bracket Notation)
- Dot Notation
  - 어떤한 객체의 Key값을 정확히 알고 있을 때 사용할 수 있는 방법
  - 코딩하는 그 순간 우리가 정말 그 키에 해당하는 값을 받아 오고 싶을 때 씀
  ```js {.line-numbers}
  function printValue(obj, key) {
  console.log(obj.key);
  }
  printValue(ellie, 'name'); // undefined
  ```
  - 코딩하는 시점에는 key를 전혀 알 수 없음
  - `obj.key`라고 하게 되면
  `obj`에 `key`라는 프로퍼티가 들어있어? 라고 물어보는 것

- Bracket Notation 
  - Dot Notation과 사용법은 비슷하지만 가장 중요한 차이점은 Bracket안에 <span style="color:#f84d3a">변수를 담을 수 있다는 점</span>
  ```js {.line-numbers}
  let obj = {
	cat: 'meow',
	dog: 'woof',
  };

  let dog = 'cat';    			let dog = 'cat';
  let sound = obj[dog];			let sound = obj.dog;

  console.log(sound); // meow		console.log(sound); // woof
  ```
  - 주로 함수의 파라미터로 객체의 프로퍼티를 참조하고 싶을때 사용하는 방법
  - 실시간으로 원하는 키의 값을 받아오고 싶을 때 사용