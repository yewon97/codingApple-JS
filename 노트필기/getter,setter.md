getter, setter 대체 왜 쓰는지 알아보기
==

### 1. getter/setter
```js {.line-numbers}
var 사람 = {
  name : 'Park',
  age : 30
}

// age라는 자료를 꺼내고 싶으면?
사람.age; // 30
```
- age라는 자료를 꺼내고 싶으면?
  - 자료를 꺼내는 법을 만들어서 꺼냄
  ```js {.line-numbers}
  // 내년 age를 알고 싶음
  var 사람 = {
    name : 'Park',
    age : 30,
    nextAge() {
      return this.age + 1
    }
  }

  사람.age + 1; 
  사람.nextAge(); // 31
  ```
  - `사람.age + 1;` 방법으로 하면 더 쉬운데 `nextAge()` 왜 만들어쓰지?
  - 함수를 만들어 object 데이터를 다루는 이유?
    1. object 자료가 복잡할 때 이득
    2. object 자료 수정시...
        ```js {.line-numbers}
        // 데이터를 바꿀 때
        사람.age = '20'; // <- 데이터 수정시 실수 유발 가능
        ```
        ```js {.line-numbers}
        var 사람 = {
          name : 'Park',
          age : 30,
          nextAge() {
            return this.age + 1
          }
          // 나이 바꾸는 함수 
          setAge(나이) {
            this.age = 나이;
          }
        }

        사람.setAge(20); // 이러면 age가 20으로 변하는 함수
        사람.age = 20; // 오브젝트를 직접적으로 바꾸긴 보단 위와 같은 방식 선호
        ```
        ```js {.line-numbers}
        var 사람 = {
          name : 'Park',
          age : 30,
          nextAge() {
            return this.age + 1
          }
          setAge(나이) {
            this.age = parseInt(나이); // 데이터 수정 전 미리 검사 가능 (안전장치 기능개발 가능)
          }
        }
        
        사람.setAge('20'); // 문자로 데이터 타입을 입력해도 setAge()가 걸러줌 
        사람.age = '20'; // 하지만 직접적인 변경은 그대로 문자타입인 '20'이 들어감
        ```
        - 데이터를 꺼내거나/수정하거나 그럴 때 편리 & 실수방지 & 관리가능
- ES5부터 set/get 키워드 사용 가능 (복잡한 소괄호 꼴보기 싫을 때)
  ```js {.line-numbers}
  var 사람 = {
    name : 'Park',
    age : 30,
    nextAge() {
      return this.age + 1
    }
    set setAge(나이) {
      this.age = parseInt(나이);
    }
  }

  사람.setAge = '20' 
  ```
  - `set` 키워드 적어주면 됨
  - `set`은 데이터를 입력해주는, 수정해주는 함수에
  ```js {.line-numbers}
  var 사람 = {
    name : 'Park',
    age : 30,
    get nextAge() { // getter
      return this.age + 1
    }
    set setAge(나이) { // setter
      this.age = parseInt(나이);
    }
  }

  사람.setAge = '20' 
  사람.nextAge; // 21
  사람.nextAge() // 이렇게 소괄호 붙이면 함수가 아니라고 에러가 뜸
  ```
  - `get`은 데이터를 뽑아주는, 가져와주는 함수에
  - `get`은 프로퍼티화~ 시켜준다고 생각하면 됨
### 2. get/set 키워드 특징
- get 함수들(getter) -> <span style="color:#f84d3a;">return 이 있어야함, 파라미터 없어야함</span>
- set 함수들(setter) -> <span style="color:#f84d3a;">파라미터가 1개 있어야함 (무조건 1개만 가능)</span>

### 3. class에서 사용하는 get/set
- prototype 함수들에도 get/set 가능
```js {.line-numbers}
class 사람 {
  constructor() {
    this.name = 'Park';
    this.age = 20;
  }
  nextAge() { // 내년 나이를 출력해주는 함수
    return this.age + 1
  }
}

var 사람1 = new 사람();
사람1.nextAge(); // 21
```
- `get`키워드 사용하면?
  ```js {.line-numbers}
  class 사람 {
    constructor() {
      this.name = 'Park';
      this.age = 20;
    }
    get nextAge() { // 내년 나이를 출력해주는 함수
      return this.age + 1
    }
  }

  var 사람1 = new 사람();
  사람1.nextAge; // 21
  ```
- `set`키워드 사용하면?
  ```js {.line-numbers}
  class 사람 {
    constructor() {
      this.name = 'Park';
      this.age = 20;
    }
    get nextAge() { // 내년 나이를 출력해주는 함수
      return this.age + 1
    }
    set setAge(나이) {
      this.age = 나이;
    }
  }

  var 사람1 = new 사람();
  사람1.nextAge; // 21
  사람1.setAge = 200;
  사람1.setAge; // 200
  ```
- 데이터 출력/수정 함수를 만들어 쓰는 이유?
  - 데이터를 꺼내서 쓸 때, 꺼내서 쓸 수 있는 방법을 미리 정의하는 것