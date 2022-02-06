이상한 Reference data type과 더 이상한 예제 3개
==
### 1. Primitive data type
- 변수에 값이 그대로 저장됨
- 그냥 문자와 숫자는 `primitive data type`
### 2. Reference data type
- 변수에 reference가 저장됨
- Array, Object는 변수에 값이 저장이 안됨
- 자료를 변수에 직접 저장하는게 아닌, 
"자료가 저쪽에 있습니다" 라는 화살표 (레퍼런스)를 변수에 저장

#### <span style="color:#348b58">- Primitive data type 다루기 : 복사</span>
```js {.line-numbers}
var 이름1 = '김';
var 이름2 = 이름1;
```
- `이름2`를 출력하면? `김`
```js {.line-numbers}
이름1 = '박';
```
- `이름2`를 출력하면? `김`
- `이름1`를 출력하면? `박`

#### <span style="color:#348b58">- Reference data type 다루기 : 복사</span>
##### 1. 복사하면 이상한 일이 일어남
```js {.line-numbers}
var 이름1 = { name : '김'};
이름1.name; // 김
var 이름2 = 이름1;

// object 수정
이름1.name = '박'; 
이름1.name; // 박

// 이름2는 데이터 변경한 적이 없는데?
이름2.name; // 박
```
- 이름1에는 `{ name : '김'}`이 저장되는 것이 아님
- "`{ name : '김'}`이 저기있어요~"라는 화살표(reference)가 저장됨
즉, 이름1의 <u>화살표</u>를 이름2에 복사한 것
- <span style="color:#f84d3a">그래서 array, object는 함부로 복사하면 큰일남!</span>
- 그럼 복사하고 싶으면 어떻게? => object 복사 기계를 만들어야함
```js {.line-numbers}
var 이름1 = { name : '김'};
var 이름2 = { name : '박'};
``` 
- 새로 중괄호를 할당할 때마다 새로운 화살표 생긴다고 생각하면 좋음
##### 2. 화살표가 할당되는 기준 & object 두개가 같은지 비교해보기
```js {.line-numbers}
var 이름1 = { name : '김'};
var 이름2 = { name : '김'};

이름1 == 이름2; // false
이름1 === 이름2; // false
``` 
- 왜 `false`가 나올까?
화살표가 저장되어 있기 때문에 각각 다른 화살표이기 때문에 다르다고 나옴

##### 3. 함수를 이용해 object를 변경하면 어떻게 될까
- 오브젝트를 변경해주는 함수
  ```js {.line-numbers}
  var 이름1 = { name : '김'};

  // 오브젝트를 변경해주는 함수
  function 변경1(obj) {
    obj.name = 'Park';
  }

  변경(이름1); // { name : 'Park'}
  ```
- 오브젝트를 재할당해주는 함수
  ```js {.line-numbers}
  var 이름1 = { name : '김'};

  // 오브젝트를 재할당해주는 함수
  function 변경(obj) {
    obj = { name : 'Park'};
  }

  변경(이름1); // { name : '김'}
  ```
  - 값이 `{ name : 'Park'}`로 안바뀜, 실패!
  - 이유? reference와 parameter의 합작
  파라미터라는 문법과 레퍼런스 데이터 타입이라는 문법이 있는데 두개의 합작품임
    ```js {.line-numbers}
    var 이름1 = { name : '김'};

    function 변경(obj) {
      obj = { name : 'Park'};
    }

    변경(var obj = 이름1);
    ```
  - 파라미터는 변수생성 & 할당과 똑같음
  - `obj`라는 파라미터 자리에 이름1이라는 변수를 집어넣으면
  `var obj = 이름1` 이렇게 파라미터형 변수를 만듬
  - `obj` 라는 변수에 `이름1`이라는 { object } 를 등호로 복사해서 넣으면
  `obj, 이름1` 이 두개 변수는 서로 같은 화살표를 갖게 되며 `{ name : '김' }` 값을 공유
  - 함수 내부를 잘 보시면 obj라는 변수는 `obj = { name : 'Park' }` 이렇게 재할당을 해주고 있음 
  `obj`라는 변수에 새로운 화살표를 재할당을 한 것이지 실제 `이름1`이라는 변수는 전혀 건드리지 않고 있음
  - 결국 `이름1`은 바뀌지 않는 것
