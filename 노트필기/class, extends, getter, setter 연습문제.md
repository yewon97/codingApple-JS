class, extends, getter, setter 연습문제 5개
== 

#### 1. 직접 class 구조 만들어보기
- 갑자기 강아지 SNS를 만들고 싶어서 자바스크립트로 열심히 코딩하던 중, 
여러 강아지 정보들을 담은 유사한 오브젝트 자료형을 테스트삼아 몇개 만들려고 합니다. 
```js {.line-numbers}
var 강아지1 = { type : '말티즈', color : 'white' };
var 강아지2 = { type : '진돗개', color : 'brown' }; 
```
- 이렇게 쭉 많이 만들고 싶은데 하드코딩하기 싫어서 class를 만들어 강아지 오브젝트들을 뽑고 싶습니다.
그럼 class를 어떻게 만드는게 좋을까요? 
- 답 : 
  ```js {.line-numbers}
  class Dog {
    constructor(type, color) {
      this.type = type;
      this.color = color;
    }
  }

  var 강아지1 = new Dog('말티즈', 'white');
  var 강아지2 = new Dog('진돗개', 'brown');
  ```

#### 2. 이번엔 고양이관련 object들을 만들고 싶습니다. 
- 이번엔 class를 이용해 고양이 오브젝트 여러개를 뽑고 싶습니다. 
```js {.line-numbers}
var 고양이1 = { type : '코숏', color : 'white', age : 5 };
var 고양이2 = { type : '러시안블루', color : 'brown', age : 2 }; 
```
- type, color는 이전에 만든 강아지 object와 유사한데
고양이들만 특별하게 age라는 속성을 하나 더 추가하고 싶습니다. 어떻게 class를 만들면 될까요? 
1번 문제에서 만들었던 강아지 class와 유사하기 때문에 extends라는 문법을 쓰는 것도 좋겠군요. 
- 답 : 
  ```js {.line-numbers}
  class Cat extends Dog {
    constructor(type, color, age) {
      super(type, color);
      this.age = age;
    }
  }

  var 고양이1 = new Cat('코숏', 'white', 5);
  var 고양이2 = new Cat('러시안블루', 'brown', 2);
  ```

#### 3. 고양이와 강아지 object들에 기능을 하나 추가하고 싶습니다. 
- 모든 고양이와 강아지 object들은 .한살먹기() 라는 함수를 사용할 수 있습니다. 
```
(1) 한살먹기 함수는 강아지 class로부터 생성된 오브젝트가 사용하면 콘솔창에 에러를 출력해주어야합니다. 
(2) 한살먹기 함수는 고양이 class로 부터 생성된 오브젝트가 사용하면 현재 가지고있는 age 속성에 1을 더해주는 기능을 실행해야합니다.
```
- 한살먹기 함수는 어떻게 만들면 좋을까요? (검색이 필요할 수 있습니다)
- 내가 푼 것 : 
  ```js {.line-numbers}
  class Cat extends Dog {
    constructor(type, color, age) {
      super(type, color);
      this.age = age;
    }
    한살먹기() {
      return this.age + 1
    }
  }

  고양이1.한살먹기();
  ```
- 정답 :
  ```js {.line-numbers}
  class Dog {
    constructor(타입, 칼라){
      this.type = 타입;
      this.color = 칼라;
    }
    한살먹기(){
        if( this instanceof Cat) {
        this.age++
        }
    }
  }
  ```
- 해설 : 
  - `한살먹기()` 함수는 Dog에 추가함 -> Cat, Dog 둘다 사용가능해야하기 때문에
  - Dog에 따로 Cat 따로도 가능하긴 함
  - 그래서 고양이들 강아지들은 전부 `한살먹기()`를 사용할 수 있음
  - 고양이들이 `한살먹기()`를 쓰면 나이를 `+1` 해주고, 강아지들이 쓰면 에러를 출력해줘야함 -> `if문`을 사용함
  - JS에는 `instanceof`라는 고마운 연산자가 있음
  - `a instanceof b` : a가 b로부터 생성된 오브젝트인지 아닌지를 `true/false`로 반환함
  - `this`가 `instanceof Cat`인 경우에만 실행하도록 if문 추가 -> `true`면 `this.age++`해줌

#### 4. get/set을 이용해봅시다
- 자바스크립트로 간단한 게임 기능을 가진 오브젝트를 뽑는 class를 만들고 싶습니다. 
다음 조건에 따라 class를 만들어보세요. class 이름은 Unit이라고 합시다.
```
(1) 모든 Unit의 인스턴스는 공격력, 체력 속성이 있으며 기본 공격력은 5, 기본 체력은 100으로 설정되어 있어야 합니다.
(2) 모든 Unit의 인스턴스는 전투력을 측정해주는 battlePoint라는 getter가 있습니다.
console.log( 인스턴스.battlePoint ) 이렇게 사용하면 현재 공격력과 체력을 더한 값을 콘솔창에 출력해주어야합니다.
(3) 모든 Unit의 인스턴스는 heal이라는 setter가 있습니다.
인스턴스.heal = 50 이렇게 사용하면 체력 속성이 50 증가해야합니다. 
```
- 인스턴스는 class로부터 새로생성되는 오브젝트를 뜻합니다.
- 답 :
  ```js {.line-numbers}
  class Unit {
    constructor() {
      this.공격력 = 5;
      this.체력 = 100;
    }
    get battlePoint() {
      return this.공격력 + this.체력;
    }
    set heal(value) {
      this.체력 += value;
    }
  }

  var character = new Unit();

  console.log(character.battlePoint);
  character.heal = 50;
  ```

#### 5. get/set을 이용해봅시다2 
- 다음과 같은 오브젝트가 있습니다. 
```js {.line-numbers}
var data = {
  odd : [],
  even : []
}
```
```
(1) data 오브젝트안에 setter 역할 함수를 하나 만들어보십시오.
setter 함수에 1,2,3,4 이렇게 아무 자연수나 파라미터로 입력하면 홀수는 odd, 짝수는 even 이라는 속성에 array 형태로 저장되어야합니다.   

(2) data 오브젝트안에 getter 역할 함수를 하나 만들어보십시오.
getter 함수를 사용하면 odd, even에 저장된 모든 데이터들이 숫자순으로 정렬되어 출력되어야합니다. 
```
- 예를 들면
  ```js {.line-numbers}
  data.setter함수(1,2,3,4,5) 이렇게 입력하면 
  data = { odd : [1,3,5], even : [2,4] }
  ```
  이렇게 저장이 되어야합니다. 
- 빨리 위의 역할을 하는 함수 두개를 data 오브젝트 내에 만들어보십시오. 
- 내가 푼 것 : 
  ```js {.line-numbers}
  var data = {
    odd : [],
    even : [],
    set seperator(...rest) {
      rest.forEach(function(num){
        if(num % 2 = 0) { // 짝수면

        } else { // 홀수면

        }
      })
    }
    get orderList() {
      return console.log(odd.sort(), even.sort())
    } 
  }
  ```
- 1번 정답+해설 : 
  - setter 함수 생성
  ```js {.line-numbers}
  var data = {
    odd : [],
    even : [],
    setter함수 : function(...숫자들){
      console.log(숫자들)
    }
  };

  data.setter함수(1,2,3);
  ```
  - `...`이라는 rest 기호를 쓰면 입력된 파라미터들을 전부 array로 싸매줌
  - `숫자들`이라는 파라미터모음 array를 하나씩 출력해보며 분류하면됨
    ```
    숫자들[0]이 홀수면 odd에 push 해주고, 짝수면 even에 push 해주고,
    숫자들[1]이 홀수면 odd에 push 해주고, 짝수면 even에 push 해주고,
    숫자들[2]이 홀수면 odd에 push 해주고, 짝수면 even에 push 해주고 ...
    ```
  - 반목문을 사용해서 만들면됨
  ```js {.line-numbers}
  var data = {
  odd : [],
  even : [],
  setter함수 : function(...숫자들){
      숫자들.forEach(function(a){
        a가 홀수면 this.odd에 push(a)하고.. 
        b가 짝수면 this.even에 push(a)하고...
      });
    }
  };

  data.setter함수(1,2,3);
  ```
  - 홀수와 짝수의 구분은 어떻게?
    - `숫자 % 2 = 0` 이면 짝수
    `숫자 % 2 = 1` 이면 홀수
  - if문을 써서 만들면됨
  ```js {.line-numbers}
  var data = {
    odd : [],
    even : [],
    setter함수 : function(...숫자들){
      숫자들.forEach(function(a){
        if ( a % 2 == 1 ) {
          this.odd.push(a)  //홀수일때
        } else {
          this.even.push(a)  //짝수일때
        }
      });
    }
  };

  data.setter함수(1,2,3); // 에러발생
  ```
  - 왜 제대로 작동 X?
    - `this.odd.push` 이 부분에서 에러남
    - `this.odd`는 내 오브젝트의 odd속성을 뜻하는게 아니라 
    `forEach`안의 `function(){}` 안에서 `this`는 `window`를 뜻함
    - 그래서 `this`를 위해 함수 모양을 arrow function으로 바꿔줘야함
  ```js
  var data = {
    odd : [],
    even : [],
    setter함수 : function(...숫자들){
      숫자들.forEach((a)=>{
        if ( a % 2 == 1 ) {
          this.odd.push(a)  //홀수일때
        } else {
          this.even.push(a)  //짝수일때
        }
      });
    }
  };

  data.setter함수(1,2,3);
  ```

- 2번 정답+해설 : 
  - array 두개를 합치려면 어떻게?
    - `spread operator` 문법을 이용하면 됨
  ```js {.line-numbers}
  var data = {
    odd : [1,3, 11],
    even : [2,4,6],
    get getter함수(){
      return [...this.odd, ...this.even ].sort()
    }
  };

  console.log(data.getter함수);
  ```
  - `...` 기호로 spread operator 문법을 이용해 `this.odd` 그리고 `this.even`이라는 array를 합치고
  - 정렬하고 싶으면 `sort()`만 뒤에 붙여주면됨 -> 근데 그러면 문자졍렬을 해줌
    ```js {.line-numbers}
    var data = [1, 11, 12, 2, 3, 4];
    data.sort(); // 결과값 [1, 11, 12, 2, 3, 4]
    ```
    - 숫자도 문자 순서대로 정렬되어 뒤죽박죽임 (1다음에 11이 옴)
  - 오름차순으로 정렬
    ```js {.line-numbers}
      get getter함수(){
        return [...this.odd, ...this.even ].sort(function(a, b){
          return a - b;
        })
      }
    ```
  - 내림차순으로 정렬
    ```js {.line-numbers}
      get getter함수(){
        return [...this.odd, ...this.even ].sort(function(a, b){
          return b - a;
        })
      }
    ```


