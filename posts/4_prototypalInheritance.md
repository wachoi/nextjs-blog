---
title: '첫 번째 과제 - Prototypal Inheritance를 사용하는 코드 작성해보기'
date: '2020-07-08'
---

이제부터 나의 스승님이 되시는 남편이 준 첫 번째 과제이다.

'Prototypal Inheritance를 사용하는 코드 작성해보기'

우선 내가 이해한대로 Javascript의 prototypal inheritance에 대해 설명해보겠다.

Javascript는 class를 사용하지 않으며, prototype을 가진 object를 class처럼 사용한다. 지금까지 경험해본 프로그래밍 언어가 R과 javascript밖에 없는 나에게는 그다지 놀랍지 않지만, class 기반의 언어를 사용해온 사람들에게는 이런 방식이 놀랍고 혼란스럽다고 한다. [참고: MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)

Javascript에서는 **new 명령어**를 이용해 어떤 object를 상속(inherit)한 새로운 object를 만들 수 있다. 일상에서 '상속'이라는 단어는 '부모님이나 다른 누군가에게 재산이나 빚, 물건 등을 물려받는 것'이라는 뜻으로 쓰인다. 이와 비슷하게, 어떤 object가 다른 object를 상속한다는 말은 그 object의 속성을 똑같이 물려받는다는 뜻이다. 만약 내가 보험설계사인 부모님께 고객 목록을 물려받아 보험설계사가 된다면, 그 고객들에게 나는 **new 보험설계사**인 것이다. 하지만 내가 부모님께 물려받은 재산 외에 내가 벌어서 모은 재산을 가질 수 있는 것처럼, 상속받은 object는 자신의 고유한 속성을 가질 수 있다.

보험설계사의 예를 좀더 확장해보자. 부모님께서 나와 나의 형제들에게도 같은 고객 목록을 물려주셨다고 생각해보자. 나와 형제들은 이제 각자 영업을 하면서 자신만의 고객 목록을 추가해나갈 것이다. 같은 고객 목록에서 시작했으니 피 튀기는 경쟁이 되겠지만 그런 현실은 생각하지 말자. 아무튼, 그런데 부모님께서 일할 때 쓰라고 컴퓨터도 한 대 물려주셨다고 생각해보자. 나와 형제들이 각자 컴퓨터를 한 대씩 사도 되겠지만, 그보다는 한 대를 같이 쓰는게 더 효율적일 것이다. **prototype**이 해주는 일이 이와 비슷하다.

Object 안에 어떤 function이 정의되어 있는 상태에서 new 명령어를 이용해 새로운 object들을 생성하면 새로 생긴 object들은 모두 그 function을 새롭게 복사해 가지고 있게 된다. 컴퓨터를 각자 한 대씩 산 것처럼 낭비라고 할 수 있다. 이때 부모님이 되는 Object의 prototype으로 function을 정의하면 새로 생긴 object들은 function을 복사하는게 아니라 **참조하여 상속**한다. 그러니까 컴퓨터를 여러 대 만드는게 아니라 한 대의 컴퓨터를 각자 필요할 때 쓰는 것이다.

과제를 던져주는 남편에게 바로 설명하려고 했더니 'Talk is cheap. Show me the code.'라는 명언을 날려주셨다.

그래서 코드도 보여드린다. 유감스럽게도 보험설계사와는 관계가 없는 코드이다.

```javascript
function GameObject() {
}

GameObject.prototype.sayColor = function() {
  console.log('I am ' + this.type + '. My color : ' + this.color.join(', '))
}

GameObject.prototype.sayStyle = function() {
  console.log('I am ' + this.type + '. My sytle : ' + this.style.join(', '))
}

function Villager(color, style) {
  this.color = color
  this.style = style
  this.type = 'villager'
}

Villager.prototype = Object.create(GameObject.prototype)

function Item(color, style) {
  this.color = color
  this.style = style
  this.type = 'item'
}

Item.prototype = Object.create(GameObject.prototype)

var mira = new Villager(['red', 'yellow'], ['active', 'cool'])
var bigRibbon = new Item(['red'], ['cute'])

console.log(mira.sayColor())
console.log(mira.sayStyle())
console.log(bigRibbon.sayColor())
console.log(bigRibbon.sayStyle())
```

다음 포스트는 [javascript event loop](eventLoop)를 다룬다.
