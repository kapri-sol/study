# 비동기
Javascript를 배우면 제일 처음 마주하게 되는 벽이 하나 있다. 바로 `비동기`에 대한 개념이다.
동기식 프로그래밍에 익숙한 개발자라면, 예상과 다르게 동작하는 코드를 보며 혼란을 느끼게 될 것이다.

어떤 부분이 다르게 동작하는지 다음의 Javascript 예제 코드를 보며 확인해보자.

```javascript
console.log('A');

setTimeout(function (){
    console.log('B');
}, 1000);

console.log('C');
```

- console.log 는 출력함수이다
- setTimeout은 두번째 인자로 넣은 시간(ms)이 지나면 첫번째 인자로 넣은 함수를 실행시켜주는 함수이다.

위의 코드는 어떻게 출력될까?

```
A
// 1초...
B 
C
```

동기에 익숙하다면 이런 출력을 예상할 것이다.

```
A
C
// 1초...
B
```

하지만 실제로 이렇게 출력된다.

`왜 Javascript 코드는 이렇게 실행될까?`

## 싱글 스레드

우리가 짠 코드는 일반적으로 프로세스 단위로 실행된다. 프로세스는 최소 한 개의 스레드 가지는데 스레드는 프로세스의 실행 흐름의 단위이다. 스레드가 하나라면 프로세스는 한 번에 하나의 작업만 수행할 수 있게 된다. 한 번에 하나의 작업만 수행한다면 수행하는 작업이 완료될때까지는 다른 작업을 받지 못하기때문에 사용성이 떨어지거나 비효율적일 수 있다. 그렇기때문에 많은 프로그램이 실행 흐름을 여러개로 나눈 멀티 스레드 방식으로 만들어진다.

그런데 Javascript 코드는 브라우저에서 싱글스레드로 동작한다. 왜 사용성이 떨어지는 싱글 스레드 방식을 선택했을까?

```
- 자바스크립트는 웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기로 결정하고 만들어진 언어가 자바스크립트 이다.
- 웹사이트를 구현하던 개발자들에게 자바라는 언어는 다소 무겁고 어려운 언어였기 때문에, 브랜던 아이크라는 사람을 스카우트하여 ‘자바스크립트’가 탄생되었다.
- 왜 무겁고 어려운 언어라 생각했을까? → 멀티 스레드 모델은 프로그래밍 난이도가 높다.
- 멀티 스레드로 구현된 서비스에서는 이 동시성 문제에 대해 많이 신경쓴다고 한다. 자바스크립트는 멀티 스레드 환경에서 발생할 수 있는 복잡한 시나리오를 신경쓸 필요 없다.

실제로 구글의 chrome 브라우저도 기존 웹 페이지에서 엄청난 동시성 문제를 일으킬 수 있다는 이유로 단일 웹 사이트 페이지의 자바스크립트 코드가 동시에 실행되는 것을 허용하지 않는다.
```

브라우저 안에서 Javascript 코드는 싱글 스레드로 동작하기 때문에 우리가 웹 페이지에서 버튼을 눌러서 작업을 요청한다면, 해당 작업이 끝날때까지 웹 페이지는 멈춰 있게 될 것이다. 그런데 실제로 우리가 사용하는 브라우저에서는 버튼을 누르며 타이핑을 한다던지 다른 링크를 눌러도 동시에 동작한다. 어떻게 이런 방식이 가능할까?

## 이벤트 루프

어떻게 코드가 동시에 동작할 수 있는지 브라우저의 내부 구조를 살펴보자.

프로세스는 실행될 때, OS로부터 메모리를 할당받게 되는데 `코드`, `데이터`, `힙`, `스택` 영역으로 나누어진다. `코드` 영역에는 개발자의 코드가 기계어 형태로 저장되어있고, `데이터` 영역에는 전역변수, 정적변수 등이 저장되고, `스택` 영역에는 지역변수, 매개변수, 리턴값, 돌아올 주소가 저장되어있다.  

대표적인 Javascript Engine인 V8 Engine 내부에는 `Call Stack`과 `Memory Heap`이 있다.


## 참고
- [자바스크립트는 왜 싱글 쓰레드일까?](https://chanyeong.com/blog/post/44)
- [자바스크립트는 싱글 스레드인데 왜 비동기가 가능할까?](https://stitchcoding.tistory.com/44)
- [What the heck is the event loop anyway? | Philip Roberts | JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
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
- [모든 자바스크립트 개발자가 알아야 하는 33가지 개념](https://github.com/yjs03057/33-js-concepts)
- [[JavaScript] 런타임 작동 방식, 비동기와 이벤트 루프](https://hanamon.kr/javascript-%EB%9F%B0%ED%83%80%EC%9E%84-%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84/)
- [Node.js 이벤트 루프(Event Loop) 샅샅이 분석하기](https://www.korecmblog.com/node-js-event-loop/)
- [NodeJS Event Loop파헤치기](https://medium.com/zigbang/nodejs-event-loop%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-16e9290f2b30)
- [브라우저와 Nodejs의 이벤트 루프는 무엇이 다를까](https://yceffort.kr/2021/08/browser-nodejs-event-loop)
- [Node.js 이벤트 루프(Event-Loop)](https://velog.io/@dosomething/Node.js-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84Event-Loop)
- [nodejs의 내부 동작 원리 (libuv, 이벤트루프, 워커쓰레드, 비동기)](https://sjh836.tistory.com/149)
- [자바스크립트의 동작원리: 엔진, 런타임, 호출 스택](https://joshua1988.github.io/web-development/translation/javascript/how-js-works-inside-engine/)