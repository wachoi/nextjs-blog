---
title: "material ui 알아보기 2"
date: "2020-07-23"
---

[앞선 포스팅](7_materiaUi1)에서는 Material UI가 무엇인지, 어떻게 만들어지고 변화되어 왔는지, 그리고 다른 React UI component library에는 무엇이 있는지를 간단히 살펴보았다. 이번 포스팅은 Material UI를 사용하는 방법을 다룬다.

개발 공부를 시작하면서 놀랐던 점이, 무료로 제공되는 라이브러리가 정말 많고 또 설명이 잘 되어 있다는 점이었다. 그리고 궁금한 점을 검색하면 왠만하면 stackoverflow에 다 있었고, 또 오픈소스 라이브러리의 github discussion에서 그 오픈소스의 개발자들이 직접 작성한 답변을 볼 수도 있다. 나와 같은 문제를 풀기 위해 여러 사람이 각자 답을 내고 어떤 게 더 좋은 방법인지 논의를 해두었으니 나는 보고 따라하기만 하면 된다.

아무튼, Material UI를 어떻게 사용하면 되는지도 Material UI 홈페이지와 github에 친절하게 다 나와있다. 따라하기만 하면 된다.

Material UI를 설치하는 방법은 [이 페이지](https://material-ui.com/getting-started/installation/)에 있다. 터미널에 `npm install @material-ui/core`이나 `yarn add @material-ui/core`을 입력해 패키지를 설치하면 된다. 끝!

기본적인 사용방법은 [이 페이지](https://material-ui.com/getting-started/usage/)에 자세히 나와있다. 간단하게 설명하자면, 1) 패키지에서 사용하려는 component를 import한다. 2) HTML tag를 사용하듯이 사용한다. 3) 각각의 Components에 대한 문서와 Component API 문서를 참고해 내가 원하는대로 Props나 CSS를 수정한다. 끝!

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

위의 방법대로 코딩을 하면서 Props와 CSS를 만지다보면 코드가 아주 지저분하게 된다. 위의 예시에서처럼 component를 사용할 때마다 props와 css sytle을 입력하기보다는 component 자체를 customize하면 코드가 훨씬 간결해질 수 있다.

Component를 customize하는 방법에 대해서는 Material UI의 [Customizing components 페이지](https://material-ui.com/customization/components/) 문서가 상세히 다루고 있다. Usecase가 narrow한 것에서부터 더 broad한 순서로 5가지 방법으로 구분하여 설명한다. 이 포스팅을 쓰기 시작하면서 원래는 이 5 가지 방법에 대해 Material UI github에서 코드를 자세히 뜯어보며 설명해보려고 했는데, 아직 내공이 부족해서 자세히 이해하지는 못할 것 같다. 포스팅이 자꾸 밀리는 것 같아 간단히만 설명하고 넘어가려고 한다.

Component를 customize 하는 방법을 다시 두 가지로 간단하게 분류하자면, **theme**를 사용해 여러 component들의 색상, 폰트 등을 한꺼번에 바꾸는 방법과, **makeStyles, styled, withStyles**을 사용해 특정한 component만 수정하여 사용하는 방법이 있다.

먼저 theme을 살펴보자. Customized theme을 사용하려면 먼저 **createMuiTheme**을 사용해 **나만의 theme 오브젝트**를 만들어야 한다. Material UI 홈페이지의 [theming 페이지](https://material-ui.com/customization/theming/)를 살펴보면 기본적인 사용법을 알 수 있다. palette, typography 등 많이 사용할 법한 속성들은 문서 안에 설명이 잘 되어 있어 따라 하면 된다.

그런데 button의 borderRadius를 바꾼다거나, 그림자 없이 납작하게 보이게 하고 싶다거나 하면 **button이 어떻게 생겼는지를 Material UI github의 소스코드에서 찾아서 거기에 맞게 CSS override를 하거나 props를 수정할 필요가 있다.** 아래 예시는 이런 변화를 가져온다. 1) Palette를 수정해 버튼 및 다른 component들의 primary 색상을 보라색으로 바꾼다. 2) MuiButton(Material UI의 button component의 이름)의 CSS style root에서 borRadius를 수정해 모든 버튼이 양 옆이 둥근 모양이 되게 한다. (다시 보니 Radius 자체를 특정한 값으로 주는 방법이라 좋은 방법 같지는 않다) 3) MuiButton의 props 중에 disableElevation을 true로 주어 납작하게 만든다. 모든 버튼에서 그림자가 사라지게 된다. (디폴트는 false)

```javascript
import { createMuiTheme } from "@material-ui/core/styles";
import purple from "@material-ui/core/colors/purple";

const theme = createMuiTheme({
  palette: {
    primary: {
      main: purple[500],
    },
  },
  overrides: {
    MuiButton: {
      root: {
        borderRadius: "8em",
      },
    },
  },
  props: {
    MuiButton: {
      disableElevation: true,
    },
  },
});
```

마지막으로, Materail UI의 theme은 React의 context를 이용하기 때문에, 내가 만든 theme를 사용하는 **ThemeProvider**로 theme을 적용하려는 component들을 감싸주어야 한다. 아래 예시와 같이 사용하면 된다.

```javascript
<ThemeProvider theme={theme}>
  <CustomCheckbox />
</ThemeProvider>
```

그 다음은 `@material-ui/styles` 또는 `@material-ui/core/styles`에서 제공하는 makeStyles, styled, withStyles를 이용하는 방법이다. 각각의 사용법은 [이 문서](https://material-ui.com/styles/basics/#material-ui-styles)에 자세히 나와 있다. Theme은 여러 component들이 공유하는 속성들을 한꺼번에 바꾸는데 사용하는데, makeStyles 등은 그보다 좀더 섬세하게 하나 하나 스타일을 바꾸고자 할 때 사용하는 것 같다. 사실 theme에서도 내가 위에서 한 것처럼 특정한 component의 스타일을 바꿀 수 있다. 아마 여기서 다루는 API들도 내가 한 것과 비슷한 방식으로 특정한 상황에서만 수정한 theme이 사용되도록 하는게 아닌가 싶다.

솔직히 이게 어떻게 작동하는 건지 잘 이해를 못했다. 지금 이해한대로만 간단히 언급하고, React의 기본 개념들을 좀더 공부한 다음에 다시 살펴봐야겠다.

1. **Hook API**: makeStyles를 이용해 style sheet를 만들고, className을 이용해 특정한 component에 적용시키는 방법. CSS selector로 특정한 html tag에 스타일을 적용하는 것과 비슷한 것 같다. 한 component를 여러 스타일로 써야 하는데, 전체 코드가 길지 않아서 스타일을 나타내는 코드와 그 스타일을 사용하는 component가 있는 코드를 같이 보는 게 가독성이 더 좋을 때 사용하면 좋을 것 같다.

2. **Styled components API**: styled를 이용해 내가 지정한 style을 가지는 component를 새로 만들어 사용하는 방법. 한 component를 여러 스타일로 써야 하는데, 반복해서 사용해야 한다면 이렇게 사용하는게 코드를 읽기 쉬울 것 같다.

3. **Higher-order component API**: withStyles을 사용해, Higher-order component가 내가 지정한 style을 가지는 component를 return하도록 하는 방법. 이건 왜 쓰는걸까... 모르겠다.

Material UI 코드를 살펴보다보니 Javascript와 React의 기초적인 문법을 아직 제대로 이해하지 못했음을 깨달았다. 언제쯤 제대로 이해할 수 있을까? 당분간은 Javascript와 React의 주요 개념들을 공부해봐야겠다.
