틀린그림 찾기능력이 향상되는 Destructuring 문법
==

> Destructuring : 패턴매칭


### 1. array 데이터를 전부 변수에 담으려면?
```js {.line-numbers}
// array 하나 생성
var arr = [2,3,4];

// 각각의 요소를 각각 변수에 담고 싶음
// 방법1 -> 너무 하드코딩임
var a = arr[0];
var b = arr[1];
var c = arr[2];

//방법2
var [a,b,c] = [2,3,4];
```
- 모양만 맞춰 변수를 선언하면 변수가 생김
- 직관적으로 변수 만들 수 있음
- array Destructuring 할 때 몇개를 빼먹는다면?
  ```js {.line-numbers}
  var arr = [2,3,4];

  // c의 값을 빼먹음
  var [a,b,c] = [2,3];

  // 등호로 기본값 지정 가능
  var [a,b,c = 10] = [2,3];
  ```
- 아예 지정을 해주지 않는다면?
  ```js {.line-numbers}
  var [a,b,c = 10] = [];

  a; // undefined
  b; // undefined
  c; // 10
  ```
  - 변수 선언만 하면 원래 `undefined`가 들어감

### 2. object 데이터를 꺼내 변수에 담으려면?
```js {.line-numbers}
var obj = { name : 'Kim', age : 30 };

// 중요한 정보를 변수로 만들려면?
// 방법1
var name = obj.name;
var age = obj.age;

// 방법2
var { name, age } = { name : 'Kim', age : 30 }; // object destructuring
```
- object는 변수명을 **key명**과 똑같이 써야함 (위치를 맞춰주는건 array)
- default 파라미터 지정가능
  ```js {.line-numbers}
  var { name, age = 31 } = { name : 'Kim' };
  ```
- 변수명도 바꿀 수 있음
  ```js {.line-numbers}
  var { name : 이름, age = 31 } = { name : 'Kim' };

  // 변수명 + default 파라미터도 주고 싶으면?
  var { name : 이름 = 'Park' } = { };
  ```

### 3. 반대로 변수들을 object에 집어넣고 싶은 경우
```js {.line-numbers}
var name = 'Kim';
var age = 30;

var obj = { name : name, age : age };
obj; // { name : 'Kim', age : 30 }
```
- ES6 문법
  ```js {.line-numbers}
  var name = 'Kim';
  var age = 30;

  var obj = { name : name, age : age };
  var obj = { name, age };
  ```
  - `key` == `value`면, 축약가능

### 4. 함수 파라미터 만들 때도 destructuring 문법 사용가능
```js {.line-numbers}
var obj = { name : 'Kim', age : 30 };

function 함수(파라미터) {
  console.log(파라미터);
}

함수(obj); // { name : 'Kim', age : 30 }
```
- object 데이터들을 파라미터로 만들고 싶은 경우
  ```js {.line-numbers}
  var obj = { name : 'Kim', age : 30 };

  function 함수({ name, age }) {
    console.log(name); // "Kim"
    console.log(age); // 30
  }

  함수({ name : 'Kim', age : 30 });
  ```
- array의 경우
  ```js {.line-numbers}
  function 함수( [ name, age ]) {
    console.log(name); // 1
    console.log(age); // 2
  }

  함수([1,2]);
  ```

### 5. 연습문제
1. 변수를 마구 만들었는데 말입니다..
```js {.line-numbers}
var [number, address] = [ 30, 'seoul' ];
var {address : a , number = 20 } = { address, number };
```
- 약간 복잡해서 여러분께 물어보겠습니다.
a와 address와 number라는 변수는 각각 무슨 값을 가지고 있을까요? 
- 답 :
  ```
  a = 'seoul'
  address = 'seoul'
  number = 30
  ```
  ```js
  var {address : a , number = 20 } = { address : 'seoul', number : 30 };
  ```
2. 다음과 같은 Object에서 데이터를 뽑아서 변수를 만들고 싶습니다. 
```js {.line-numbers}
let 신체정보 = {
  body: {
    height: 190,
    weight: 70
  },
  size: ["상의 Large", "바지 30인치"],
};
```
- 여러분의 뛰어난 신체 정보를 담은 Object입니다. 
여기서 키, 몸무게, 상의사이즈, 하의사이즈 정보를 각각 뽑아서 4개의 변수를 만들고 싶습니다.
- 어떻게 만들면 될까요?
- (참고 : 데이터가 얼마나 복잡하든간에 좌우 형태를 똑같이 맞추시면 destructuring 문법으로 변수를 만들 수 있습니다.)
- 내가 푼 것 :
  ```js {.line-numbers}
    var {height : 키, weight : 몸무게} = { 
      height: 신체정보.body.height, 
      weight: 신체정보.body.weight 
    }
    var [상의사이즈, 하의사이즈] = [신체정보.size[0], 신체정보.size[1]];
  ```
- 정답+해설
  ```js {.line-numbers}
  let {
    body: {
      height, 
      weight
    },
    size: [ 상의, 하의 ]
  } = 신체정보;
  ```
  - 등호를 이용해서 좌우를 똑같이 맞춤
  - 왼쪽엔 변수명을 적어줌
  - `height`, `weight`, `상의`, `하의`라는 이름의 변수가 생성됨

