---
title: "React 홈페이지 훑어보기"
date: "2020-07-30"
---

[지난 포스팅](8_materialUI2)과 [그 이전 포스팅](7_materialUI1)은 지금 진행 중인 프로젝트에서 사용하고 있는 Material UI를 살펴보았다. theme를 적용하고 스타일을 다듬으면서 Material UI의 소스코드를 살펴볼 일이 많았는데, 보다보니 역시 Javascript와 React 문법의 기초가 부족하다는 생각이 들었다. 앞으로 진행 중인 프로젝트에 필요한 공부를 하면서 틈틈히 Javascript와 React의 기본적인 문서들도 공부하여 포스팅하려고 한다.

오늘은 [React 홈페이지 첫 화면](https://reactjs.org/)의 내용을 읽어보았다.

가장 먼저 눈에 들어오는 header에는 다음과 같이 써있다.

> **A JavaScript library for building user interfaces**

React는 UI를 만들기 위한 Javascript 라이브러리라고 한다. 그렇다면 React는 무엇이 특별한가? Header의 바로 아래 이어지는 section에 React의 세 가지 특징이 써있다.

- **Declarative**
- **Component-Based**
- **Learn Once, Write Anywhere**

각각의 특징이 어떤 의미가 있는지, 그 내용을 살펴보자. React 홈페이지에 공식 한글 문서가 있지만 공부하는 차원에서 직접 번역했다. 혹시 잘못 번역한 내용이 있을 수 있다.

**Declarative**

> React makes it painless to create interactive UIs. Design simple views for each state in your application, and React will efficiently update and render just the right components when your data changes.
> Declarative views make your code more predictable and easier to debug.

> React는 interavtive한 UI를 쉽게 만들게 해준다. 사용자가 각각의 state에서 application이 가져야 하는 view를 설계해두면, 데이터가 변화할 때마다 React가 필요한 component들을 효율적으로 업데이트하고 그린다. Declarative view는 코드가 예측 가능하고 디버깅이 쉬워지도록 만든다.

상호의존하는 부분들로 이루어진 전체의 움직임을 상상하는 일은 매우 어렵다. React는 사용자들이 state를 기준으로, 전체 동작을 상상할 수 있는 규모의 component들로 쪼개어 Javascript 코드를 구성하도록 한다. 아마도 state를 잘 사용하려고 하면 component를 쪼갤 수 밖에 없을 것 같다. 그러다보면 예측 가능하고 디버깅이 쉬운 코드가 될 것 같다.

**Component-Based**

> Build encapsulated components that manage their own state, then compose them to make complex UIs.
> Since component logic is written in JavaScript instead of templates, you can easily pass rich data through your app and keep state out of the DOM.

> 자신의 state를 자신이 관리하는 캡슐화된 component들을 만들고, 그 component들을 조합(compose)해 복잡한 UI를 만든다. Component logic은 template이 아니고 Javascript 코드이기 때문에, app에서 손쉽게 다양한 데이터를 사용할 수 있고, DOM에 의존하지 않고 state를 관리할 수 있다.

React의 component는 함수다. Input과 output이 있고, 그 외에는 신경쓰지 않아도 되도록 만들어져 있다. 각각의 componenet들을 움직이는 state는 component 안에 있고, input으로 받은 데이터를 사용하거나, output으로 데이터를 내보낼 수는 있지만 그렇지 않고는 외부에 의존하거나 영향을 끼치지 않는다. 'DOM에 의존하지 않고 state를 관리할 수 있다'는 말은, 아마도 DOM에 직접적으로 state를 보내거나, DOM에서 무엇을 받아와 직접적으로 state로 사용하지 않는다는 뜻이 아닐까 싶다. 사실 이 부분은 잘 모르겠다.

React의 component는 template가 아니고, 오히려 방법론이라고 할 수 있을 것 같다. 이렇게 Javascript 코드를 쓰면 declarative한 부분들로 구성된 UI를 만들 수 있다는 방법론 같은 게 아닐까? 아무튼, template이 아니니까 내가 필요한대로 만들어서 쓰면 된다.

**Learn Once, Write Anywhere**

> We don’t make assumptions about the rest of your technology stack, so you can develop new features in React without rewriting existing code.
> React can also render on the server using Node and power mobile apps using React Native.

> React는 React로 작성된 이외의 부분들이 어떤 기술 스택으로 구성되어 있는지 알 필요가 없다. 따라서 기존 코드를 재작성하지 않고도 새로 만드는 부분에서 React를 사용할 수 있다. React는 Node를 이용한 서버에서의 render를 지원하고, React Native로 모바일 앱도 지원한다.

React는 고유한 state를 가지는 declarative한 component들로 구성되어 있기 때문에, 다른 부분들과 독립적으로 사용할 수 있는 것 같다... 아직 여러 기술을 사용해 볼 일은 없지만, input과 output만 제대로 주어지면 아마도 문제가 없지 않을까?

그리고 이어지는 section은 **고유한 state를 가지는 declarative한 component**라는 React의 중요한 특성을 좀더 이해하기 쉽도록 React component의 특징을 간단한 예제와 함께 소개하고 있다.

- **A Simple Component**: React component는 input data를 받아서 결과물을 그려내는 render() method를 가진다. Input data는 this.props으로 주어진다. `<div>Hello {this.props.name}</div>`을 return하여 'Hello Taylor'와 같은 형태로 메세지를 보여주는 component를 예시로 들었다.

- **A Stateful Component**: React component는 input data 외에도 자체적인 내부 state 데이터를 가진다. State data는 this.state로 접근한다. seconds라는 props를 가지는 state와, 1초 간격으로 seconds에 1을 더하는 tick()이라는 메소드를 이용해 React.DOM에 이 component가 그려진 시점으로부터 현재까지의 시간을 초 단위로 보여주는 component를 예시로 들었다.

- **An Application**: Props와 state를 사용해 작은 앱을 만들 수 있다. State에 현 시점의 할 일 목록과 입력된 텍스트를 가지는 TODO app을 예시로 들었다.

- **A Component Using External Plugins**: React는 다른 라이브러리나 프레임워크들과 함께 사용될 수 있다. [Remarkable](https://github.com/jonschlinkert/remarkable)이라는 외부 라이브러리를 이용해 `<textarea>`에 입력된 값을 실시간으로 변환시켜 보여주는 예시를 들었다.
