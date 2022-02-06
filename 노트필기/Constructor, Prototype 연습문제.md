constructor, prototype 연습문제 4개
==

1. 오브젝트 자료 여러개를 만들고 싶습니다. 
```js {.line-numbers}
var 학생1 = { name : 'Kim', age : 20 }
var 학생2 = { name : 'Park', age : 21 }
var 학생3 = { name : 'Lee', age : 22 }
```
- constructor문법을 사용해서 위의 오브젝트와 똑같은 오브젝트 3개를 한번 뽑아보십시오.  
여기에 학생1.sayHi()라고 사용하면 "안녕 나는 Kim이야" 라는 글자가 콘솔창에 나오도록 sayHi()라는 함수도 constructor 안에 추가해보세요.
- 답 : 
  ```js {.line-numbers}
  function Student(name, age) {
    this.name = name;
    this.age = age;
    this.sayHi = function() {
      console.log(`안녕 나는 ${this.name} 이야`);
    }
  }

  var 학생1 = new Student('Kim', 20);
  var 학생2 = new Student('Park', 21);
  var 학생3 = new Student('Lee', 22)
  ```

2. 다음 코드의 출력 결과는 무엇일까요?
```js {.line-numbers}
function Parent(){
  this.name = 'Kim';
}
var a = new Parent();

a.__proto__.name = 'Park';
console.log(a.name)
```
- 답 : `Kim` 
- 해설 : `Parent` 생성자 함수에 의해 `a = { name : 'Kim' }`이 되고,
`a.__proto__.name = 'Park';` 이건 부모 prototype에 `{ name : 'Park' }` 이걸 추가하라는 뜻
`a.name` 이라고 사용했을 때, 내가 직접 가지고 있는 `{ name : 'Kim' }` 이걸 우선 출력

3. 함수가 안들어가요 엉엉
- 위에 1번 문제에서 Student라는 부모에 sayHi라는 함수를 하나 추가하라고 했죠?
그래서 sayHi()라고 사용하면 "안녕 나는 ~~이야" 라고 내 이름을 출력해주는 함수를 prototype에 추가했습니다. 
하단처럼 만들었는데 의도한 대로 이름이 출력되지 않고 있습니다. 
원인은 무엇일까요? 
```js {.line-numbers}
function Student(이름, 나이){
  this.name = 이름;
  this.age = 나이;
}

Student.prototype.sayHi = () => {
    console.log('안녕 나는 ' + this.name + '이야');
  }
var 학생1 = new Student('Kim', 20);

학생1.sayHi(); //왜 이 코드가 제대로 안나오죠?
```
- 답 : 
  sayHi() 라는 함수를 prototype에 추가할 때 arrow function을 사용
  ```js {.line-numbers}
  Student.prototype.sayHi = () => {
    console.log(this); // window
    // 'use strict' 모드에선 undefined 출력
  }
  ```
  - arrow function은 그냥 일반 function 대체품이 아님.
  - arrow function은 `this`를 바깥에 있는 `this` 그대로 사용하고 싶을 때 쓰는 함수라고 함
  - `sayHi()` 함수를 만들 때 arrow function을 사용해서 내부에 있던 `this`라는 값이 이상해진 것

#### <span style="color:#348b58">- this 잠깜 복습해보기</span>
- 함수 안에서 `this`키워드의 뜻은 매번 재정의 됨
- object 안에 들어있는 함수 안에 있는 `this`는 함수를 부른 object가 된다고 했음
- 하지만 arrow function의 경우 함수 안에서 `this` 뜻이 재정의 되지 않고 바깥에 있던 this를 사용함

