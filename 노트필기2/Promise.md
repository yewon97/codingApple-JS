인간의 언어로 설명하는 ES6 Promise
==

### 1. Promise 생김새 맛보기
- 콜백함수 디자인패턴이 마음에 안들면 Promise 디자인패턴 사용하면됨
- 이건 자바스크립트의 새로운 기능이라기보다는 코드/함수 디자인 패턴일 뿐
```js {.line-numbers}
var 프로미스 = new Promise();

프로미스.then(function() { // 프로미스가 성공일 경우 실행할 코드

})
```
- Promise는 콜백함수 만드는 것과 비슷하고
- 대신에 콜백함수보다는 약간 기능이 많음
```js {.line-numbers}
프로미스.then(function() {

}).then(function() {

})
```
- 옆으로 길어지지 않아서 좋음
```js {.line-numbers}
프로미스.then(function() {

}).catch(function() { // 실패할 경우에도 코드실행 가능

})
```
- `catch`를 이용해서 실패할 경우에도 코드실행 가능하다.
- 일반 콜백함수의 경우에는, 
  1번 실행 후 2번 실행해주세요~ 만 할 수 있었음
- Promise로 만드는 경우에는,
  1번 실행 후 성공시 2번 실행해주세요~
  실패시 3번 실행해주세요~
```js {.line-numbers}
프로미스.then(function() {

}).finally(function() { // 성공이던 실패던 뭔가 일어났을 때 실행시켜주세요

})
```

### 2. Promise 정의 & 디자인하는 법 
- 그냥 쓰면 안됨 -> 성공, 실패인지 판정을 해줘야함
> **외우자**
  Promise = 성공/실패 판정 기계
  ~일 경우 성공이고, ~일 경우 실패입니다 <- 정의해줘야함
```js {.line-numbers}
var 프로미스 = new Promise(function(resolve, reject){ // 2개의 파라미터 필수임

});
var 프로미스 = new Promise(function(성공, 실패){ // 2개의 파라미터 필수임
  성공(); // 성공 판정
  실패(); // 실패 판정
});

프로미스.then(function() { // 프로미스가 성공일 경우 실행할 코드

})
```
- 성공 판정이 나면 `then`안에 있는 코드를 실행시킴
  ```js {.line-numbers}
  var 프로미스 = new Promise(function(성공, 실패){ // 2개의 파라미터 필수임
    성공(); // 성공 판정
  });

  프로미스.then(function() { // 프로미스가 성공일 경우 실행할 코드
    // 성공() -> 여기 안에 코드 실행
  })
  ```
- 실패 판정 나면 `catch`안에 있는 코드 실행시킴
  ```js {.line-numbers}
  var 프로미스 = new Promise(function(성공, 실패){ // 2개의 파라미터 필수임
    실패(); // 실패 판정
  });

  프로미스.then(function() { // 프로미스가 성공일 경우 실행할 코드

  }).catch(function() { // 프로미스가 실패일 경우 실행할 코드
    // 실패() -> 여기 안에 코드 실행
  })
  ```

### <span style="color:#348b58">Promise 예시1</span>
- 어려운 연산이 끝나면 특정 코드를 실행하고 싶음
- 콜백함수를 만들든가 하면...
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 
  // 1 + 1 연산이 끝나면 성공() 판정을 내려주세요~
  var 어려운연산 = 1 + 1;
  성공();
  // 실패();
});

프로미스.then(function() { 
  console.log('성공했어요!');
}).catch(function() { // 실패(); 라면
  console.log('실패했어요!');
})
```
- 프로미스는 성공/실패를 판정해주는 **기계**
- 콜백 대신 예쁜 코드
- 성공/실패의 경우에 맞춰 각각 다른 코드 실행 가능

### 3. 성공/실패시 데이터 전달하기
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 
  var 어려운연산 = 1 + 1;
  성공(10); // 파라미터의 10이 then 함수까지 전해짐
});

프로미스.then(function() { 
  console.log('성공했어요!');
}).catch(function() { // 실패(); 라면
  console.log('실패했어요!');
})
```
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 
  var 어려운연산 = 1 + 1;
  성공(어려운연산); // 어려운연산을 then함수에 보내줌
});

프로미스.then(function(결과) { 
  console.log(결과); // 2
}).catch(function() { 
  console.log('실패했어요!');
})
```
### <span style="color:#348b58">Promise 예시2</span>
- 프로미스는 성공/실패 판정해주는 기계임
- 1초 후에 성공하는 Promise 그리고 성공시 특정 코드를 실행하고 싶음
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 
  setTimeout(function(){
    성공(); // 1초 후에 성공을 판정해주는 프로미스
  }, 1000);
});

프로미스.then(function(결과) { 
  console.log('연산이 성공했습니다' + 결과)
}).catch(function() { 
  console.log('실패했어요!');
})
```
1. 프로미스 기계 발동!
2. 성공/실패에 따라 코드실행

### 4. Promise 특징 정리
```console
프로미스;
Promise {<resolved>:undefined}
```
- 프로미스를 콘솔에 출력을 하면 프로미스는 오브젝트임 (중괄호로 시작함)
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 

});
```
- 성공/실패를 아무것도 개발안한 상태에서 console에 출력하면
  ```console
  프로미스;
  Promise {<pending>}
  ```

### <span style="color:#348b58">Promise의 3가지 상태</span>
1. 성공하면 <resolved>
2. 판정 대기중이면 <pending>
3. 실패하면 <rejected>
- 확인해 보고 싶으면 아래 코드 실행시켜보자!
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 
  setTimeout(function(){
    성공(); // 5초 후에 성공을 판정해주는 프로미스
  }, 5000);
});

프로미스.then(function() { 
  console.log('성공했어요!');
}).catch(function() {
  console.log('실패했어요!');
})
```
- console에
  ```console
  프로미스;
  Promise {<pending>}
  - 5초 후에 - 
  프로미스;
  Promise {<resolved>:undefined}
  ```
- 그리고 성공을 실패나 대기상태로 다시 되돌릴 순 없음. 참고로 알아두자. 

#### <span style="color:#f84d3a">Promise에 대한 오해</span>
- Promise는 비동기적 처리가 가능하게 바꿔주는 마법의 문법이 아님
- Promise는 비동기적 실행과 전혀 상관이 없음
- 그냥 콜백함수 디자인의 대체품일 뿐
  - 예를 들면.. Promise 안에 10초 걸리는 어려운 연산을 시키면 10초동안 브라우저가 멈춤
  10초 걸리는 연산을 해결될 때 까지 대기실에 제껴두고 그런거 아님 
- 그냥 원래 자바스크립트는 평상시엔 동기적으로 실행이 되며 비동기 실행을 지원하는 특수한 함수들 덕분에 가끔 비동기적 실행이 될 뿐
#### <span style="color:#1573ff">Promise가 적용된 곳들</span>
- jQuery.ajax()
- fetch()
```js {.line-numbers}
$.ajax().done(function(){}).fail();

// fetch : Promise를 리턴함 -> 프로미스가 이자리에 남음
fetch().then().catch()
```

## <span style="color:#348b58">Promise 연습문제</span>
#### 1. `<img>` 이미지 로딩 성공시 특정 코드를 실행하고 싶습니다. 
```html 
<img id="test" src="https://codingapple1.github.io/kona.jpg"> 
```
- 이 이미지가 로드가 되면 콘솔창에 성공, 로드가 실패하면 콘솔창에 실패를 출력하고 싶은데
Promise 문법의 then, catch 함수를 사용해 만들고 싶습니다. 어떻게 코드를 짜면 될까요?

- (참고) 이미지 로딩이 끝났다는 것은 `<img>`에 load라는 이벤트리스너를 붙여서 체크가 가능합니다. 
- (참고) 이미지 로딩이 실패했다는 것은 `<img>`에 error라는 이벤트리스너를 붙여서 체크가 가능합니다.
- 답 :
  ```js {.line-numbers}
  var 이미지로딩 = new Promise(function(성공, 실패) {
    let test = document.querySelector("#test"); 
    test.addEventListener('load', function() {
      성공();
    });
    test.addEventListener('error', function() {
      실패();
    });
  });

  이미지로딩.then(function() {
    console.log('로딩 성공');
  }).catch(function() {
    console.log('로딩 실패');
  })
  ```
  - 응용하기! -> 함수로 싸매놓으면 나중에 모든 이미지에 활용가능 (재사용 가능한 프로미스)

#### 2. Ajax 요청이 성공하면 무언가 코드를 실행하고 싶습니다. 
- https://codingapple1.github.io/hello.txt 라는 경로로 GET 요청을 하면 인삿말이 하나 딸려옵니다. 
- 여기로 GET 요청을 해서 성공하면,
 Promise의 then 함수를 이용해서 Ajax로 받아온 인삿말을 콘솔창에 출력해주고 싶습니다.
- 어떻게 하면 될까요? (jQuery done함수 자체에 Promise 기능이 있기 때문에 코드가 약간 중복도 많고 쓸데없을 수 있지만 연습삼아 해봅시다.)
```html
<!-- 이것은 jQuery Ajax 편리하니까 jQuery CDN 파일 -->
<script src="https://code.jquery.com/jquery-3.4.1.min.js"></script> 
```
- Ajax 사용방법
  ```js {.line-numbers}
  $.ajax({
    type : 'GET',
    url : 'URL 경로'
  })

  $.get('URL 경로') // -> 서버에 'URL 경로'에 있는 데이터를 요청함

  // 둘 중 맘에 드는 것 쓰기
  // URL 경로 상에 있는 데이터를 가져올 수 있음
  ```
  ```js {.line-numbers}
  // 가져온 데이터를 출력하거나 가져온 후에 특정 코드 실행하고 싶으면?
  $.ajax({
    type : 'GET',
    url : 'URL 경로'
  }).done(function(결과){
    console.log(결과);
  });

  $.get('URL 경로').done(function(결과){
    console.log(결과)
  });
  // done함수를 뒤에 붙여서 이렇게 쓰면 됨
  // '결과'라는 파라미터에 여러분이 가져온 데이터가 담겨있음 
  ```

- 답 :
  ```js {.line-numbers}
  const URL = 'https://codingapple1.github.io/hello.txt';
  // ajax
  $.ajax({
    type : 'GET',
    url : URL
  }).done(function(결과){
    console.log(결과);
  });

  // get
  $.get(URL).done(function(결과){
    console.log(결과)
  });
  ```
  - 콘솔창에 Ajax로 가져온 인삿말이 출력됨
  - 근데 이걸 성공하면 Promise의 then 함수로 뭔가 코드를 실행시키고 싶음
  - 그럼 Promise 기계를 하나 만들어주고, 그 안에 성공판정 기준도 하나 마련해주면됨
  ```js {.line-numbers}
  const URL = 'https://codingapple1.github.io/hello.txt';
  // ajax()
  var 프로미스 = new Promise(function(성공, 실패) {
    $.ajax({
      type : 'GET',
      url : URL
    }).done(function(결과){ // 결과 => Ajax 요청 결과(인삿말)
      성공(결과);
    });
  })

  // get()
  var 프로미스 = new Promise(function(성공, 실패) {
    $.get(URL).done(function(결과){ // 결과 => Ajax 요청 결과(인삿말)
      성공(결과);
    });
  })

  프로미스.then(function(결과) {
    console.log(결과);
  })
  ```
#### 3. Promise chaining
- 2번 문제에서 https://codingapple1.github.io/hello.txt 라는 경로로 GET 요청을 한 뒤에
`.then`을 이용해 인삿말을 콘솔창에 출력해보았습니다. 
이번엔 그 직후 https://codingapple1.github.io/hello2.txt 라는 경로로 GET 요청을 또 하고
`.then`을 이용해 인삿말을 또 출력해보고 싶습니다. 

- 쉽게 말하면 
  1. hello.txt GET 요청
  2. 그게 완료되면 hello2.txt GET 요청
  3. 그게 완료되면 hello2.txt 결과를 콘솔창에 출력

- 2번에서 만든 코드를 어떻게 업데이트하면 될까요?
- 힌트1) 프로미스.then(()=>{둘째실행할거}).then(()=>{셋째실행할거})
이렇게 then을 여러개 이어붙여 만들어봅시다.
- 힌트2) .then()은 당연히 new Promise()로 생성한 프로미스 오브젝트들에 붙일 수 있습니다. 

-  답 :
    ```js {.line-numbers}
    const URL = 'https://codingapple1.github.io/hello.txt';
    const URL2 = 'https://codingapple1.github.io/hello2.txt';
    var 프로미스 = new Promise(function(성공, 실패) {
      $.get(URL).done(function(결과){
        성공(결과);
      });
    })

    프로미스.then((결과) => {
      console.log(결과);

      return new Promise(function(성공, 실패) {
        $.get(URL2).done(function(결과){
          성공(결과);
        });
      });
    }).then(function(결과) {
      console.log(결과);
    })
    ```
    ```js {.line-numbers}
    const URL = 'https://codingapple1.github.io/hello.txt';
    const URL2 = 'https://codingapple1.github.io/hello2.txt';
    var 프로미스 = new Promise(function(성공, 실패) {
      $.get(URL).done(function(결과){
        성공(결과);
      });
    })

    // then을 여러개 붙여서 단계적으로 실행할 수 있음
    // 하지만 그냥 붙이면 안되고, then 함수는 new Promise()로 부터 생성된 오브젝트에만 붙일 수 있음
    // then을 붙일 수 있게 첫째 then에서 return new Promise() 해주면 되지 않을까?
    // return 해주면 그 자리에 new Promise()가 남아서 거기 뒤에 .then을 붙일 수 있으니까
    프로미스.then((결과) => {
      console.log(결과);

      var 프로미스2 = new Promise(function(성공, 실패) {
        $.get(URL2).done(function(결과){
          성공(결과);
        })
      });

      return 프로미스2;
      // 첫 then 안에 프로미스2를 만들고, 프로미스2는 두번째 ajax요청을 해주고 성공판정을 내림
      // 그리고 프로미스2를 return 해주는 기능을 만듬
      // 즉, .then()에서 new Promise()를 배출하면 뒤에 then을 또 쓸 수 있음
    }).then(function(결과) { // 이제 뒤에 then을 붙여서 코드를 연달아 실행이 가능
      console.log(결과);
    })
    ```
    ```js {.line-numbers}
    function ajax해주는함수(URL) {
      return new Promise((성공, 실패) => {
        $.get(URL).done(function(결과){
          성공(결과);
        })
      });
    }

    var 프로미스 = ajax해주는함수('https://codingapple1.github.io/hello.txt');

    프로미스.then((결과) => {
      console.log(결과);
      return ajax해주는함수('https://codingapple1.github.io/hello2.txt');
    }).then(function(결과) {
      console.log(결과);
    })
    ```
    - 흐름은 이렇다!
      1. 첫프로미스가 성공하면 then() 안의 코드를 실행
      2. 근데 거기 안에는 프로미스2가 있습니다. 프로미스2가 성공하면
      3. 뒤에 있는 then() 안의 코드를 실행
    - 그래서 이렇게 하시면 프로미스를 이용해 단계적으로 코드를 실행할 수 있음

