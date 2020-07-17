---
title: '두 번째 과제 - Javascript eventloop 설명하기'
date: '2020-07-09'
---

두 번째 과제는 Javascript eventloop를 설명하는 것이다.
사실 원래 어제 과제였는데 이해를 잘 못해서 오늘 또 하게 되었다. 이번에도 정확히 이해한 것인지는 모르겠지만 일단 설명해보겠다.

Javascript는 프로그래밍 언어이다. 그래서 그 자체로는 무엇을 할 수 없고, Javascript로 쓴 코드를 어딘가에서 실행을 해줘야 무엇을 할 수 있다. 웹브라우저도 Javascript code를 실행할 수 있는 소프트웨어이다. 대표적인 웹브라우저 크롬은 Javascript engine으로 크롬 V8을 가지고 있다.

Javascript는 프로그래밍 언어인데, single thread runtime을 가지는 프로그래밍 언어이다. 이 말은 Javascript로 쓴 코드를 실행시키면 1개의 stack을 사용한다는 뜻이다. stack은 해야 할 일들을 적어놓은 메모장이라고 할 수 있다. 카페 알바할 때 쓰는 주문서와 비슷하다. 하지만 stack은 카페 주문서와 반대로 나중에 들어온 주문을 먼저 처리한다. Javascript code를 실행시키면 코드를 읽어나가면서 서로 관련된 코드들을 stack에 쌓았다가 다 모이면 마지막에 들어온 일을 먼저 하면서 stack을 비워나가고, 다 비워지면 다음 코드 덩어리들을 살펴본다. 그렇기 때문에 Javascript code는 sequential하게 실행된다고 할 수 있다.

하지만 Javascript는 concurrent하게 실행 가능한 언어이기도 하다. 이 말은 서로의 결과물에 영향을 끼치지 않는 코드 덩어리들을 따로 따로 실행시킬 수 있다는 뜻이다. 필요하다면 어떤 코드 덩어리의 작업이 완료되지 않은 상태에서 다음 코드 덩어리를 실행시킬 수 있다. 이런 특성을 이용하면 javascript code를 asynchronous하게 실행되게 할 수 있다.

Javascript code가 어떻게 asynchronouse하게 실행 가능한지 살펴보기 전에, 모든 javascript code를 synchronous하게 작성하면 어떤 문제가 있는지 생각해보자.

Javascript code가 synchronous하게 되어 있다면 -즉 하나의 작업이 완료되어야지만 다음 작업을 할 수 있게 되어있다면- 어떤 작업에 문제가 생겨서 답이 오지 않거나, 아니면 답이 매우 느리게 오면 사용자는 아무것도 못하고 마냥 기다려야 한다. 이때 만약 우선 처리가 가능한 부분들을 먼저 처리하고 나머지는 응답이 오는대로 무언가를 하도록 분리시키면 사용자는 웹 브라우저가 더 빨리 반응하는 것으로 느낄 것이다. 또 어떤 작업들은 먼저 화면이 그려진 후에 사용자가 무엇을 해야지만 실행 가능한 것들도 있다. 그런 일들은 기다리게 하는 것 자체가 말이 안된다.

한편 웹 브라우저가 나중에 응답을 주겠다는 약속을 해주면, javascript engine은 응답을 기다리지 않고 다음 코드 덩어리를 실행시킬 수 있다. 이런 방법으로 javascript code를 asynchronous하게 구성할 수 있다. 다행히 Timeout, AJAX와 같은 Web API가 마련되어 있어 손쉽게 웹 브라우저에게 응답을 달라고 약속을 받을 수가 있다. 예를 들어 Timeout은 특정 시간이 지나면 응답을 주기로 되어있다. 이 응답을 Callback이라고 한다.

한편 Web API는 javascript engine에 바로 callback을 보내어 코드 덩어리를 실행시키지 않는다. 그게 불가능한건지, 그렇게하지 않는 것이 더 좋은 설계여서 그런지는 잘 모르겠다. 아무튼, API는 응답을 보내야 할 상황이 오면 callback을 queue에 쌓아둔다. Queue는 stack과 반대로 먼저 받은 주문을 먼저 처리하도록 하는 데이터 타입이다. 웹 브라우저에는 우선순위가 다른 여러 queue들이 있어, javascript engine에서 처리해야 할 일들의 우선순위를 정리하는 역할도 한다고 한다.

그리고 마침내.. 이번 과제에서 답해야 하는 'eventloop'가 등장한다.

Eventloop는 callback queue와 javasrvipt engine의 stack 사이에 존재하는 loop이다. Loop는 [어떤 조건을 만족하게 될 때까지 반복하여 수행되는 명령](https://www.thoughtco.com/definition-of-loop-958105)이다. Eventloop가 가지는 조건은 'queue가 메세지를 기다린다'는 것이다. 그리고 eventloop가 반복하여 수행하는 명령은 'callback queue가 자신의 첫 번째 메세지(다음 메세지)를 javascript engine의 stack에 보내게 하는 것'이다. 이때 javascript engine의 stack이 차 있으면 메세지를 보낼 수 없으므로, 다시 queue를 확인하는 상태로 돌아간다. 앞서 브라우저에는 서로 우선순위가 다른 여러 queue들이 있다고 했다. Eventloop는 우선순위가 높은 queue에 메세지가 있는지를 우선 확인한다. Eventloop는 이렇게 돌다가 javascript engine의 stack이 비어 있을 때 stack을 살피게 되면 queue의 callback을 javascript engine에 보낸다. 그러면 그 callback 함수를 받아 실행되는 코드 덩어리가 실행되게 된다! 

[MDN 문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)에서는 이 일을 다음의 while 문으로 표현헸다.
```
while (queue.waitForMessage()) {
  queue.processNextMessage()
}
```
그러니까 eventloop는 single threaded concurrent language인 javascript가 정해진 우선순위에 따라 브라우저에서 발생하는 여러 이벤트들에 대한 반응을 할 수 있도록 필요한 고리인 것이다!

우와 길었다.

참고 문서:
[Philip Roberts: What the heck is the event loop anyway?](https://www.youtube.com/watch?time_continue=92&v=8aGhZQkoFbQ&feature=emb_logo)
[Concurrency model and the event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
[Event Loop(이벤트 루프) - thms200.log](https://velog.io/@thms200/Event-Loop-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84)
