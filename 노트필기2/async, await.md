Promise 어려워서 싫으면 async/await을사용합시다
==

### <span style="color:#348b58">Promise 복습</span>
- 순차적실행을 위해 콜백함수 대신 쓸 수 있는 코딩 패턴
- 프로미스라는 object는 성공/실패를 판정할 수 있는 기계
```js {.line-numbers}
var 프로미스 = new Promise(function(성공, 실패){ 
  setTimeout(function(){
    성공(); // 1초 후에 성공을 판정해주는 프로미스
  }, 1000);
});

프로미스.then(function() { 
  console.log('성공했어요!');
}).catch(function() {
  console.log('실패했어요!');
})
```

### 1. Promise를 자동으로 뱉어주는 async 키워드
- 콜백함수 써서
  ```js {.line-numbers}
  function 더하기(콜백) {
    1 + 1
    // 연산이 끝나면 특정 코드를 실행하고 싶음
    콜백(); // 콜백함수 씀
  }

  더하기(함수);
  // 더하기 실행한 후 함수 실행해주세요
  ```
- Promise 써서
  ```js {.line-numbers}
  var 더하기 = new Promise(function(성공, 실패) {
    let 결과 = 1 + 1;
    성공(결과);
  })

  더하기.then(function(결과) {
    console.log(결과);
  })
  ```
- async 써서
  - async는 함수 앞에서만 쓸 수 있는 키워드
  - 함수 선언 앞, arrow function 앞에 쓸 수 있음
  - `async`를 function 앞에 붙이면 함수가 Promise 역할 가능
  ```js {.line-numbers}
  async function 더하기() {
    1 + 1
  }

  // 위에 함수가 실행 후 항상 Promise 객체가 그 자리에 남음
  // Promise 오브젝트가 남기 때문에 .then() 쓸 수 있음
  더하기().then(function(){
    console.log('성공이에요');
  })
  ```
- `async`만 붙이면 `new Promise(~~)` 디자인 안해도 `.then()` 사용가능
```js {.line-numbers}
async function 더하기() {
  return 1 + 1; // return 키워드 붙이면 결과를 출력해줄 수 있음 
}

더하기().then(function(결과){
  console.log('성공이에요' + 결과);
})
```
#### <span style="color:#f84d3a">async 단점</span>
- 성공만 가능
- 파라미터가 따로 들어가는게 없어서 실패를 보낼 수가 없음
- 강제로 실패를 보내줄 수 있음
  ```js {.line-numbers}
  async function 더하기() {
    return Promise.reject('실패임'); // .then() 실행이 안됨 
  }

  더하기().then(function(결과){
    console.log('성공이에요' + 결과); // 실행안됨
  })
  ```

### 2. await 키워드 
- `async function` 안에서 쓰는 `await`
- Promise를 정확히 잘 쓰고 싶어서 
함수 안에서 Promise 쓸려고 함
  ```js {.line-numbers}
  async function 더하기() {
    var 프로미스 = new Promise(function(성공, 실패) {
      var 힘든연산 = 1 + 1;
      성공();
    })

    프로미스.then(function(){
      // 프로미스 내의 연산이 성공하면 이 코드를 실행시켜주세요~
      console.log('성공이에요');
    });
  }
  
  더하기(); // 성공이에요
  ```
- `async function` 안에서 쓰는 `await`
  `then` 대신에 사용가능
  ```js {.line-numbers}
  async function 더하기() {
    var 프로미스 = new Promise(function(성공, 실패) {
      var 힘든연산 = 1 + 1;
      성공(100);
    })

    // await = 기다려주세요!
    var 결과 = await 프로미스; // 프로미스 해결까지 기다려주세요
    console.log(결과); // 100
  }
  
  더하기(); 
  ```
- 만약 프로미스 실패하면?
  ```js {.line-numbers}
  async function 더하기() {
    var 프로미스 = new Promise(function(성공, 실패) {
      var 힘든연산 = 1 + 1;
      실패(100);
    })

    var 결과 = await 프로미스; // <- 여기서 에러로 인해 멈춤
    console.log(결과); // 에러출력

    console.log(111); // 에러로 인해 출력 안됨
  }
  
  더하기(); 
  ```
  - `await`은 프로미스 실패시 에러나고 멈춤
- 위와 같은 상황 방지하려면 try-catch문 사용
  ```js {.line-numbers}
  async function 더하기() {
    var 프로미스 = new Promise(function(성공, 실패) {
      var 힘든연산 = 1 + 1;
      실패(100);
    })

    try { 이걸해보고에러가나면 } catch { 이걸실행해주세요 }
    var 결과 = await 프로미스;
    console.log(결과); 
  }
  
  더하기(); 
  ```
  ```js {.line-numbers}
  async function 더하기() {
    var 프로미스 = new Promise(function(성공, 실패) {
      var 힘든연산 = 1 + 1;
      실패(힘든연산);
    })

    try { // 성공하면
      var 결과 = await 프로미스;
      console.log(결과); 
    } catch { // 실패하면
      console.log('프로미스 연산이 잘안되었군요');
    }
  }
  
  더하기(); 
  ```
  - 프로미스 연산결과는 변수에 저장가능

### <span style="color:#348b58">예제 문제</span>
1. 예제 : `<button>`을 누르면 성공하는 Promise 만들기
- HTML 페이지 내에 버튼 아무거나 하나 만들고
그걸 클릭하면 성공하는 Promise를 만들고 싶습니다. 
성공하면 콘솔창에 '성공했어요'를 출력하고요.
어떻게 코드를 짜면 될까요? 
(async, await이 필요하면 써봅시다)

- 내가 푼 것 : (작동 잘함)
  ```html
  <button id="button" type="button">버튼</button>
  ```
  ```js {.line-numbers}
  const BUTTON = document.querySelector("#button");
  BUTTON.addEventListener('click', pushButton)

  async function pushButton() {
    var 프로미스 = new Promise(function(성공, 실패) {
      성공('성공했어요');
    })
    
    var 결과 = await 프로미스;
    console.log(결과);
  }
  ```

- 정답+해설 :
  ```js {.line-numbers}
  async function pushButton() {
    const BUTTON = document.querySelector("#button");
    var 프로미스 = new Promise(function(성공, 실패) {
      BUTTON.addEventListener('click', function() {
        성공('성공했어요');
      })
    })
    
    // 프로미스.then(); 도 가능
    var 결과 = await 프로미스;
    console.log(결과);
  }

  pushButton();
  ```
  - 순차적으로 많은 것을 실행할 때 유용