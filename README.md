# Frontend Guidelines
This was initially forked from [here](https://github.com/bendc/frontend-guidelines). It was a good base to start with. I've removed rules i don't agree with and kept ones i do.


## HTML

### Seperate layout from style
Very rarely should classes be doing layout as well as style. In the example below we can insert any number of items in the `_Header`, `_Body` or `_Footer` below and the spacing between each of them will remain consistent. If we rely on the `_Title` to provide the spacing between that and the `nws-CardList` and a request has come in that the section needs to have `_Text` as well as the `_Title` we need to go and take the `margin-bottom` from the `_Title`, add it to the `_Text` as well as style the `_Text`. If we have it on the `_Header` originally we seperate the layout aspect from the style aspect.

```html
<!-- bad -->
<div class="sec-Section">
  <div class="sec-Section_Inner">
    <h2 class="sec-Section_Title"></h2>

    <div class="nws-CardList"></div>

    <div class="sec-Section_Links">
      <a class="sec-Section_Link sec-Section_Link-primary"></a>
      <a class="sec-Section_Link sec-Section_Link-secondary"></a>
    </div>
  </div>
</div>

<!-- good -->
<div class="sec-Section">
  <div class="sec-Section_Inner">
    <header class="sec-Section_Header">
      <h2 class="sec-Section_Title"></h2>
    </header>

    <div class="sec-Section_Body">
      <div class="nws-CardList"></div>
    </div>

    <footer class="sec-Section_Footer">
      <div class="sec-Section_Links">
        <a class="sec-Section_Link sec-Section_Link-primary"></a>
        <a class="sec-Section_Link sec-Section_Link-secondary"></a>
      </div>
    </footer>
  </div>
</div>
```

This doesn't just apply to sections. Lists of cards are a great example of this. The `Good` example may seem overly verbose but each element plays and important role in seperating layout and style.

The `nws-CardList` allows us to vertically space the list based on it's context.<br>
The `nws-CardList_Items` acts as our `GridRow`<br>
The `nws-CardList_Item` acts as our `GridColumn`<br>
The `nws-Card` is the module we can use to style our cards

```html
<!-- bad -->
<div class="nws-CardList">
  <a class="nws-Card" href="#">
    ...
  </a>
</div>

<!-- good -->
<div class="nws-CardList">
  <ul class="nws-CardList_Items">
    <li class="nws-CardList_Item">
      <a class="nws-Card" href="#">
        ...
      </a>
    </li>
  </ul>
</div>
```

### Semantics

HTML5 provides us with lots of semantic elements aimed to describe precisely the content. Make sure you benefit from its rich vocabulary.

```html
<!-- bad -->
<div id="main">
  <div class="article">
    <div class="header">
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- good -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime="2015-02-21">21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

Make sure you understand the semantics of the elements you're using. It's worse to use a semantic
element in a wrong way than staying neutral.

```html
<!-- bad -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- good -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

### Brevity

Keep your code terse. Forget about your old XHTML habits.

```html
<!-- bad -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### Accessibility

Accessibility shouldn't be an afterthought. You don't have to be a WCAG expert to improve your
website, you can start immediately by fixing the little things that make a huge difference, such as:

* learning to use the `alt` attribute properly
* making sure your links and buttons are marked as such (no `<div class=button>` atrocities)
* not relying exclusively on colors to communicate information
* explicitly labelling form controls

```html
<!-- bad -->
<h1><img alt="Logo" src="logo.png"></h1>

<!-- good -->
<h1><img alt="My Company, Inc." src="logo.png"></h1>
```

### Language

While defining the language and character encoding is optional, it's recommended to always declare
both at document level, even if they're specified in your HTTP headers. Favor UTF-8 over any other
character encoding.

```html
<!-- bad -->
<!doctype html>
<title>Hello, world.</title>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### Performance

Unless there's a valid reason for loading your scripts before your content, don't block the
rendering of your page. If your style sheet is heavy, isolate the styles that are absolutely
required initially and defer the loading of the secondary declarations in a separate style sheet.
Two HTTP requests is significantly slower than one, but the perception of speed is the most
important factor.

```html
<!-- bad -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- good -->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### Flex
Flex is used for laying out items either vertical or horizontally. `display: block` also achieve the same vertical layout as `display: flex;` does. So on smaller breakpoints work out whether `flex` is necessary.

We also don't want to make smaller browsers parse CSS code they don't have to.

```css
/* bad */
.crd-CardList_Items {
  display: flex;
  margin-right: -10px;
  margin-left: -10px;
}

.crd-CardList_Item {  
  width: 100%;
  margin-right: 10px;
  margin-left: 10px;
  
  @media (--sm) {
    width: calc((4 / 8 * 100%) - 20px);
  }
}

/* good */
.crd-CardList_Items {
  @media (--sm) {
    display: flex;
    margin-right: -10px;
    margin-left: -10px;
  }
}

.crd-CardList_Item {  
  @media (--sm) {
    width: calc((4 / 8 * 100%) - 20px);
    margin-right: 10px;
    margin-left: 10px;
  }
}
```

### Fonts
Most font sizing should be controlled through mixins. These ensures we have consistent responsive font-sizing when it comes to certain sizes.

```css
--Font-30: { font-size: 30px; letter-spacing: 0; line-height: 38px; };
--Font-32: { font-size: 32px; letter-spacing: 0; line-height: 40px; };
--Font-38: { font-size: 38px; letter-spacing: 0; line-height: 48px; };
--Font-46: { font-size: 46px; letter-spacing: 0; line-height: 52px; };
--Font-48: { font-size: 48px; letter-spacing: 0; line-height: 58px; };

/* bad */
.cls-Class1 {
  @apply --Font-30;
  
  @media (--sm) {
    @apply --Font-32;
  }
  
  @media (--md) {
    @apply --Font-38;
  }
  
  @media (--lg) {
    @apply --Font-46;
  }
}

.cls-Class2 {
  @apply --Font-32;
  
  @media (--lg) {
    @apply --Font-46;
  }
}

/* good */
@mixin Font_46 {
  @apply --Font-30;
  
  @media (--sm) {
    @apply --Font-32;
  }
  
  @media (--md) {
    @apply --Font-38;
  }
  
  @media (--lg) {
    @apply --Font-46;
  }
}

.cls-Class1 { @include Font_46; }
.cls-Class2 { @include Font_46; }
```

Obviously there are exceptions to this rule but for most typographic elements on the site this is the preferred method.

### Lists of items (horizontal)
Horizontal list of items should have their vertical spacing done by margin-top if using `display: flex;` (If using `display: grid;` this point is negated as you have `grid-gap`).

The reason we use `margin-top` instead of `margin-bottom` is we can guarentee how many cards will be at the start but not at the end.

```css
/* bad */
.lst-Cards_Items {
  flex-direction: column;

  display: flex;
  
  @media (--sm) {
    flex-direction: row;
    
    width: calc(4 / 8 * 100%);
  }
}

.lst-Cards_Item {
  margin-bottom: 30px;
  
  &:last-child {
    margin-bottom: 0;
  }
}

/* good */
.lst-Cards_Items {
  @media (--sm) {
    display: flex;
  }
}

.lst-Cards_Item {
  margin-top: 30px;
  
  @media (--sm) {
    width: calc(4 / 8 * 100%);
  }
  
  &:first-child {
    margin-top: 0;
  }
  
  &:nth-child(-n+2) {
    @media (--sm) {
      margin-top: 0;
    }
  }
}
```

In the bad example if we had 2 rows of 2 columns of cards only the last item will have a `margin-bottom` of `0` meaning the 2nd to last item will have `margin-bottom: 30px` meaning that we get 30px spacing at the bottom where we might not want it.

You also can't do `&:nth-last-child(-n+2)` because if we have some instances of 2 rows with just 1 card in the 2nd row then this means the 2nd item on the 1st row now has no `margin-bottom`.

It could be argued that you could just do a negative margin on `.lst-Cards_Items`. This is a perfectly acceptable solution when you don't know how many cards you will have per row (no `width: {x}` is set on the `.lst-Card_Item`'s). There is a drawback to this though. When you have a negative margin on the `.lst-Cards_Items` you can't have a `margin-top` that would space it from the element that comes before and in some circumstances it might no be ideal to have `margin-bottom` on the element that comes before the list of cards.

### Mixins
Mixins should not you parenthesis `()`. Mixins should be used to help keep code consistent. They allow you to encapsulate more than just a single item as a variable. If they start accepting arguments they become code generators and start masking how output code is done.

```css
/* bad */
@mixin Grid_Row($margin, $shouldWrap) {
  @if $shouldWrap {
    flex-wrap: wrap;
  }

  display: flex;
  margin-right: $margin;
  margin-left: $margin;
}

.cls-Class {
  @include Grid_Row(-15px, true)
}

/* good */
@mixin Grid_Row {
  display: flex;
  margin-right: -15px;
  margin-left: -15px;
}

.cls-Class {
  @include Grid_Row;
  
  flex-wrap: wrap;
}
```

### Shorthands
Don't use shorthands for `margin` or `padding` unless you are at the start of the class. Even then only use the shorthand if you are going to be declaring all sizes.

```css
/* bad */
.cls-Class {
  margin: 10px 0 0;
}

/* good */
.cls-Class {
  margin-top: 10px;
}


/* bad */
.cls-Class {
  margin-top: 10px;

  @media (--sm) {
    margin: 10px 25px;
  }
}

/* good */
.cls-Class {
  margin-top: 10px;

  @media (--sm) {
    margin-right: 25px;
    margin-left: 25px;
  }
}
```

### Vertical spacing
All vertical (bar very few occasions) should be done by `vr` (Vertical rhythm). Vertical rhythm is derived from the base `line-height`. For example if our base `font-size` is `16px` and the base `line-height` is `1.5` then `1vr` == `24px`.

If you use random arbitrary numbers to match things like `31px` and `27px` then this defeats the purpose of `vr`. `vr` should be used to help with consistency.

```css
/* bad */
.cls-Class1 { margin-top: 0.2vr; }
.cls-Class2 { margin-top: 0.45vr; }
.cls-Class3 { margin-top: 0.32vr; }
.cls-Class4 { margin-top: 0.8vr; }
.cls-Class5 { margin-top: 1.9vr; }

/* good */
.cls-Class1 { margin-top: 1.25vr; }
.cls-Class2 { margin-top: 0.5vr; }
.cls-Class3 { margin-top: 1vr; }
.cls-Class4 { margin-top: 1.75vr; }
.cls-Class5 { margin-top: 2vr; }
```

At the start of a project 3-4 baselines should be worked out and only they should be used throughout the project.

A few good examples would be:

| vr     | output |
| ---    | ------ |
| 0.25vr | 6px    |
| 0.5vr  | 12px   |
| 0.75vr | 18px   |
| 1vr    | 24px   |

Not so pretty, but these numbers could be made in to variables (`--Vr1: 0.23076; .cls { margin-top: var(--Vr1)vr; }`). We are aiming for consistency here over anything else so unfortunately we need to work this way sometimes.

| vr        | output |
| ---       | ------ |
| 0.23076vr | 7px    |
| 0.5vr     | 13px   |
| 0.76923vr | 20px   |
| 1vr       | 26px   |

| vr     | output |
| ---    | ------ |
| 0.25vr | 7px    |
| 0.5vr  | 14px   |
| 0.75vr | 21px   |
| 1vr    | 28px   |

## CSS

### Flow

Don't change the default behavior of an element if you can avoid it. Keep elements in the
natural document flow as much as you can. For example, removing the white-space below an
image shouldn't make you change its default display:

```css
/* bad */
img {
  display: block;
}

/* good */
img {
  vertical-align: middle;
}
```

Similarly, don't take an element off the flow if you can avoid it.

```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

### Inheritance

Don't duplicate style declarations that can be inherited.

```css
/* bad */
.nws-Card_Body {
  ...
}

.nws-Card_Title {
  color: #fff;
  ...
}

.nws-Card_Text {
  color: #fff;
  ...
}

/* good */
.nws-Card_Body {
  color: #fff;
}

.nws-Card_Title {
  ...
}

.nws-Card_Text {
  ...
}
```

### Vendor prefixes

Kill obsolete vendor prefixes aggressively. If you need to use them, insert them before the
standard property.

```css
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### Animations

Favor transitions over animations. Avoid animating other properties than
`opacity` and `transform`.

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### Colors

If you need transparency, use `rgba`. Otherwise, always use the hexadecimal format.

```css
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}
```

### Drawing

Avoid HTTP requests when the resources are easily replicable with CSS.

```css
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

## JavaScript

### Performance

Favor readability, correctness and expressiveness over performance. JavaScript will basically never
be your performance bottleneck. Optimize things like image compression, network access and DOM
reflows instead. If you remember just one guideline from this document, choose this one.

```javascript
// bad (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// good
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

### Statelessness

Try to keep your functions pure. All functions should ideally produce no side-effects, use no outside data and return new objects instead of mutating existing ones.

```javascript
// bad
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// good
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

### Natives

Rely on native methods as much as possible.

```javascript
// bad
const toArray = obj => [].slice.call(obj);

// good
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

### Coercion

Embrace implicit coercion when it makes sense. Avoid it otherwise. Don't cargo-cult.

```javascript
// bad
if (x === undefined || x === null) { ... }

// good
if (x == undefined) { ... }
```

### Loops

Don't use loops as they force you to use mutable objects. Rely on `array.prototype` or `lodash` methods.

```javascript
// bad
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// good
import { sum } from 'lodash'

// or
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```
If you can't, or if using `array.prototype` methods is arguably abusive, use recursion.

```javascript
// bad
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// bad
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// good
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDivs(howMany - 1);
};
createDivs(5);
```

### Arguments

Forget about the `arguments` object. The rest parameter is always a better option because:

1. it's named, so it gives you a better idea of the arguments the function is expecting
2. it's a real array, which makes it easier to use.

```javascript
// bad
const sortNumbers = () =>
  Array.prototype.slice.call(arguments).sort();

// good
const sortNumbers = (...numbers) => numbers.sort();
```

### Apply

Forget about `apply()`. Use the spread operator instead.

```javascript
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// bad
greet.apply(null, person);

// good
greet(...person);
```

### Bind

Don't `bind()` when there's a more idiomatic approach.

```javascript
// bad
["foo", "bar"].forEach(func.bind(this));

// good
["foo", "bar"].forEach(func, this);
```
```javascript
// bad
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// good
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

### Higher-order functions

Avoid nesting functions when you don't have to.

```javascript
// bad
[1, 2, 3].map(num => String(num));

// good
[1, 2, 3].map(String);
```

### Composition

Avoid multiple nested function calls. Use composition instead.

```javascript
const plus1 = a => a + 1;
const mult2 = a => a * 2;

// bad
mult2(plus1(5)); // => 12

// good
const pipeline = (...funcs) => val => funcs.reduce((a, b) => b(a), val);
const addThenMult = pipeline(plus1, mult2);
addThenMult(5); // => 12
```

### Caching

Cache feature tests, large data structures and any expensive operation.

```javascript
// bad
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// good
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

### Variables

Favor `const` over `let` and `let` over `var`.

```javascript
// bad
var me = new Map();
me.set("name", "Ben").set("country", "Belgium");

// good
const me = new Map();
me.set("name", "Ben").set("country", "Belgium");
```

### Conditions

Favor IIFE's and return statements over if, else if, else and switch statements.

```javascript
// bad
var grade;
if (result < 50)
  grade = "bad";
else if (result < 90)
  grade = "good";
else
  grade = "excellent";

// good
const grade = (() => {
  if (result < 50)
    return "bad";
  if (result < 90)
    return "good";
  return "excellent";
})();
```

### Object iteration

Avoid `for...in` when you can.

```javascript
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// bad
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// good
Object.keys(obj).forEach(prop => console.log(prop));
```

### Curry

Currying is a powerful but foreign paradigm for many developers. Don't abuse it as its appropriate
use cases are fairly unusual.

```javascript
// bad
const sum = a => b => a + b;
sum(5)(3); // => 8

// good
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

### Readability

Don't obfuscate the intent of your code by using seemingly smart tricks.

```javascript
// bad
foo || doSomething();

// good
if (!foo) doSomething();
```
```javascript
// bad
void function() { /* IIFE */ }();

// good
(function() { /* IIFE */ }());
```
```javascript
// bad
const n = ~~3.14;

// good
const n = Math.floor(3.14);
```

### Code reuse

Don't be afraid of creating lots of small, highly composable and reusable functions.

```javascript
// bad
arr[arr.length - 1];

// good
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```javascript
// bad
const product = (a, b) => a * b;
const triple = n => n * 3;

// good
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

### Dependencies

Minimize dependencies. Third-party is code you don't know. Don't load an entire library for just a couple of methods easily replicable:

```javascript
// bad
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// good
const compact = arr => arr.filter(el => el);
const unique = arr => [...Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```
