---
title: "Javascript promise 이해하기"
date: "2020-09-19"
---

한 스프린트가 끝날 때마다 사부님의 조언에 따라 JavaScript의 주요 개념들을 공부하고 있다.
이번에는 promise가 무엇인지 알아보고, 또 CodinGame에서 좋은 Playground를 찾아서 풀어보았다.

**살펴본 문서**

- [You Don't Know JS: Async & Performance - 1st Edition](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20&%20performance/README.md#you-dont-know-js-async--performance)
- [MDN web docs - Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [CodinGame - JavaScript promises, mastering the asynchronous](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/what-is-asynchronous-in-javascript)

[You Don't Know JS](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20&%20performance/README.md#you-dont-know-js-async--performance)는 너무 어려워서, 내용을 거의 이해하지 못했다. 내가 당연히 순서대로 처리될 것이라고 생각했던 많은 것들이 사실은 여러 이유로 내가 기대한 대로 작동하지 않을 수 있고, 그래서 *async하게 코드를 구성하는 것이 중요하다*는 것을 느끼게 된 정도로 넘어갔다. Javascript 코드를 많이 작성하고 어느 정도 익숙해져야 읽을 수 있는 책인 것 같다.

[MDN web docs - Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) 문서 역시 처음에 읽었을 때는 이해가 잘 되지 않았는데, [CodinGame의 예제](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/what-is-asynchronous-in-javascript)를 풀고 나서 다시 읽어보니 읽을 만 했다. 무엇보다 단어 하나 하나를 신중하게 썼다는 느낌을 받았다. 이 문서의 내용은 [CodinGame](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/what-is-asynchronous-in-javascript)을 다룬 후에 다음 포스팅에서 다루려고 한다.

[CodinGame](https://www.codingame.com/start)은 코딩을 게임처럼 즐겁게 하면서 배울 수 있는 서비스이다. 주된 서비스는 게임의 형태로 주어지는 문제를 코딩으로 푸는 'Practice'와 'Compete'이고, 보다 이론적인 내용은 [Tech.io](https://tech.io/)의 Playground를 가져와 제공하고 있다. 나는 Javascript Promise example을 찾다가, [JavaScript promises, mastering the asynchronous Playground](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/what-is-asynchronous-in-javascript)를 발견해서 해보았다.

Javascript에는 원래 자체적으로 async한 코드를 만들 방법이 없었고, setTimeout과 setInterval를 이용해 브라우저의 eventloop가 callback 함수를 보내도록 하여 async하게 작동하게 할 수 밖에 없었다고 한다. 그래서 이 Playground는 먼저 [setTimeout과 setInterval](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/what-is-asynchronous-in-javascript)을 다룬다. 그리고 setTimeout과 setInterval을 사용한 코드를 보여주고, 결과가 어떻게 나오는지 맞추는 [퀴즈](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/quick-quiz)를 풀게 한다. 아래와 같은 코드가 나오는데, 생각보다 재밌다.

```javascript
let fs = require("fs");

console.log("1");

fs.readFile("test.txt", "utf8", function (error, data) {
  if (error) {
    throw error;
  }

  console.log("2");
});

console.log("3");
```

(정답은 1, 3, 2)

그 다음에는 주어진 callback 함수들을 특정한 순서대로 호출시키는 코드를 작성하는 [practice](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/some-pratice)를 풀게 한다. 내가 작성한 답은 아래와 같다. callback1은 2초 뒤에 1번 호출되고, callback2는 1초 간격으로 3번 호출하는 함수이다. 자, 이렇게 callback 함수로 sync하게 작동하는 나의 첫 Javascript 코드를 작성했다!

```javascript
function job(callback1, callback2) {
  var delayedCallback1 = setTimeout(callback1, 2000);
  var count = 0;
  var repeatedCallback2 = setInterval(function () {
    callback2();
    count++;
    if (count == 3) {
      clearInterval(repeatedCallback2);
    }
  }, 1000);
}

module.exports = job;
```

[이어지는 장](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/the-challenges-of-the-asynchronous)에서는 promise가 왜 필요한지 설명한다.

Async한 코드는 오래 걸리는 작업을 기다릴 필요없이 여러 작업을 처리할 수 있도록 해, 퍼포먼스를 개선한다. 또한 브라우저는 점차 synchronous한 요청을 거부하는 방향으로 바뀔 것어서, 모든 Javascript 코드를 async하게 만들어야만 한다고 한다. 그래서 async한 Javascript 코드는 선택이 아니라 필수이다!

그런데 callback 함수를 이용하는 방식은 한계가 있다.

먼저, 코드 재사용성이 나쁘다. 특히 순서대로 처리해야하는 여러 작업에서 각각의 작업마다 callback 함수를 지정해주어야 하고, 또 이전 단계의 callback 함수가 그 다음 단계의 작업에 알려져야 하기 때문에 콜백 지옥(또는 The pyramid of DOOM)이 만들어질 수 있다.

또 여러 async한 작업의 결과를 사용하는 어떤 작업을 수행하려고 하면, 각각의 작업들이 완료되었는지를 먼저 체크해야한다. 번거롭기도 하거니와 코드도 지저분해진다.

하지만 이제 [promise class](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/the-promise-class)가 등장해 이 문제들을 모두 해결했다!

promise는 아래 코드와 같이 생성자를 통해 만들 수 있다. 아래 코드에서 볼 수 있듯이, promise는 resolve 함수와 reject 함수를 인자로 가지는 함수 오브젝트이다. 만약 promise를 만들면서 인자를 하나만 주면, 그 인자는 resolve 함수인 것으로 처리된다.

```javascript
var promise = new Promise(function (resolve, reject) {});
```

(사부님에 따르면, 요즘에는 대부분의 함수가 promise를 만들 수 있도록 만들어져 나오기 때문에, 실무에서 위처럼 생성자로 새로운 프로미스를 만드는 경우는 드물다고 한다.)

그 [다음 페이지](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/your-first-code-with-promises)는 promise로 무엇을 할 수 있는지 설명한다.

아래의 예시를 살펴보자.

```javascript
var promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve("hello world");
  }, 2000);
});

promise.then(function (data) {
  console.log(data);
});
```

위 코드를 보면, promise를 생성한 후 사용할 때 then이라는 메소드를 호출하는 것을 볼 수 있다. then은 promise 오브젝트가 가지는 특별한 메소드이다. then은 자신이 정의하는 함수(callback 함수)의 인자로 promise의 resolve 함수가 받은 값을 보내는 함수이다.

위에서 정의된 promise는 resolve 되었을 때 'hello world'로 무엇을 할지 아예 모른다. 그 promise의 메소드인 then이 그것으로 무엇을 해야하는지(여기서는 콘솔 창에 데이터를 보여주는 것)를 정의한다.

그렇기 때문에 하나의 promise가 resolve 되었을 때 아래와 같이 여러 callback을 가지도록 할 수가 있다. 코드 재사용성이 높아지는 것이다.

```javascript
var promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve("hello world");
  }, 2000);
});

promise.then(function (data) {
  console.log(data + " 1");
});

promise.then(function (data) {
  console.log(data + " 2");
});

promise.then(function (data) {
  console.log(data + " 3");
});
```

한편, promise가 두 번째 인자로 받는 reject 함수는 에러를 발생시킬 때 사용한다. then도 promise이므로, 두 번째 인자로 reject 함수를 넣을 수 있다. (함수를 하나만 입력하면 resolve 함수가 된다) 뒤에서 다루겠지만, 보통은 이렇게 쓰기보다는 catch 메소드를 사용한다.

then의 두 번째 인자로 reject 함수를 넣은 예시는 아래와 같다. (이 코드들은 모두 CodinGame의 playground 안에서 사용한 예시이다)

```javascript
var promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    reject("We are all going to die");
  }, 2000);
});

promise.then(
  function success(data) {
    console.log(data);
  },
  function error(data) {
    console.error(data);
  }
);
```

이어지는 페이지에서는 지금까지 배운 것을 잘 이해했는지 확인하는 2 가지 문제가 나온다.

첫 문제는 2초 뒤에 'hello world'를 출력하는 promise를 리턴하는 함수를 만드는 것이다.
나는 아래와 같이 답변했다.

```javascript
function job() {
  let p = new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve("hello world");
    }, 2000);
  });
  p.then((data) => {
    console.log(data);
  });
  return p;
}

module.exports = job;
```

그 다음 문제는 아래와 같이 promise를 리턴하는 함수를 만드는 것이다.

- 입력값이 숫자가 아니면 "error"(string)를 출력하며 즉시 reject되는 promise.
- 입력값이 홀수면 "odd"(string)를 출력하며 1초 뒤에 resolve되는 promise.
- 입력값이 짝수면 "even"(string)을 출력하며 2초 뒤에 reject되는 promise.

나는 아래와 같이 답변했다.

```javascript
function job(data) {
  function isEven(n) {
    return n % 2 == 0;
  }

  function isOdd(n) {
    return Math.abs(n % 2) == 1;
  }

  let p = new Promise((resolve, reject) => {
    if (isNaN(data)) {
      reject("error");
    } else if (isOdd(data)) {
      setTimeout(function () {
        resolve("odd");
      }, 1000);
    } else if (isEven(data)) {
      setTimeout(function () {
        reject("even");
      }, 2000);
    }
  });

  p.then(function (result) {
    console.log(result);
  }).catch(function (err) {
    console.log(err);
  });

  return p;
}

module.exports = job;
```

무사히 통과하고 나면, [chaining](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/chaining-the-promises)을 공부할 수 있다.

**then은 그 자신도 promise**이기 때문에 또 다른 then으로 resolve 함수가 받은 값을 보내줄 수가 있다. 이를 이용해 여러 async한 serial한 작업들을 then으로부터 값을 받는 then으로부터 값을 받는 then으로 줄줄이 이어나갈 수 있다. 이렇게 쓴 코드는 굉장히 긴 하나의 라인이 되기 때문에, 읽기 편하도록 보통 아래와 같이 끊어서 표기한다고 한다. 예시 코드는 역시 playground에서 가져온 것이다.

```javascript
var promise = job1();

promise

  .then(function (data1) {
    console.log("data1", data1);
    return job2();
  })

  .then(function (data2) {
    console.log("data2", data2);
    return "Hello world";
  })

  .then(function (data3) {
    console.log("data3", data3);
  });

function job1() {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve("result of job 1");
    }, 1000);
  });
}

function job2() {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve("result of job 2");
    }, 1000);
  });
}
```

그 다음으로는 promise를 사용하면서 [쉽게 저지르는 실수들](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/traps-of-promises)을 다룬다. [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)에도 비슷한 내용(Common mistakes)이 있다.

- **The broken chain:** then은 하나의 독립적인 promise이므로, 어떤 함수의 리턴으로 then의 결과를 사용하려면 then을 리턴해야 한다. 그런데 깜빡하고 then이 아니라 그 then에게 값을 보내는 promise를 리턴하는 수가 있다.
- **The pyramid of doom:** then이 정의하는 함수 안에서 다른 then을 리턴하는 형태로 코딩하면 안된다. 그렇게 하면 then이 resolve 함수가 받은 값을 받는 것이 아니기 때문에 promise chaning이 되지 않는다.
- **The ghost promise:** promise를 리턴할 수 있는 함수는 promise를 리턴하도록 해야 한다. 어떤 경우에는 promise로 리턴하고, 다른 경우에는 promise가 아닌 값을 리턴하도록 하면 안된다. async한 것과 sync한 것을 섞어서 쓰지 말라는 뜻인 것 같다. resolve 함수가 별도의 작업을 거치지 않고 항상 어떤 값을 리턴하게 하고 싶으면, 그냥 그 값을 리턴하는게 아니라 Promise.resolve() 메소드 등으로 auto-resolve 하는 promise를 리턴하도록 해야 한다.
- **The forgotten promise:** Promise() 생성자로 이미 정의된 promise를 리턴하는 메소드를 호출하는 새로운 promise를 만들면 안된다. 아마도 이 예시는 어떤 메소드가 promise를 리턴하는 것을 잊어버린 예시인 것 같다. promise를 리턴하도록 하고, 필요하다면 그 promise에 then으로 다른 작업을 이어서 하면 된다.

이어지는 페이지에서는 [catch](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/the-catch-method)를 다룬다.

앞에서 다룬 바와 같이, 그 자체가 promise인 then은 resolve 함수와 reject 함수를 인자로 받는다. 이때 인자를 하나만 넣으면 resolve 함수로 취급이 되고, `then(null, errorCallback)`와 같이 쓰면 resolve 함수는 없고 reject 함수만 넣어준 것이 된다. catch 메소드가 이와 같다!

하나의 then 안에 resolve 함수와 reject 함수를 모두 넣기보다는, then은 resolve 함수만 가지고 있게 하고, catch가 then의 오류 처리도 담당하도록 하는 것이 더 안전하고 읽기 편한 코드가 되기 때문에 catch를 따로 사용한다고 한다.

앞서 두 번째 인자로 reject 함수를 가지고 있던 then의 예시를 catch를 사용해 다시 작성해 보면 아래와 같다. 한결 깔끔하다!

```javascript
var promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
    reject("We are all going to die");
  }, 2000);
});

promise
  .then(function (data) {
    console.log(data);
  })
  .catch(function (error) {
    console.error(error);
  });
```

한편, .then().catch()는 try{ }catch{ }와 비슷하게 생겼고 역할도 비슷하다고 볼 수 있지만, .then().catch()은 async한 작업을 처리하는 method이고, try{ }catch{ }는 sync한 작업에서의 에러를 처리하는 문법(메소드가 아니다)이라는 점에서 다르다.

Promise는 async한 작업을 처리하는데 도움을 주는 여러가지 유용한 메소드들을 가지고 있다. [Promise.all](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/how-to-join-promises)은 서로 chaining 된 것이 아닌 여러 async한 작업들이 모두 처리되어야만 처리될 수 있는 어떤 작업이 있을 때 유용하다.

Promise.all은 promise들의 array를 인자로 받으며, promise를 리턴한다. Promise.all은 array 안의 모든 promise가 resolve 되었을 때에만 resolve되어 then이 있다면 then에 값을 넘겨줄 수 있다.

이때 then이 받는 값은 array 안의 promise들이 resolve 함수에 보내는 값들의 array이다. 그래서 아래 예시에서는 forEach를 사용해 각각의 값들을 콘솔 창에 띄우는 것을 볼 수 있다.

```javascript
var promise = Promise.all([job1(), job2(), job3()]);

promise.then(function (data) {
  console.log("All done");
  data.forEach(function (text) {
    console.log(text);
  });
});
```

한편, Promise.all은 하나라도 reject되면 reject된 promise를 리턴한다. 이런 특성을 fail-fast behaviour라고 한다. 하나라도 reject되면 다른 promise들이 resolve되는 것을 기다리지 않고 바로 reject된 promise를 리턴해버리기 때문인 것 같다. 아마도 어차피 모든 promise가 resolve되었을 때에만 어떤 작업을 수행하기 위한 메소드라서 이렇게 하는 것이 퍼포먼스를 높이기 때문일 거 같다.

resolve된 다른 promise들의 데이터에 대해서는 알 수 없다. 알고 싶다면 Promise.all에 입력하는 array 안에 auto-resolve되는 catch 함수를 넣어, error가 발생해도 항상 resolve되도록 할 수는 있다.

[Promise.race 메소드](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/a-few-tricks-with-promises)는 Promise.all처럼 promise들의 array를 입력받지만, Promise.all과는 다르게 가장 먼저 resolve 또는 reject된 promise를 리턴한다.

우와! 드디어 [마지막 문제](https://www.codingame.com/playgrounds/347/javascript-promises-mastering-the-asynchronous/the-last-challenge)이다.

문제의 내용은 아래와 같다.

id를 입력받고, 아래의 오브젝트를 resolve하는 promise를 리턴하는 함수를 만든다.
{
id: A number,
username: A string,
country: A string,
firstname: A string,
lastname: A string,
email: A string
}
username과 country는 3개의 db에 분산되어 있으며, central 함수를 통해 어떤 db에 해당 id의 username, country 정보가 있는지 알 수 있다. central 함수에서 문제가 생기면, 'Error central'라는 스트링을 reject하는 promise를 리턴해야 한다.
db1, db2, db3 함수로 username, country 정보를 가져올 수 있다. db1, db2, db3 함수에서 문제가 생기면, 각각 'Error db1', 'Error db2', 'Error db3'라는 스트링을 reject하는 promise를 리턴해야 한다.
vault 함수를 통해 firstname, lastname, email 정보를 가져올 수 있다. vault 함수에서 문제가 생기면, 'Error vault'라는 스트링을 reject하는 promise를 리턴해야 한다.
모든 정보를 성공적으로 가져오면, mark 함수를 호출해 이 id의 정보를 사용했다는 기록을 남겨야 한다. mark 함수의 작업이 완료될 때까지 기다릴 필요가 없으며, 문제가 생겨도 아무것도 하지 않아도 된다.
모든 작업은 200ms 안에 마쳐야 하며, 모든 함수는 100ms 안에 응답하지만 vault 함수는 150ms가 걸린다.
모든 함수는 promise를 리턴한다.

내가 작성한 답은 아래와 같다. 꽤나 애를 먹었다... 그래도 이 과정을 통해 promise를 더 많이 이해하게 된 것 같아 기쁘다.
아직도 모르는 것들이 있겠지만, 코드를 작성해나가면서 더 배울 수 있을 것 같다.

```javascript
let central = require("./central"),
  db1 = require("./db1"),
  db2 = require("./db2"),
  db3 = require("./db3"),
  vault = require("./vault"),
  mark = require("./mark");

const getCentral = function (id) {
  return central(id).catch(function () {
    return Promise.reject("Error central");
  });
};

const getDb1 = function (id) {
  return db1(id).catch(function () {
    return Promise.reject("Error db1");
  });
};

const getDb2 = function (id) {
  return db2(id).catch(function () {
    return Promise.reject("Error db2");
  });
};

const getDb3 = function (id) {
  return db3(id).catch(function () {
    return Promise.reject("Error db3");
  });
};

const getVault = function (id) {
  return vault(id).catch(function () {
    return Promise.reject("Error vault");
  });
};

const tryMark = function (id) {
  return mark(id).catch(function () {});
};

module.exports = function (id) {
  getBasicInfo = function (data) {
    if (data == "db1") {
      return getDb1(id);
    } else if (data == "db2") {
      return getDb2(id);
    } else if (data == "db3") {
      return getDb3(id);
    }
  };

  return Promise.all([getVault(id), getCentral(id).then(getBasicInfo)]).then(
    function ([privateInfo, basicInfo]) {
      tryMark(id);
      return (userInfo = {
        id: id,
        username: basicInfo.username,
        country: basicInfo.country,
        firstname: privateInfo.firstname,
        lastname: privateInfo.lastname,
        email: privateInfo.email,
      });
    }
  );
};
```
