---
title: "Next js의 dynamic routing 이해하기"
date: "2020-08-10"
---

처음 Next js로 이 블로그를 만들었을 때에는 Next js 공식 홈페이지의 tutorial에서 시키는대로 따라하기만 했지 잘 이해하지는 못했다. 아마 Next js 뿐 아니라 개발 공부를 하면서 접하는 모든 것들을 다시 공부해야할 날이 올 것 같다. 무엇을 모르는지 모르다보니, 실제 업무에서 사용을 해봐야만 내가 무엇을 몰랐는지가 발견된다. 안다고 생각했던 것들을 다시 보고 또 다시 보면서 점점 더 이해하게 되지 않을까... 이번 포스팅 역시 Next js로 만든 홈페이지에서 페이지를 추가하려다보니 dynamic routing 개념을 잘 이해하지 못한 것을 알게 되어 쓰게 되었다. 앞으로도 어떤 코스를 따라서 공부하기보다는 일을 하면서 필요하거나 잘 이해하지 못해 어려움을 느끼는 개념들을 그때 그때 공부하게 될 것 같다.

Dynamic routing을 설명하기 전에 먼저 Next js의 Pages와 Routing에 대해 간단히 짚고 넘어갈 필요가 있다.

Next js 문서 중 [Getting Started](https://nextjs.org/docs/getting-started)와 [Pages](https://nextjs.org/docs/basic-features/pages)를 살펴보자.

Next js는 폴더 구조와 파일명을 이용해 자동으로 웹사이트의 여러 페이지를 만들어주는 신박한 기능을 제공한다. Pages 디렉토리 안에 .js, .jsx, .ts, or .tsx 확장자를 가진 파일을 만들고, 그 파일 안에서 React Component로 내용을 export하면 그 파일의 이름을 가진 page가 만들어진다. 예를 들어, `pages/about.js` 파일을 만들고 그 안에서 아래와 같이 React Component를 export하면 `/about` 페이지가 만들어진다.

```javascript
function About() {
  return <div>About</div>;
}

export default About;
```

이렇게 어떤 페이지를 그리기 위해 그 페이지를 만드는 코드로 요청을 보내주는 것을 Route를 한다고 한다. 즉, Router는 사용자가 어떤 페이지를 방문했을 때 어떤 일이 일어나야하는지를 결정하는 메커니즘을 말한다. 참고 - [What is: Routing](https://divpusher.com/glossary/routing/#:~:text=Routing%20or%20router%20in%20web,user%20visits%20a%20certain%20page.) Next js는 폴더 구조와 파일명을 이용해서 routing을 하기 때문에, file-system based router를 가진다고 한다.

Next js의 [Router](https://nextjs.org/docs/routing/introduction) 페이지에서 file-system based router에 대해 조금 더 알아보자.

- Next js는 어떤 디렉토리에 index라는 이름의 파일이 있으면 그 파일을 그 디렉토리의 root로 route 시킨다. 즉, index가 그리는 페이지가 진입점이 된다. 다른 이름의 파일들은 그 이름이 경로가 된다.

- Next js는 폴더 구조를 그대로 routing에 적용한다. 예를 들어 `pages/blog/first-post.js`라는 파일이 있으면, `/blog/first-post`와 같이 폴더 구조가 그대로 경로가 된다.

- Next js는 square bracket([])으로 둘러싼 이름을 가진 파일이 있으면 predefined 되지 않은 경로를 만들어준다. 이를 dynamic routing이라고 한다. 예를 들어, pages/posts/\[id\].js이라는 파일은 posts/1, posts/2, posts/abc 등 앞이 'posts/'인 경로이기만 하면 어떤 경로로든 접근할 수 있다.

이제 [Dynamic Routes](https://nextjs.org/docs/routing/dynamic-routes) 페이지에서 dynamic routing에 대해 조금 더 자세히 알아보자.

위에서 예로 든 pages/posts/\[id\].js 파일은 posts/1, posts/2, posts/abc 등 대상 파일의 폴더 구조 및 파일명과 동일하게 앞이 'posts/'인 모든 경로를 통해 접근할 수 있다. 이때 '대상 파일의 폴더 구조 및 파일명과 동일하게 앞이 posts/이다'라는 것을 다른 말로 경로가 pages/posts/\[id\].js에 **match 되었다**라고 표현한다. Next js는 입력된 path를 match된 파일로 route한다.

Next js는 또한 match된 path를 그 페이지에 query parameter로 보낼 수 있다. [공식 홈페이지](<(https://nextjs.org/docs/routing/dynamic-routes)>)의 예시를 통해 살펴보자.

`pages/post/[pid].js` 라는 페이지의 내용이 아래와 같다고 생각해보자.

```javascript
import { useRouter } from "next/router";

const Post = () => {
  const router = useRouter();
  const { pid } = router.query;

  return <p>Post: {pid}</p>;
};

export default Post;
```

위 페이지는 route에 입력된 pid를 parameter로 받게 되어 있다. 즉, `/post/abc`는 `{ "pid": "abc" }`를 query object로 가진다. `/post/abc?foo=bar`와 같은 형태로 여러 개의 parameter를 보내는 것도 가능한데, parameter의 이름이 같으면 override되니 주의하자. `pages/post/[pid]/[comment].js`와 같은 형태로 여러 개의 dynamic route segment를 사용하는 것도 가능하다.

예를 들어 내가 위키피디아를 만든다고 생각해보자. 사용자가 검색해볼만한 수만개의 검색어들을 다 predefined된 페이지로 만들수도 있겠지만 끔찍한 중노동이 될 것이다. 하지만 dynamic routing을 이용하면, 경로에 입력된 검색어를 parameter로 받아서 parameter를 이용해 DB에서 데이터를 가져와 채우도록 할 수가 있을 것이다. 페이지는 가져온 데이터를 어떤 모습으로 그릴지만 묘사하면 된다. 아직 제대로 사용해보지는 않았지만, 내가 앞으로 만들어야 하는 페이지에서 유용하게 쓰일 것 같다. 경험해보면 그 진가를 더 잘 알 수 있을 것 같다.

한편, `pages/post/[...slug].js`와 같은 형태로 점 세 개를 찍어 `/post/a` 뿐 아니라 `/post/a/b`, `/post/a/b/c`와 같이 nested된 것과 같은 형태의 모든 route를 catch할 수도 있다. 이런 route를 catch all routes라고 한다. 이때 query parameter는 array 형태로 주어진다. 예를 들어, `/post/a/b`는 `{ "slug": ["a", "b"] }`를 보낸다. 단 점 세 개를 찍는 방법은 `pages/post`는 포함하지 않는다. `pages/post`까지 포함하려면 square bracket를 두 개 써서 `pages/post/[[...slug]].js`와 같이 써야 한다. 이건 Optional catch all routes라고 한다. 어떤 경우에 이런 경로가 필요한지는 아직 잘 모르겠다.

주의할 점: Predefined routes가 dynamic routes 보다 우선이다. 그리고 dynamic routes가 catch all routes보다 우선이다. 만약 `pages/post/create.js`과 `pages/post/[pid].js`이라는 파일이 있으면, `/post/create`는 `pages/post/create.js`에만 match 된다. 그리고 만약 `pages/post/[pid].js`이라는 파일과 `pages/post/[...slug].js`이라는 파일이 있으면, `/post/abc`는 `pages/post/[pid].js`에만 match 된다.
