# You don't need loops :loop:
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/you-dont-need/loops)

## 읽기 전에: 

루프는 주니어 프로그래머들이 배우는 첫 번째 과제 중 하나지만, 소프트웨어 개발 과정에서 많은 잠재적 문제를 일으킬 수 있는데다가 대부분 피할 수 없습니다.

"이거 그냥 루프 vs 재귀 아닌가요?" 예, 아닙니다. 사실 둘 다 그리 좋은 것은 아닙니다. `reduce`도 그리 좋지 않아서 함수형 프로그래밍을 하는 개발자도 탐탁치 않아하며 쓰죠. 그러나 여기서 중요한 사실은 표현력 있는 고차 함수들이 어디서 왔는지 아는 것 입니다.

루프에는 `for`, `everyone`, `while`, `do`, `for...of` 그리고 `for...in` 같은 것들이 있습니다. `map`이나 `reduce` 또한 루프를 사용한다 주장할 수 있습니다. 물론 그건 사실이며, 그래서 우리는 우리만의 정의를 하려합니다. 실생활에서는 라이브러리를 사용하거나 배열의 메서드를 사용하지만, 그래도 기초부터 탄탄하게 하여, 원리를 이해하는 것이 더 좋을 것 입니다. 당신은 성능이 좋지 않을 거라 반문할 수 있습니다. 음, 일단 알겠습니다. 그리고 우선 계속 읽어주시길 바랍니다.

자바스크립트는 절충의 세상입니다. 성능 좋은 코드, 유지보수가 가능하고 이해하기 쉬운 코드, 그리고 구조적으로 옳바른 코드를 쓰는 것 사이에는 긴장감이 있습니다. 아마도, 그것의 밸런스를 유지하는 것은 꽤 어렵기에, pull request 할 때 논쟁의 원인이 됩니다.

### 구조에 의한 정확성

> 한 줄 요약: No bugs

루프는 [Off-by-one 에러](https://en.wikipedia.org/wiki/Off-by-one_error), [무한 루프](https://ko.wikipedia.org/wiki/%EB%AC%B4%ED%95%9C_%EB%A3%A8%ED%94%84), 상태성(Statefulness), 숨겨진 의도(Hidden intent)

이렇게 네 가지 주요한 문제가 있습니다. 당신은 `for...in` 같은 루프를 사용하면 Off-by-one 에러를 없앨 수 있다 주장할 수 있습니다만, 여전히 상태를 저장하면서, 의도를 숨길 수 있습니다. 재귀에도 몇 가지 같은 문제가 있습니다.

### 인간 공학과 유지관리성

> 한 줄 요약: No refactoring

많은 개발자들은 성능 좋고 버그가 없는 코드를 작성하는데 많은 시간을 소비하기 때문에 요구 사항을 변경하는 것을 무척 싫어합니다. 새로운 요구사항이 생기면 코드를 재구성하고, 단위 테스트를 갱신해야 합니다. 코드베이스 안에서 루프를 자유롭게 이동할 수 있나요? 부작용이나, 변이가 있을 것이기에 거의 불가능 합니다. 성능상 이유로 거대한 루프와 중첩된 루프가 사용이 불가피 할 때도 있습니다. 루프 안에서는 통제되지 않는 부작용을 포함하여 무엇이든 할 수 있으므로 종종 [최소 힘의 규칙](https://en.wikipedia.org/wiki/Rule_of_least_power)을 어기는 일이 많습니다. 하스켈 같은 언어들은 반복을 "Merge" 하기 위해서 [fusion](https://stackoverflow.com/questions/38905369/what-is-fusion-in-haskell)을 사용합니다. [Wholemeal programming](https://www.quora.com/What-is-wholemeal-programming)은 코드를 모듈화하고 재사용 할 수 있는 좋은 패턴입니다.

### 런타임 성능

당신은 루프와 모든 것을 가진 가장 성능이 좋은 코드를 작성할 수 있습니다. 하지만 요구사항이 변경되었을 때 여전히 성능이 좋은가요? 당신의 성능 좋은 코드를 다른 사람들이 이해할 수 있습니까? 코드를 리팩터링한 후에도 코드가 여전히 작동합니까? 더 큰 규모에서 수동 최적화는 코드 재사용성, 모듈성을 줄이고 구성요소를 더 복잡하게 만듭니다. 코드는 이해하기 어려워지고 정확성을 테스트하기 어려워집니다.

*당신의 코드는 **변경**되고, **동료가 읽어야 한다는 사실**을 유념하시길 바랍니다. 버려질 코드를 적는다면 코드 품질을 전혀 걱정하지 않아도 됩니다.*

그래서 삼각관계의 밸런스가 중요합니다. 지금 엔지니어링 팀에서는 정확성과 인간 공학을 위해 성능을 희생하는 경우가 95%에 당합니다. 컴퓨터가 빠르기에, 조기 최적화는 대개 나쁜 것으로 간주되기 때문입니다. 하지만 루프를 고치면 엄청나게 성능을 떨어지면서, 스택 오버플로 발생할 가능성도 있습니다. 이 3가지 포인트는 똑같이 중요합니다만, 이 글은 정확성과 인간 공학에 초점을 맞추며, 실제 프로젝트에서는 최상의 지식을 활용하여 균형을 맞춰야 합니다. 이 세 가지 중에서 어느 것도 희생하지 않기 위한 노력을 바란다면 [Haskell](https://www.seas.upenn.edu/~cis194/spring13/lectures.html)을 참조하십시오. 이것은 함수형 프로그래밍에서 좋은 [성능](https://prog21.dadgum.com/40.html)을 발휘할 수 있도록 기초부터 설계한 것입니다.

우리들은 당신이 함수형 프로그래밍에 대해 기본적인 내용을 알고 있기를 기대하며, 온라인의 여러 글을 보시길 바랍니다. (예: `if` 대신 삼항식을 사용하는 이유, 변수를 변형하면 안되는 이유, 복잡성/TCO(꼬리 호출 최적화 등등)).

이하에 제공되는 다른 항목에 공헌하는 것을 환영합니다.

* 코드 추가, 수정을 하고 싶은 경우는 PR를 보내주시길 바랍니다. 이론이 없는 한, issue를 발행할 필요는 없습니다.


## 명령형 vs 선언형

재귀는 선언적인 반면, 루프는 명령적입니다. 그럼 짝수 리스트를 준비합시다: [2,4,6,8,10]

### 명령형

명령형에서 순서는, 우선 리스트의 첫 숫자를 얻습니다. 그리고 2로 나눠서, 거기에 나머지를 확인합니다. 나머지가 0이라면 무엇을 한다, 그리고 다음 숫자로 이동합니다, 등등. 이것이 루프로 루프 안에서 과정입니다. 이를 위해서 상태관리 문제가 발생합니다.(세부 사항은 밑에).

### 선언형

선언형에서는 간단하게 말하면 '짝수를 모두 내놔라' 말하면 됩니다. 짝수는 '2로 나눠 0이 되는 수'로 정의되어 있습니다. 짝수인가 어떤가 알기위한 방법을 알려줄 필요는 없습니다. 그냥 단순하게 그것을 정의할 뿐입니다. 상태도 없습니다.

https://www.quora.com/Why-doesnt-Haskell-have-loops-e-g-for-or-while/answer/Ava-Mastic


## 개발자의 목소리

> [초기 명령형 언어는 재귀를 전혀 지원하지 않았는데, 지금도 여전히 충분하지 못하며, 루프를 이용한 반복에 뭔가를 더 추가해야 했다. 재귀와 고차함수의 조합은 같은 일을 더 좋게, 더 자연스럽게 실행한다.](https://qr.ae/TWzG3O)

> &mdash;<cite>[Tikhon Jelvis](https://www.quora.com/profile/Tikhon-Jelvis), Target의 서플라인 체인 최적화와 시뮬레이션을 하는 리드 데이터 과학자</cite>

> [루프 안에서 시간의 경과와 함께 프로그램 상태를 변화시킨다, 즉 프로그램의 상태에 변이를 주기 위해 부작용을 이용하고 있습니다. 하지만, 상태, 특히 로컬 밖에서는 상태는 프로그램을 적고, 디버그해서 유지하는 것이 어려운 일입니다.](https://qr.ae/TWzIqg)

> &mdash;<cite>[Mark Sheldon](https://www.quora.com/profile/Mark-Sheldon-15), Tufts 대학 컴퓨터 학부교수</cite>

> [One-off Problem, Infinite Loops, Statefulness and Hidden intent를 피하라.](https://thenewstack.io/4-reasons-not-to-use-programming-loops-and-a-few-ways-to-avoid-them/)

> &mdash;<cite>[JOAB JACKSON](https://twitter.com/Joab_Jackson), The New Stack University 편집장</cite>

> 중첩된 루프, `continue`, `break` 그리고 `goto` 이것들은 당신을 공명의 함정에 빠트린다. 그것들은 혼란스러우면서 유지관리되지 않는다.

> &mdash;<cite>이건, 기본 상식이야 :)</cite>

> 만약 당신이 여전히 루프를 쓰고 있다해도, 당신은 나쁜 사람이 아닙니다. 루프를 쓸 필요가 있는지 아니면 더 나은 대안이 있는지 생각해보세요. 루프는 CPU 수준에서 가장 잘 실행되며, 대부분의 개발자들이 우려하는 바를 잘 알고 있습니다. 

> &mdash;<cite>[Marco Emrich](https://twitter.com/marcoemrich), 소프트웨어 크래프터, 웹 개발, 코드 코치, code retreat facilitator, 저자, 컨설턴트</cite>


## ESLint 플러그인

[eslint-plugin-fp](https://github.com/jfmengels/eslint-plugin-fp)안에 [규칙](https://github.com/jfmengels/eslint-plugin-fp/blob/master/docs/rules/no-loops.md)이 있습니다. 또한 플러그인에 [다른 많은 유용한 규칙](https://github.com/jfmengels/eslint-plugin-fp#rules)도 있으니 확인해보세요!


## 잠재적 문제 :imp:

| Name                                       | Off-by-one error | Infinite loop    | Statefulness     | Hidden intent    |
| ------------------------------------------ | ---------------- | ---------------- | ---------------- | ---------------- |
| Loops                                      | Yes :scream:     | Yes :scream:     | Yes :scream:     | Yes :scream:     |
| Iterables                                  | NO :green_heart: | NO :green_heart: | Yes :scream:     | Yes :scream:     |
| Recursion (Without higher-order functions) | NO :green_heart: | Yes :scream:     | NO :green_heart: | Yes :scream:     |
| Recursion (With higher-order functions)    | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| Corecursion                                | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| Transducers                                | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| Monoids                                    | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| F-Algebras                                 | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |


## 제한사항

| Name                   | Iteration | Transformation | Accumulation |
| ---------------------- | --------- | -------------- | ------------ |
| Loops                  | ✔         | ✔              | ✔            |
| Recursion              | ✔         | ✔              | ✔            |
| Corecursion            | ✔         | ✔              | ✔            |
| Transducers            | ✔         | ✔              | ✖            |
| Monoids                | ✔         | ✖              | ✔            |
| F-Algebras             | ✖         | ✔              | ✔            |

## Quick Links

**[Recursion](#recursion)**

1. [Sum](#sum)
1. [Reverse](#reverse)
1. [Tail recursive sum](#tail-recursive-sum)
1. [Reduce](#reduce)

*[With higher-order functions](#higher-order-functions)*

1. [Sum](#sum-1)
1. [Reverse](#reverse-1)
1. [Map](#map)
1. [Filter](#filter)
1. [All](#all)
1. [Any](#any)
1. [Size](#size)
1. [Max](#max)
1. [Min](#min)
1. [SortBy](#sortBy)
1. [Find](#find)
1. [GroupBy](#groupBy)
1. [First](#first)
1. [Last](#last)
1. [Take](#take)
1. [Drop](#drop)
1. [Paramorphism](#paramorphism)

**[Corecursion](#corecursion)**

1. [Unfold](#unfold)
1. [Range](#range)
1. [Linked list](#linked-list)
1. [Tree](#tree)

**[Transducers](#transducers)**

1. [Map](#map-1)
1. [Filter](#filter-1)
1. [Filter and Map](#filter-and-map)

**[Monoids](#monoids)**

1. [Sum](#sum-2)
1. [Product](#product)
1. [Max](#max)
1. [All](#all)
1. [Any](#any)

**[F-Algebras](#f-algebras)**

1. [Catamorphism](#catamorphism)
1. [Sum](#sum-3)
1. [Map](#map-2)
1. [Anamorphism](#anamorphism)
1. [arrToList](#arrToList)
1. [makeAlphabet](#makeAlphabet)
1. [range](#range-1)
1. [Real-world examples](#real-world-examples)

## Recursion

재귀를 사용함으로 off-by-one 에러와 상태 변이를 회피할 수 있습니다.

몇 가지 도움이 되는 함수를 정의해봅시다:

```js
const first = xs => xs[0]
const rest = xs => xs.slice(1)
```
*NOTE:* 이러한 함수는 `reduce`로도 정의되지만, 스택 오버플로를 일으키기 쉬워집니다. 되도록이면 기존의 배열 메서드를 사용합시다.


### Sum

```js
const sum = xs =>
  xs.length === 0
    ? 0
    : first(xs) + sum(rest(xs));
```

**[⬆ back to top](#quick-links)**

### Reverse

```js
const reverse = xs => 
  xs.length === 0
    ? []
    : reverse(rest(xs)).concat(first(xs));
```

**[⬆ back to top](#quick-links)**

### Tail recursive sum

```js
const sum = list => {
  const go = (acc, xs) =>
    xs.length === 0
      ? acc
      : go(acc + first(xs), rest(xs));
  return go(0, list) 

```

**[⬆ back to top](#quick-links)**

### Reduce

```js
const reduce = (f, acc, xs) =>
  xs.length === 0
    ? acc
    : reduce(f, f(acc, first(xs)), rest(xs));
```

NOTE: 현재 꼬리호출 최적화는 Safari 에서만 지원되므로 다른 JavaScript 환경에서 [꼬리 호출](https://stackoverflow.com/questions/33923/what-is-tail-recursion)은 스택 오버플로를 일으킬 가능성이 있습니다. [the Chrome devs](https://bugs.chromium.org/p/v8/issues/detail?id=4698#c75) 등, 다른 사람들이 이 테마를 온-오프에서 의논하고 있습니다만, 여기서는 루프를 사용해 타협하는 것이 좋을 것입니다(이는 삼각관계의 밸런스를 유지하기 위한 예이기도 합니다.).

```js
const reduce = function(reduceFn, accumulator, iterable){
  for (let i of iterable){
    accumulator = reduceFn(accumulator, i)
  }
  return accumulator
}
```

**[⬆ back to top](#quick-links)**

## Higher-order functions

재귀는 너무 낮은 수준입니다. 기계에 직접 접근한다는 의미에서 낮은 수준이 아니라, 언어 설계와 추상화 관점에서 낮은 수준입니다. **루프와 재귀 모두 의도를 제대로 전달할 수 없습니다.** 여기서, **고차함수**가 등장합니다. map, filter, fold 등등은 일반적인 재귀적 패턴을 패키지화 해서, 직접 재귀를 짜는 것보다 사용하기 쉬우며, 의도를 전달하기 좋은 라이브러리 함수입니다.

### Sum

```js
const sum = xs => 
  reduce((acc, x) => x + acc, 0, xs)
sum([1,2,3])
// => 6
```

**[⬆ back to top](#quick-links)**

### Reverse

```js
const reverse = xs =>
  reduce((acc, x) => [x].concat(acc), [], xs)
```

**[⬆ back to top](#quick-links)**

### Map

```js
const map = (f, xs) =>
  reduce((acc, x) => acc.concat(f(x)), [], xs)
```

**[⬆ back to top](#quick-links)**

### Filter

```js
const filter = (f, xs) =>
  reduce((acc, x) => f(x) ? acc.concat(x) : acc, [], xs)
```

**[⬆ back to top](#quick-links)**

### All

```js
const all = xs =>
  reduce((acc, x) => acc && x, true, xs)
```

**[⬆ back to top](#quick-links)**

### Any

```js
const any = xs =>
  reduce((acc, x) => acc || x, false, xs)
```

**[⬆ back to top](#quick-links)**

*NOTE:* 이하로 이어지는 내용은, 상급자를 위한 것이라 생각하셔도 됩니다. 상세한 전문용어를 모두 이해할 필요는 없이, 어떻게 해야 사물을 추상화해 잘 구축할 수 있는가, 전체적인 직감을 얻을 수 있습니다. [여기](https://www.seas.upenn.edu/~cis194/spring13/lectures.html)부터 학습을 시작하시는 것도 좋습니다. 이 코스는 Haskell 학습자라면 강력히 권장드립니다.


### Paramorphism

```js
const para = (f, acc, xs) =>
  xs.length === 0 
    ? acc
    : para(f, f(acc, first(xs), xs), rest(xs));
```

**[⬆ back to top](#quick-links)**


## Corecursion

### Unfold

```js
const unfold = (f, seed) => {
  const go = (f, seed, acc) => {
    const res = f(seed);
    return res ? go(f, res[1], acc.concat([res[0]])) : acc; 
  }
  return go(f, seed, [])
}
unfold(x => 
  x < 26
    ? [String.fromCharCode(x + 65), x + 1]
    : null
, 0);
//=> [A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z]
```

**[⬆ back to top](#quick-links)**

### Range

```js
const range = (i, count) =>
  unfold(x => (x <= count) 
    ? [x, x+1]
    : null
, i);
range(5, 10)
//=> [ 5, 6, 7, 8, 9, 10 ]
```

**[⬆ back to top](#quick-links)**

### Linked list

```js
const Nil = {}
const _Cons = function(h, tl) {
  this.head = h;
  this.tail = tl;
};
const Cons = (h, tl) => 
  new _Cons(h, tl)
```

```js
const fold = (f, acc, xs) => 
  xs.head 
    ? fold(f, f(acc, xs.head), xs.tail)
    : acc
const lst = Cons(3, Cons(4, Cons(5, Nil)));
fold((acc, x) => acc + x, 0, lst)
//=> 12
```

**[⬆ back to top](#quick-links)**

### Tree

```js
const Empty = {}
const _Leaf = function(x) { this.x = x; }
const Leaf = x => new _Leaf(x)
const _Node = function(l, x, r) {
  this.left = l;
  this.x = x;
  this.right = r;
}
const Node = (l, x, r) => new _Node(l, x, r)
```

```js
const tree = Node(Node(Leaf(2), 1, Leaf(3)), 0, Leaf(4))
fold((acc, x) => acc + x, 0, tree) // 스스로 `fold`를 구현해 보세요
//=> 10
```

**[⬆ back to top](#quick-links)**

## Transducers


상태저장 변형기 추가 및 그룹화 작업.

도우미 함수:

```js
const concat = (a, b) => a.concat(b)
```

### Map

```js
const mapper = (f, cnct) => (acc, x) => 
  cnct(acc, f(x))
reduce(mapper(x => x + 1, concat), [], [1,2,3]) 
//=> [2,3,4]
```

**[⬆ back to top](#quick-links)**

### Filter

```js
const filterer = (f, cnct) => (acc, x) => 
  f(x) ? cnct(acc, x) : acc
reduce(filterer(x => x > 1, concat), [], [1,2,3]) 
//=> [2,3]
```

**[⬆ back to top](#quick-links)**

### Filter and Map

```js
reduce(
  filterer(x => x > 1,
  mapper(x => x + 1, concat)),
  [], [1,2,3]
)
//=> [3,4]
```

```js
// `append`를 스스로 구현해보세요.
reduce(filterer(x => x > 1,
       mapper(x => x + 1, append)),
       Nil, Cons(1, Cons(2, Cons(3, Nil))))
//=> [3,4]
```

```js
// insert를 스스로 구현해보세요.
reduce(filterer(x => x > 1,
       mapper(x => x + 1, insert)),
       Empty, Node(Node(Leaf(2), 1, Leaf(3)), 0, Leaf(4)))
//=> [3,4]
```

Iteration(반복) ✔ | Transformation(변형) ✔ | Accumulation(누적) ✖

**[⬆ back to top](#quick-links)**

## Monoids

도우미 함수:

```js
const fold = xs =>
  xs.length
      ? first(xs).concat(fold(rest(xs)))
      : empty
```

### Sum

```js
const _Sum = function(x) { this.val = x }
const Sum = x => new _Sum(x)

_Sum.prototype.concat = y =>
  Sum(this.val + y.val)
_Sum.prototype.empty = () => Sum(0)
const empty = _Sum.prototype.empty()
fold([Sum(1), Sum(2), Sum(3), Sum(4)])
//=> Sum(10)
```

**[⬆ back to top](#quick-links)**

### Product

```js
const _Product = function(x) { this.val = x }
const Product = x => new _Product(x)
_Product.prototype.concat = y => Product(this.val * y.val)
_Product.prototype.empty = () => Product(1)
const empty = _Product.prototype.empty()
fold([Product(1), Product(2), Product(3), Product(4)])
//=> Product(24)
```

**[⬆ back to top](#quick-links)**

### Max

```js
const _Max = function(x) { this.val = x }
const Max = x => new _Max(x)
_Max.prototype.concat = function(y){
  return Max(this.val > y.val ? this.val : y.val)
}
_Max.prototype.empty = () => Max(-Infinity)
const empty = _Max.prototype.empty()
fold([Max(11), Max(16), Max(3), Max(9)])
//=> Max(16)
```

**[⬆ back to top](#quick-links)**

### All

```js
const _All = function(x) { this.val = x }
const All = x => new _All(x)
_All.prototype.concat = function(y){
  return All(this.val && y.val)
}
_All.prototype.empty = () => All(true)
const empty = _All.prototype.empty()
fold([All(false), All(false), All(true), All(false)])
//=> All(false)
```

**[⬆ back to top](#quick-links)**

### Any

```js
const _Any = function(x) { this.val = x }
const Any = x => new _Any(x)
_Any.prototype.concat = function(y){
  return Any(this.val || y.val)
}
_Any.prototype.empty = () => Any(false)
const empty = _Any.prototype.empty()
fold([Any(false), Any(false), Any(true), Any(false)]) 
//=> Any(true)
```

Iteration(반복) ✔ | Transformation(변형) ✖ | Accumulation(누적) ✔

**[⬆ back to top](#quick-links)**

## F-Algebras

### Catamorphism

```js
const cata = (f, xs) =>
  f(xs.map(ys => cata(f,ys)))
```

**[⬆ back to top](#quick-links)**

### Sum

```js
Nil.map = f => Nil
_Cons.prototype.map = function(f) {
   return Cons(this.head, f(this.tail))
}
const sum = (x) =>
  (x === Nil) ? 0 : x.head + x.tail
const lst = Cons(2, Cons(3, Cons(4, Nil)));
cata(sum, lst);
//=> 9
```

**[⬆ back to top](#quick-links)**

### Map

```js
const map = (f, xs) =>
  cata(x => (x == Nil) ? Nil : Cons(f(x.head), x.tail), xs)
map(x => x + 1, Cons(2, Cons(3, Cons(4, Nil))))
//=> Cons(3, Cons(4, Cons(5, Nil)))
```

```js
Empty.map = f => Empty
_Leaf.prototype.map = function(f) {
  return Leaf(this.x)
}
_Node.prototype.map = function(f) {
  return Node(f(this.left), this.x, f(this.right))
}
```

```js
const tr = Node(Node(Leaf(2), 1, Leaf(3)), 0, Leaf(4))
cata(t =>
  t.constructor === _Node
    ? t.left + t.x + t.right
    : t.constructor === _Leaf
      ? t.x
      : 0
, tr)
//=> 10
```

**[⬆ back to top](#quick-links)**

### Anamorphism

```js
const ana = (g, a) => g(a).map(x => ana(g, x))
```

**[⬆ back to top](#quick-links)**

### arrToList

```js
const arrToList = xs =>
  xs.length === 0 ? Nil : Cons(first(xs), rest(xs))
ana(arrToList, [1, 2, 3, 4, 5])
//=> Cons(1, Cons(2, Cons(3, Cons(4, Cons(5, Nil)))))
```

**[⬆ back to top](#quick-links)**

### makeAlphabet

```js
const makeAlphabet = x =>
  x > 25
    ? Nil
    : Cons(String.fromCharCode(x + 65), x + 1)
ana(makeAlphabet, 0)
//=> Cons(A, Cons(B, Cons(C, Cons(D, Cons(E, Cons(F, Cons(G, Cons(H...
```

**[⬆ back to top](#quick-links)**

### range

```js
const range = (acc, count) =>
  ana(x => (x >= count) ? Nil : Cons(x, x + 1), acc)
range(2, 10)
//=> Cons(2, Cons(3, Cons(4, Cons(5, Cons(6, Cons(7, Cons(8, Cons(9,  Nil))))))))
```

**[⬆ back to top](#quick-links)**

### Real-world examples

```js
const _Const = function(val) { this.val = val }
const Const = x => new _Const(x)
const _Add = function(x, y) {
  this.x = x;
  this.y = y;
}
const Add = (x, y) => new _Add(x, y)
const _Mul = function(x, y) { 
  this.x = x
  this.y = y
}
const Mul = (x, y) => new _Mul(x, y)

_Const.prototype.map = function(f) { return this }
_Add.prototype.map = function(f) {
  return Add(f(this.x), f(this.y))
}
_Mul.prototype.map = function(f) {
  return Mul(f(this.x), f(this.y))
}

const interpret = a =>
  a.constructor === _Mul
    ? a.x * a.y
    : a.constructor === _Add
      ? a.x + a.y
      : /* a.constructor === _Const */ a.val
const program = Mul(Add(Const(2), Const(3)), Const(4))
cata(interpret, program);
//=> 20
```

```js
const _Concat = function(v, next) {
  this.val = v;
  this.next = next;
}
const Concat = (v, x) => new _Concat(v, x)
const _Replace = function(v, x, next) { 
  this.val = v;
  this.x = x;
  this.next = next;
}
const Replace = (v, x, nt) => new _Replace(v, x, nt)
const _Input = function(v) { this.val = v }
const Input = v => new _Input(v)

_Concat.prototype.map = function(f) {
  return Concat(this.val, f(this.next))
}
_Replace.prototype.map = function(f) {
  return Replace(this.val, this.x, f(this.next))
}
_Input.prototype.map = function(f) {
  return Input(this.val)
}

const interpret = t =>
  t.constructor === _Concat
    ? t.next.concat(t.val)
    : t.constructor === _Replace
      ? t.next.replace(t.val, t.x)
      : /* t.constructor === _Input */ t.val
const prog = Concat("world", Replace("h", "m", Input("hello")))
cata(interpret, prog)
//=> melloworld

const interpret1 = t =>
  t.constructor === _Concat
    ? "concatting "+t.val+" after "+t.next
    : t.constructor === _Replace
      ? "replacing "+t.val+" with "+t.x+" on "+t.next
      : /* t.constructor === _Input */ t.val
const prog = Concat("world", Replace("h", "m", Input("hello")))
cata(interpret1, prog)
//=> hello에서 h를 m으로 바꿔 world와 연결한다.
```

**[⬆ back to top](#quick-links)**

Iteration(반복) ✖ | Transformation(변형) ✔ | Accumulation(누적) ✔

## Inspired by:

* [A Million Ways to Fold in JS](https://www.youtube.com/watch?v=JZSoPZUoR58&t=1121s)
* [You don't (may not) need Lodash/Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)


# License

MIT


[chrome-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png
[firefox-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png
[ie-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png
[opera-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png
[safari-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png
[edge-image]:
https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png
