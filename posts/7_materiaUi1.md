---
title: "material ui 알아보기 1"
date: "2020-07-22"
---

지금 만들고 있는 웹페이지에서 [Material UI](https://material-ui.com/)를 사용하고 있다. Material UI에서 theme을 적용하기 위해 살펴본 내용을 바탕으로 포스팅을 하려다, 그 전에 Material UI를 간단히 소개하는 포스팅이 필요할 것 같아 방향을 바꿨다. 이번 포스팅에서는 먼저 Material UI와 다른 ui component library에 대해 간단히 알아보려고 한다.

Material UI는 Google의 **material design**을 적용한 오픈소스 **React component library**이다. 이름이 비슷해서 매우 헷갈리지만, Material UI는 구글에서 만든 것이 아니다! 구글 검색 결과에 Material UI와 material design에 관한 내용이 뒤죽박죽으로 나오기 때문에 처음에는 같은 것인줄 알았지만, 운영 주체도 다르고 내용도 다르다.

먼저 **material design**은 React나 다른 어떤 프로그래밍 언어의 library가 아닌 **디자인 언어**이다. [Wikipedia에 따르면,](https://en.wikipedia.org/wiki/Material_Design) Google의 materail design은 구글이 2014년에 개발 및 공표한 디자인 언어이다. 자유자재로 늘어나고 변형되는 종이와 같은 'material'로 이루어졌다고 해서 material design으로 불리는 것 같다. 종이처럼 납작하면서도 재질이 있는 요소들을 입체적으로 배치하여 그림자 등으로 깊이감을 나타내는 특징이 있다.

한편, Google에서도 material design을 적용한 라이브러리를 제공하고 있다. 이 라이브러리는 [Google Material Component](https://github.com/material-components)라고 하는데, Material UI처럼 MIT 라이선스이다. 하지만 Material UI의 github star 수가 59.4k인데 반해, Google Material Component web의 star 수는 14.6k이다.

오늘의 주인공 Material UI는 [Hai Nguyen](https://github.com/hai-cea)이 처음 만들었다. [이 글](https://www.freecodecamp.org/news/meet-your-material-ui-your-new-favorite-user-interface-library-6349a1c88a8c/)에 따르면, Material ui는 Material design과 React가 모두 세상에 나온지 얼마 안 된 2014년도에 탄생했다. Material UI의 [2014년 10월 11일에 수정된 readme.md](https://github.com/hai-cea/material-ui/commit/5fc4521c2321d37eba210cf771cd595bc5facf2f)를 읽어보면, Hai Nguyen은 [Call-Em-All](https://www.call-em-all.com/)에서 사용하기 위해 Material UI를 만들었다고 한다. Hai Nguyen은 현재까지도 Call-Em-All에서 Co-founderd이자 VP of Engineering로 일하고 있다.

_참고 - material ui의 2014년 10월 11일 readme.md 중에서_

> Material-UI is a CSS framework and a set of [React](http://facebook.github.io/react/) components that implement [Google's > Material Design](https://www.google.com/design/spec/material-design/introduction.html) specification.

(중략)

> ## Contribute
>
> [Material-UI](http://callemall.github.io/material-ui/) came about from our love of [React](http://facebook.github.io/react/) and [Google's Material Design](https://www.google.com/design/spec/material-design/introduction.html). We're currently using it on a project we're working on at [Call-Em-All](https://www.call-em-all.com/) and plan on adding to it and making it better. If you'd like to help, check out our [gh-pages](https://github.com/callemall/material-ui/tree/gh-pages) branch. We'd greatly appreciate any feedback you may have. :)

위의 readme.me에도 나와있듯이, 이 당시의 Material UI는 LESS로 만들어진 CSS framewalk였다. 하지만 2016년에 지금도 Material UI의 중추 역할을 하고 있는 [Olivier Tassinari](https://github.com/oliviertassinari)의 결정으로 LESS를 벗어나 JSS를 사용하게 되었다. [이 글](https://github.com/oliviertassinari/a-journey-toward-better-style)에 JSS를 사용하기로 결정한 이유가 자세히 나온다. 요약하자면, 1) Javascript를 충분히 활용할 수 있는 API가 있는가? 2) first paint까지의 시간이 얼마나 빠른가? 3) 라이브러리의 크기(용량)가 얼마나 작은가?, 4)Server rendering이 가능한가? 5) 치우치지 않는 것인가?(많은 사람들이 사용하는가?), 6) 사용자들이 debug하기 편리한가?를 중점적으로 보았고, 추가로 CSS output의 크기를 고려했다.

그리고 [앞서 인용한 글](https://www.freecodecamp.org/news/meet-your-material-ui-your-new-favorite-user-interface-library-6349a1c88a8c/)이 쓰여진 2018년에 이르면 JSS로 만들어진 Material UI는 충분히 성숙하여 React의 성장과 함께 나날히 발전하는 훌륭한 라이브러리가 된다. 이 글에서 LESS를 벗어나 JSS를 사용하게 되어 얻은 이점을 설명해주는데, 아직 그 정도는 잘 이해를 못하겠다. 차차 알게 되겠지.

지금까지 material design과 Material UI의 정체, 그리고 Material UI의 간략한 역사와 주요 인물을 살펴보았다. 한편, Material UI말고도 여러 단체와 기업에서 좋은 오픈소스 라이브러리들을 제공하고 있다. 내 프로젝트에서 어떤 것을 사용할지 살펴보려고 잠깐 알아봤는데, 흥미로운 것들이 많았다. 오늘의 주인공은 Material UI니까, 간단하게만 살펴보자.

- [Ant Design](https://ant.design/): 알리바바 그룹에 속한 앤트 파이낸셜에서 제공하는 라이브러리이다. Material UI는 아무래도 구글스러운데, Ant Design은 좀더 apple의 디자인에 가깝다. 즉, 더 예쁘다. 나도 사용해보려고 했는데, 내가 답을 찾고 있던 문제에 대한 github discussion의 일부가 중국어로 되어있어 그만두었다... 그래도 Github star 61.9k로 내가 살펴본 라이브러리들 중에서는 1등이다. [github 주소](https://github.com/ant-design/ant-design)
- [Semantic UI](https://semantic-ui.com/): 2013년에 origami라는 이름으로 만들어진 오픈소스 라이브러리이다. 예쁘긴 한데, 2016년 이후에는 관리가 잘 안되고 있는 것 같다. Github star 48.2k [github 주소](https://github.com/Semantic-Org/Semantic-UI)
- [Blueprint](https://blueprintjs.com/docs/) 피터 틸의 데이터 기업인 팔렌티어에서 제공하는 라이브러리. 팔렌티어의 특성에 맞게 data heavy한 B2B UI에 최적화되어 있다. 많은 양의 데이터를 보기 좋게 보여주는 UI라는게 사실 정말 어려워서 더 귀한 라이브러리이다. Github star 16.5k [github 주소](https://github.com/palantir/blueprint)
- [Fluent UI](https://developer.microsoft.com/en-us/fluentui#/) 요즘 제일 잘 나가는(?) 마이크로소프트에서 제공하는 라이브러리. UI Fabric에서 이름이 바뀌었다. 문서를 어떻게 읽는지 잘 모르겠다... Github star 8.9k [github 주소](https://github.com/microsoft/fluentui)
- [Gestalt](https://gestalt.netlify.app/#/getting-started/Installation) 핀터레스트에서 제공하는 라이브러리. 핀터레스트답게 Collage, Masonry 처럼 사진을 여러 장 보여줄 때 편리한 component들이 있다. Github star 3.6k [github 주소](https://github.com/pinterest/gestalt)
- [Carbon](https://react.carbondesignsystem.com/?path=/story/accordion--default): IBM에서 제공하는 라이브러리이다. PC시대를 열어간 회사 답게, 약간은 고풍스러운 멋이 있다. 네모 반듯한 형태에, 버튼 안의 글씨를 왼쪽 상단에 align시키는 등, 레트로한 면이 내 개인 취향에는 맞다. IBM Plex에서 한글 폰트도 나왔는데 얼른 css 지원이 됐으면 좋겠다. Github star 3.2k [github 주소](https://github.com/carbon-design-system/carbon)
- [Spectrum](https://opensource.adobe.com/spectrum-css/index.html) Adobe에서 제공하는 라이브러리. 날로 무서워져가는 Adobe에서 제공하는 것답게 깔끔하고 예쁘다. star 수가 너무 적어서 무섭긴 한데, 한번쯤은 써보고 싶다. Adobe XD를 사용하는 디자이너와도 언젠가 일해보고 싶다. Github star 671 [github 주소](https://github.com/adobe/spectrum-css)

그렇다면 여러 React UI component library 중에서 어떤 것이 _가장 잘 나가는_ 라이브러리일까? Github star 수로는 Ant Design이 1등이지만, 스승님의 말로는 github star 수가 실제 사용자 수를 보장하지는 않으며, 허수가 있을 수도 있다고 해서 npm trends를 살펴보았다. [Npm trends](https://www.npmtrends.com/)는 npm package download 수를 비교해 보여주는 웹페이지로, 실제 다운로드 수이기 때문에 실제 사용자 수를 더 잘 반영하는 데이터라고 할 수 있다. 물론, npm 외에 yarn이나 다른 다운로드 경로가 있기 때문에 모든 사용자 수를 알려주는 것은 아니다. 하지만 어떤 패키지가 더 잘 나가는지 파악하기에 충분하다.

![npm trends](/images/7_MaterialUI1_npmTrends.png)

놀랍게도 Material UI가 압도적으로 Ant Design 보다 많이 쓰이고 있다. Github star 수 차이가 신기할 따름이다. 참고로, Material UI의 다운로드 수가 2018년 5월까지는 없는 것처럼 보이는 이유는 패키지 이름이 바뀌었기 때문이다. 아마도 당분간은 계속 Material UI를 쓰게 될 것 같다.
