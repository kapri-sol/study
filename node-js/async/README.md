# 비동기
Javascript를 배우면 제일 처음 마주하게 되는 벽이 하나 있다. 바로 `비동기`에 대한 개념이다.
동기식 프로그래밍에 익숙한 개발자라면 본인의 생각과 전혀 다르게 동작하는 코드를 보며 

다음의 Javascript 예제 코드를 확인해보자.

```javascript
console.log('A');

setTimeout(() => {
    console.log('B');
}, 1000);

console.log('C');
```

- console.log 는 출력함수이다
- setTimeout은 두번째 인자로 넣은 시간(ms)이 지나면 첫번째 인자로 넣은 함수를 실행시켜주는 함수이다.

위의 코드는 어떻게 출력될까?

[동기]
```
A
// 1초...
B 
C
```

일반적으로 이런 출력을 예상할 것이다.

[비동기]
```
A
C
// 1초...
B
```

하지만 Javascript 코드는 실제로 이렇게 출력된다.

`도대체 왜 Javascript 코드는 이렇게 실행될까?`


## 싱글 스레드

Javascript는 웹 브라우저에서 싱글스레드로 동작한다. 왜 싱글 스레드 방식일지 검색해 보면 이런 결과가 나온다.

```성
- 자바스크립트는 웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기로 결정하고 만들어진 언어가 자바스크립트 이다.
- 웹사이트를 구현하던 개발자들에게 자바라는 언어는 다소 무겁고 어려운 언어였기 때문에, 브랜던 아이크라는 사람을 스카우트하여 ‘자바스크립트’가 탄생되었다.
- 왜 무겁고 어려운 언어라 생각했을까? → 멀티 스레드 모델은 프로그래밍 난이도가 높다.
- 멀티 스레드로 구현된 서비스에서는 이 동시성 문제에 대해 많이 신경쓴다고 한다. 자바스크립트는 멀티 스레드 환경에서 발생할 수 있는 복잡한 시나리오를 신경쓸 필요 없다.

실제로 구글의 chrome 브라우저도 기존 웹 페이지에서 엄청난 동시성 문제를 일으킬 수 있다는 이유로 단일 웹 사이트 페이지의 자바스크립트 코드가 동시에 실행되는 것을 허용하지 않는다.
```

싱글 스레드이기 때문에 동기 방식으로 동작한다면, 우리가 웹 페이지에서 어떤 버튼을 눌러서 서버에 요청을 했을 때, 응답이 올 때까지 웹 페이지는 멈춰 있게 된다. 한 번에 하나의 동작만 수행할 수 있게된다. 그렇게 되면 웹 사이트의 사용성은 매우 떨어지게된다. 그렇기 때문에 필수적으로 비동기 방식의 동작이 필요했던 것이다.

## 비동기 함수

`그런데 엄밀히 말하자면 Javascript는 비동기적으로 실행되지 않는다.`

비동기적으로 실행되는 것은 웹 브라우저나 NodeJS의 API를 실행하는 특정 함수이다. setTimeout, ajax 등이 있으며 Promise를 이용해서 직접 만들 수도 있다.


[//]: # (## Blocking I/O vs Non Blocking I/O)

[//]: # (CPU는 매우 빠르게 주어진 작업을 처리한다. 하지만 파일과 네트워크와 관련된 I/O 작업은 상대적으로 많은 시간이 걸린다. 이 작업들은 외부에 요청을 하기때문에 시간이 오래 걸릴 수 밖에 없다.)

[//]: # (그런데 이런 I/O 작업은 시간은 많이 걸리는데 비해 정작 CPU는 아무일도 하지않는다. I/O Device가 대신 작업을 수행하기 때문이다. 그 긴 시간동안 CPU를 가만히 놔두는 것은 굉장히 비효율적이라는 생각이 든다.)


## 참고
- [자바스크립트는 왜 싱글 쓰레드일까?](https://chanyeong.com/blog/post/44)
- [자바스크립트는 왜 싱글 스레드를 선택했을까? 프로세스, 스레드, 비동기, 동기, 자바스크립트 엔진, 이벤트루프](https://miracleground.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%EC%8B%B1%EA%B8%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EB%A5%BC-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%8A%A4%EB%A0%88%EB%93%9C-%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%8F%99%EA%B8%B0-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%97%94%EC%A7%84-%EC%9D%B4%EB%B2%A4%ED%8A%B8%EB%A3%A8%ED%94%84)
- [자바스크립트는 싱글 스레드인데 왜 비동기가 가능할까?](https://stitchcoding.tistory.com/44)
- [백엔드 개발자들이 알아야할 동시성 2 — 블로킹과 논블로킹, 동기와 비동기](https://choi-geonu.medium.com/백엔드-개발자들이-알아야할-동시성-2-블로킹과-논블로킹-동기와-비동기-e11b3d01fdf8)
- [블럭,논블럭,동기,비동기 이야기](https://hamait.tistory.com/930)
- [Blocking I/O와 Non-blocking I/O](https://youtu.be/XNGfl3sfErc)
- [blocking I/O, non-blocking I/O에 대하여 (sync, async와의 차이)](https://etloveguitar.tistory.com/140)
- [[Node.js] 비동기 개념에 익숙해지기](https://elvanov.com/2682)
- [로우 레벨로 살펴보는 Node.js 이벤트 루프](https://evan-moon.github.io/2019/08/01/nodejs-event-loop-workflow/)
- [자바스크립트와 이벤트 루프](https://meetup.nhncloud.com/posts/89)
- [Node.js 동작원리 (Single thread, Event-driven, Non-Blocking I/O, Event loop)](https://medium.com/@vdongbin/node-js-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC-single-thread-event-driven-non-blocking-i-o-event-loop-ce97e58a8e21)
- [Node.js 비동기 처리 실행 과정](https://keyhyuk-kim.medium.com/node-js-promise%EC%9D%98-%EC%8B%A4%ED%96%89-%EA%B3%BC%EC%A0%95-c48e8c902779)
- [JavaScript 비동기 핵심 Event Loop 정리](https://medium.com/sjk5766/javascript-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%95%B5%EC%8B%AC-event-loop-%EC%A0%95%EB%A6%AC-422eb29231a8)
- [V8 엔진은 어떻게 내 코드를 실행하는 걸까?](https://evan-moon.github.io/2019/06/28/v8-analysis/)