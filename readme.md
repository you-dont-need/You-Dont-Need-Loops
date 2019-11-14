# You don't need loops

Loops are bullshit. Loops are bullshit. Hey, and we have tail-call elimination ES6 (and in many other languages), which means recursion is on the menu. This list provides as many alternatives to those dreadful loops as we can. Impress your loved ones with catamorphisms, anamorphisms, bifunctors, fix points, f-algebras, co-recursion, and more. Any loop can be captured with a fold!

![Any loop can be captured with a fold!](./loop-captured-with-fold.png)

(so many puns!)

Although they are one of the first constructs that junior programmers learn, loops can pose many potential issues in the software development process, and could be avoided in any case.

You are welcome to contribute with more items provided below.

* Please send a PR if you want to add or modify the code. No need to open an issue unless it's something big and you want to discuss.


## Voice of Developers

> [Early imperative languages didn't support recursion at all and even modern ones support it poorly, forcing them to use something else to iterate—loops. A combination of recursion and higher-order functions does the same thing but better and more naturally.](https://qr.ae/TWzG3O)

> &mdash;<cite>[Tikhon Jelvis](https://www.quora.com/profile/Tikhon-Jelvis), lead data scientist at Target working on supply chain optimization and simulation</cite>

> [We use side effects to change the state of a program over time in the loop - the program state is mutable. But state, especially non-local state, makes programs hard to write, debug, and maintain.](https://qr.ae/TWzIqg)

> &mdash;<cite>[Mark Sheldon](https://www.quora.com/profile/Mark-Sheldon-15), Lecturer in Computing at Tufts University</cite>

> [Avoid The One-off Problem, Infinite Loops, Statefulness and Hidden intent.](https://thenewstack.io/4-reasons-not-to-use-programming-loops-and-a-few-ways-to-avoid-them/)

> &mdash;<cite>[JOAB JACKSON](https://twitter.com/Joab_Jackson), Managing Editor at The New Stack University</cite>

> Nested loops, `continue`, `break` and `goto` are clever tricks to trap you. They are confusing and unmaintainable.

> &mdash;<cite>Well, this one is kinda common sense :)</cite>

> If you are still writing loops, you’re not a bad person. Just think about whether you need to write loops or if there’s a better alternative. Loops are best executed at the CPU level, well-beneath the concerns of most developers.

> &mdash;<cite>[Marco Emrich](https://twitter.com/marcoemrich), Software crafter, web dev, code coach, code retreat facilitator, author, consultant</cite>


## Potential problems :imp:

| Name                                       | Off-by-one error | Infinite loop    | Statefulness     | Hidden intent    |
| ------------------------------------------ | ---------------- | ---------------- | ---------------- | ---------------- |
| Loops                                      | Yes :scream:     | Yes :scream:     | Yes :scream:     | Yes :scream:     |
| Recursion (Without higher-order functions) | NO :green_heart: | Yes :scream:     | NO :green_heart: | Yes :scream:     |
| Recursion (With higher-order functions)    | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| Corecursion                                | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| Transducers                                | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| Monoids                                    | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |
| F-Algebras                                 | NO :green_heart: | NO :green_heart: | NO :green_heart: | NO :green_heart: |


## Limitations 

| Name        | Iteration | Transformation | Accumulation |
| ----------- | --------- | -------------- | ------------ |
| Loops       | ✔         | ✔              | ✔            |
| Recursion   | ✔         | ✔              | ✔            |
| Corecursion | ✔         | ✔              | ✔            |
| Transducers | ✔         | ✔              | ✖            |
| Monoids     | ✔         | ✖              | ✔            |
| F-Algebras  | ✖         | ✔              | ✔            |

## Quick Links

**[Not convinced?](#no-loops-are-easier-to-read-and-performant)**

**[Recursion](#recursion)**

1. [Sum](#sum)
1. [Reverse](#reverse)
1. [Tail recursive sum](#tail-recursive-sum)
1. [Reduce](#reduce)

*[With higher-order functions](#higher-order-functions)*

1. [Sum](##sum-1)
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

## No, loops are easier to read and performant

```js
const norm2 = (x, n) => {
  let theSum = 0;
  for (let i = 0; i < n; i++) { 
    theSum += x[i] * x[i]; 
  }
  return Math.sqrt(theSum);
}
```

This code is very clear and performs well, but we've entirely lost modularity. In this case, the code is very short, so we don't give the compromise a second thought. But at a larger scale, this kind of manual optimization reduces code reuse and makes components more complex. Code becomes harder to understand, and harder to test for correctness.

But what about the performance of recursions?

In tail recursion, you perform your calculations first, and then you execute the recursive call, passing the results of your current step to the next recursive step. This results in the last statement being in the form of (return (recursive-function params)). Basically, the return value of any given recursive step is the same as the return value of the next recursive call. The consequence of this is that once you are ready to perform your next recursive step, you don't need the current stack frame anymore.

## Recursion

You can immediately avoid off-by-one error and state by using recursions.

Let's define some helper functions:

```js
const first = xs => xs[0]
const rest = xs => xs.slice(1)
```

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
}
```

**[⬆ back to top](#quick-links)**

### Reduce

```js
const reduce = (f, acc, xs) =>
  xs.length === 0
    ? acc
    : reduce(f, f(acc, first(xs)), rest(xs));
```

**[⬆ back to top](#quick-links)**

## Higher-order functions

Recursion is too low-level. Not low-level in the sense of direct access to the machine but low-level in the sense of language design and abstraction. **Both loops and recursions do a poor job of signalling intent.** This is where **higher-order functions** come in. Map, filter, fold and friends package up common recursive patterns into library functions that are easier to use than direct recursion and signal intent.

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
fold((acc, x) => acc + x, 0, tree) // Try to implement `fold` yourself
//=> 10
```

**[⬆ back to top](#quick-links)**

## Transducers

Adding stateful transducers and grouping operations.

Helper functions:

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
// Try to implement append yourself 
reduce(filterer(x => x > 1,
       mapper(x => x + 1, append)),
       Nil, Cons(1, Cons(2, Cons(3, Nil))))
//=> [3,4]
```

```js
// Try to implement insert yourself 
reduce(filterer(x => x > 1,
       mapper(x => x + 1, insert)),
       Empty, Node(Node(Leaf(2), 1, Leaf(3)), 0, Leaf(4)))
//=> [3,4]
```

Iteration ✔ | Transformation ✔ | Accumulation ✖

**[⬆ back to top](#quick-links)**

## Monoids

Helper functions:

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

Iteration ✔ | Transformation ✖ | Accumulation ✔

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
//=> concatting world after replacing h with m on hello
```

**[⬆ back to top](#quick-links)**

Iteration ✖ | Transformation ✔ | Accumulation ✔

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
