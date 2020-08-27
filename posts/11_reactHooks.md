---
title: "React의 Hooks 이해하기"
date: "2020-08-27"
---

이번 포스팅은 먼저 React의 Hooks에 대해 다루려고 한다.

이번 프로젝트를 시작할 때는 React의 Hooks에 대해서 전혀 몰랐다. 그래서 Material UI의 useMediaQuery나 useTheme을 쓰면서도 이게 Hook인지도 몰랐고, 보기 예쁘게 정리하다보니 자연스럽게 component의 상단에 모아놓았기 때문에 오류를 겪지도 않았다. 몰라도 쓸 수 있다는게 stackoverflow와 github example들의 위대한 점인 것 같다(?)

그런데 이번 프로젝트에서는 dialog, alert 등을 사용할 일이 많아 useState를 쓰게 되었고, 또 지역에 맞는 언어를 적용하기 위해 사부님이 만들어주신 useLocalization도 사용해야 했다. 사용하는 Hook이 많다보니 내 나름으로 정리하는 과정에서 조건문 등의 뒤에 위치하는 Hook이 생겼고, 그래서 드디어 이 오류를 만나게 되었다.

**'Rendered fewer hooks than expected'**

처음 이 오류를 만났을 때는 정말 무슨 말인지 짐작조차 가지 않았다. Hooks는 뭔데? 어떻게 기대를 하는건데..? 사부님께 SOS를 보내니, 내가 사용한 function들 중에서 use로 시작하는 것들을 Hooks라고 부르며, component 내에서 맨 위에 모아놓으면 해결될 거라 하셨다. 사부님 말씀대로 하니 오류는 해결이 되었지만, 의문은 더 커졌다. 이게 뭔데 순서가 중요하지? 이 의문에 대한 답은 포스팅 끝에 나온다.

Hooks란 무엇인가? [Introducing Hooks](<[https://reactjs.org/docs/hooks-intro.html](https://reactjs.org/docs/hooks-intro.html)>)

Hooks는 React 16.8(2019년 2월)에서 도입되었다. Hooks는 state와 lifecycle을 function component로 가져오는('hook into') Javascript function을 말한다. Class 없이도(wrapper hell 없이도) stateful logic을 여러 component에서 재사용할 수 있게 하기 위해 도입되었다.

React에서는 Hook 개념의 발표와 함께 useState, useEffect, useContext 라는 기본적인 built-in Hooks (Basic Hooks)와 useReducer, useCallback 등 다른 Hooks API를 제공했다. 이런 Built-in Hooks를 사용해 내가 사용하는 stateful behavior를 custom Hooks로 만들어 여러 component에서 재사용할 수도 있다.

[React Conf 2019의 Hook Proposal](<[https://youtu.be/dpw9EHDh2bM](https://youtu.be/dpw9EHDh2bM)>)에서 [Dan Abramov](<[https://github.com/gaearon](https://github.com/gaearon)>)이 사용한 '전자-원자-물질'의 비유를 내 맘대로 해석해보자면, Hooks는 React Component가 UI를 대하는 방식과 같은 방식으로 function을 대한다고 할 수 있다. (이 영상을 보고 Hooks 문서를 읽으면 더 이해가 쉽기 때문에, 영상을 꼭 보기를 권한다.)

React Component는 UI를 declarative한 부분들로 나누어 생각하게 한다. 각각의 component들은 그 안에서 Javascript 특성대로 procedural하게 움직이지만, 정의된 component들 각각은 declarative하게 사용된다. 그래서 그 안에서 무슨 일이 일어나는지 신경쓰지 않고 pure function 처럼 사용할 수 있다. Dan은 이를 '원자'가 '물질'을 이루는 것과 비슷하다고 표현했다.

Hooks는 이와 비슷하게, state나 lifecycle가 브라우저에서 어떻게 처리되는지를 그 component가 신경쓰지 않아도 되도록 만들어준다. useState의 예를 들면, initial state와 setState만 입력해주면 render 시에 어떻게 처리할 것인지는 Hook 내에사 알아서 처리해준다. 즉, state가 어떻게 생겨야 하는지, 그리고 어떻게 바뀌어야 하는지만 정의해주면 나머지는 다른 곳에서 알아서 해주는 것이다.

이를 통해 Hooks는 우리가 lifecycle이나 stateful logic에 따라 코드를 나누는 것이 아니라, 각 코드가 해야할 일에 따라 코드를 나누도록 한다. 또한 React가 적용되어야 하는 effect나 관리되어야 하는 state를 알아서 적용해주기 때문에, .bind, this.state. 등을 쓰지 않는 깔끔한 코드가 된다.

한편, Hooks는 호출될 때마다 완전히 독립된 state를 가짐으로써 state의 재사용 없이 stateful logic을 재사용할 수 있게 해준다. 따라서 한 component 내에서 같은 Hooks를 여러 번 사용하는 것도 가능하다. [Hooks at a Glance 참고](<[https://reactjs.org/docs/hooks-overview.html](https://reactjs.org/docs/hooks-overview.html)>)

단, 이때 React는 호출된 순서를 이용해 각각의 Hooks의 state를 관리하기 때문에, Hook이 호출되는 순서가 바뀌면 안된다(호출되는 Hook의 수도 당연히 같아야 한다). 그래서 조건문이나 루프 안에 Hooks를 넣으면 오류가 발생하는 것이다. React는 매번 render 시마다 같은 순서로 Hooks가 호출될 것을 기대한다. [Rules of Hooks 참고](<[https://reactjs.org/docs/hooks-rules.html](https://reactjs.org/docs/hooks-rules.html)>) 이것이 내가 만난 오류의 원인이었다!

또한 Hooks는 React Function 안에서만 호출되어야 한다. 일반적인 Javascript function에서는 호출될 수 없다. 이 조건을 만족시키기 위해 component의 상단에서만 Hooks를 호출하도록 하는 것이다.

Dan이 영상에서도 언급한 바와 같이, 이런 관리 방식은 흔하지는 않다고 한다. Component와 Hooks가 그 정신은 비슷하나, 실제 구현에서는 이런 방식의 차이가 아쉬움을 남기는 것 같다.
