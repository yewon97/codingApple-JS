Stack, Queue를 이용한 웹브라우저 동작원리
==

> 웹 브라우저 동작원리
> 왜 알아야 하지?
> 자바스크립트는 크롬이 실행해주기 때문이다.

### 1. JS 동작 순서
- `setTimeout()`
  ```js {.line-numbers}
  console.log(1+1);
  setTimeout(function(){}, 1000); // 1초 쉬고
  console.log(2+2);
  ```
  - 1초 안쉬고, 바로 출력됨 
  ```js {.line-numbers}
  console.log(1+1);
  setTimeout(function(){ console.log(2+2) }, 1000);
  ```
  - 이런식으로 적어줘야지 1초 쉬고, 4를 출력해줌
  ```js {.line-numbers}
  console.log(1+1);
  setTimeout(function(){ console.log(2+2) }, 1000);
  console.log(3+3);
  ```
  - 출력 순서 : 2 -> 6 -> 1초 쉬고 -> 4
- 코드 위에 적든 밑에 적든 빠른 것부터 먼저 실행시켜줌

### 2. JS 동작 원리
<img src="../images/3.jpg" width="70%">

- 코드를 실행할 때 `i`라는 변수를 만나면
- `i`라는 변수를 찾음
- Heap이라는 공간에서 변수들이 저장되어 있는데
- Heap이라는 공간에서 변수를 가져다 씀

<img src="../images/2.jpg" width="70%">

- Stack이라는 공간이 제일 중요하다
> stack이란? 
> 그냥 다 집어넣고 맨 윗줄부터 하나하나 실행시키는 공간
- Stack은 1개밖에 없어서 한번에 코드 1줄밖에 실행 못함
- 그래서 JS는 보통 `single threaded` language라고 함

<img src="../images/4.jpg" width="70%">

- `setTimeout()`은 바로 실행할 수 없는 코드임
- Stack에 넣어서 실행하지 않음 (왜냐하면 1초동안 기다릴 수 없으니까)

<img src="../images/5.jpg" width="70%">

- `setTimeout()`같은 것들은 잠깐 대기실로 치워놓는다.
(처리가 오래걸리는 것들은 다 대기실로 이동)
- 그리고 Stack에 있는 것들 먼저 실행한다.
> 대기실 보내는 코드들 :
> Ajax 요청 코드
> 이벤트리스너
> setTimeout 등
> 이런 코드는 처리하기까지 시간이 오래걸림 
ajax 요청은 서버에서 응답을 받기까지 시간이 오래걸리고
버튼 이벤트리스너는 사용자가 버튼을 누르기까지 시간이 오래걸림
그래서 그런건 Stack에 쌓아서 실행하지 않고 잠깐 보류해놨다가 완료가 되는 시점에 Stack으로 보냅니다.

<img src="../images/6.jpg" width="70%">

- 1초가 다된 `setTimeout()` 이제 실행해야하는데 바로 Stack으로 가는 것이 아니라
- Queue라는 대기실을 거쳐서 Stack으로 이동하게 됨
- Queue는 처리가 완료된 오래걸리는 코드들을 줄을 세워놈
- Queue에서 Stack으로 하나씩 올려보내줌 (참고로 Queue는 들어온 순서대로 차례차례 Stack으로 옮겨줌)
> 왜 Queue라는 대기실을 만들었지?
> Stack이라는 공간은 굉장히 바쁜 공간이기 때문에
> Queue에서 Stack으로 올려보내는 조건이 Stack이 텅 비어있을 때만 올려보냄

#### Q. 문제
<img src="../images/7.jpg" width="70%">

- `setTimeout()`에 0초를 줌
어떻게 동작할까?
- 답 : 2 -> 6 -> 4 순서로 출력이 된다.
- 해설 : `setTimeout()`는 무조건 대기실로 이동하기 때문에

### <span style="color:#f84d3a">for 반복문 쓸 때</span>
- 가끔 for 반복문 천만번 돌리면 오래걸림
- 이런거 JS로 시키면 안됨!!!
- 이런 10초걸리는 연산을 JS에 시키게 되면

<img src="../images/8.jpg" width="70%">

- 10초동안 버튼클릭, ajax요청, setTimeout 타이머 다 작동 안된다.
- 버튼 누르면 모달창 띄어주는 그런 것들 버튼 눌러도 모달창이 안뜬다는 말이다.

<img src="../images/9.jpg" width="70%">

- Ajax 요청 코드, 이벤트리스너, setTimeout 등은 무조건 대기실 -> Queue -> Stack 이런 순서대로 실행된다.
- 근데 Queue에서 Stack으로 이동되는 조건이 Stack이 텅텅~ 비었을 때이다.
- 10초 동안 Stack이 비지 않으니까 Stack으로 이동할 수 없는 것!
- browser freezing의 원인이 되기도함

### <span style="color:#348b58">오늘의 교훈</span>
1. Stack을 바쁘게 하지말아라!
2. Queue를 바쁘게 하지말라라!
(버튼 하나에 이벤트를 100개씩 달아놓으면 바빠짐)
3. 자바스크립트는 동기적 vs 비동기적?
    - JS는 원래 동기적으로 처리됨
    (한번에 한줄 순서대로 실행 <- Stack은 하나니까)  
    - JS는 가끔 비동기적인 처리도 가능
    (setTimeout, 이벤트리스너, ajax 한수쓰면 된다.)

---

### <span style="color:#FDAC53">- 문제 - </span>
- 반복문을 100억번 돌리긴 해야함... 어떻게하지?
```js {.line-numbers}
for (let i = 0; i < 1e10; i++) {
  i++;
}
```
- (참고로 1e10은 0을 10개 붙이라는 뜻입니다)
이렇게 쓰면 반복문이 100억번 돌아가는데 아무리 CPU가 좋아도 시간이 약간 오래걸릴겁니다.
10초가 걸린다고 하면 10초동안 사용자가 버튼클릭 이런게 전혀 안먹는다는 소리입니다.
이런 작업을 꼭 해야한다면 가장 간단한 트릭은 

1. `setTimeout`을 이용
    - setTimeout()을 이용해서 0초마다 0~1억 반복, 1억~2억 반복, 2억~3억 반복... 
    이렇게 코드를 실행하면 보다 쾌적하게 작업을 실행할 수 있습니다. 
    0초마다 Queue로 보내기 때문에 그 사이사이에 사용자의 이벤트리스너 이런 코드를 실행가능하니까요.
    (setTimeout 타이머를 0초로 설정해도 실은 4ms로 동작합니다 설정가능한 최소시간이 4ms 임)

2. Web worker를 이용
    - 다른 자바스크립트 파일을 이용해서 그 파일에서 힘든 연산을 시키고 그게 완료가 되면 값을 가져오라고 명령이 가능합니다.
    이미 만들어진 Worker라는 클래스를 사용하면 됩니다
    ```js {.line-numbers}
    // 📄 메인 js 파일
    var myWorker = new Worker('worker.js'); 

    w.onmessage = function(e){
      console.log(e.data) //이러면 1 나올듯
    };
    ```
    ```js {.line-numbers}
    // 📄 worker.js 파일
    var i = 0;
    postMessage(i + 1); //postMessage라는 특별한 함수가 있음
    ```
    - 이런 식으로 셋팅해놓으면 worker.js에서 작업이 완료될 시 postMessage() 이렇게 실행하면
    다른 파일로 완료된 결과값을 전달해줄 수 있습니다. 
    이러면 Stack이 바빠지지 않습니다. 

