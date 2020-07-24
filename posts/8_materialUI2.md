---
title: "material ui 알아보기 2"
date: "2020-07-23"
---

[앞선 포스팅](7_materiaUi1)에서는 Material UI가 무엇인지, 어떻게 만들어지고 변화되어 왔는지, 그리고 다른 React UI component library에는 무엇이 있는지를 간단히 살펴보았다. 이번 포스팅은 Material UI를 사용하는 방법을 다룬다.

개발 공부를 시작하면서 놀랐던 점이, 무료로 제공되는 라이브러리가 정말 많고 또 설명이 잘 되어 있다는 점이었다. 그리고 궁금한 점을 검색하면 왠만하면 stackoverflow에 다 있었고, 또 오픈소스 라이브러리의 github discussion에서 그 오픈소스의 개발자들이 직접 작성한 답변을 볼 수도 있다. 나와 같은 문제를 풀기 위해 여러 사람이 각자 답을 내고 어떤 게 더 좋은 방법인지 논의를 해두었으니 나는 보고 따라하기만 하면 된다.

아무튼, Material UI를 어떻게 사용하면 되는지도 Material UI 홈페이지와 github에 친절하게 다 나와있다. 따라하기만 하면 된다.

Material UI를 설치하는 방법은 [이 페이지](https://material-ui.com/getting-started/installation/)에 있다. 터미널에 `npm install @material-ui/core`이나 `yarn add @material-ui/core`을 입력해 패키지를 설치하면 된다. 끝!

기본적인 사용방법은 [이 페이지](https://material-ui.com/getting-started/usage/)에 자세히 나와있다. 간단하게 설명하자면, 1) 패키지에서 사용하려는 component를 import한다. 2) HTML tag를 사용하듯이 사용한다. 3) Components 문서와 Component API 문서를 참고해 tag 안에서 내가 원하는대로 Props나 CSS를 수정한다. 끝!

[Material UI 홈페이지에 예시로 나와 있는 코드](https://material-ui.com/getting-started/usage/#quick-start)를 보면 더 잘 이해가 될 것이다.

```javascript
import React from "react";
import ReactDOM from "react-dom";
import Button from "@material-ui/core/Button";

function App() {
  return (
    <Button variant="contained" color="primary">
      Hello World
    </Button>
  );
}

ReactDOM.render(<App />, document.querySelector("#app"));
```

위의 방법대로 코딩을 하면서 Props와 CSS를 만지다보면 코드가 아주 지저분하게 된다. 하나 하나의 component에서 일일히 props와 css를 지정하기보다는 component 자체를 customize하면 코드가 훨씬 간결해질 수 있다. Material UI의 [Customizing components 페이지](https://material-ui.com/customization/components/)는 Usecase가 narrow한 것에서부터 더 broad한 순서로 5가지 방법으로 구분하여 설명하고 있다.

하지만 나는 아직 깊숙한 내용을 잘 이해를 못해서 그런지 큰 차이가 없는 것 같아서, 이 글에서는 사용하는 패키지에 따라 2 가지로 나누어 설명하려고 한다.

첫번째는 `@material-ui/styles` 또는 `@material-ui/core/styles`를 사용하는 방법이다.

이 패키지는 makeStyles, styled, withStyles의 세 가지 API를 제공한다. 각각의 사용법은 [이 문서](https://material-ui.com/styles/basics/#higher-order-component-api)에 자세히 나와 있다. 세 가지 API의 논리는 동일하다고 하는데, 각각에 대해 간단히 설명하면 아래와 같다.

makeStyles
