# Frontend Guidelines
This was initially forked from [here](https://github.com/bendc/frontend-guidelines). It was a good base to start with. I've removed rules i don't agree with and kept ones i do.


## HTML

### Separate structure from aesthetics
The way we approach HTML & CSS is to split styling into 2 main responsibilities. Structural styling & aesthetic styling.

This allows us to compose layouts a lot easier by not overloading elements and components making them hard to re-use.

With the example below we control all of the vertical spacing in the section with the `_Header`, `_Body` and `_Footer` classes. This allows us to put whatever we wants inside these elements and the vertical spacing remains consistent. This means we can put carousels, cards, accordions, etc. inside the `_Body` and re-use the `sec-Section` in multiple places to keep a consistent section layout.

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
      <div class="prd-List"></div>
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

This doesn't just apply to sections. Lists of cards are a great example of this. The `Good` example may seem overly verbose but each element plays and important role in separating layout and style.

The `prd-List` allows us to vertically space the list based on it's context.<br>
The `prd-List_Items` acts as our `GridRow`<br>
The `prd-List_Item` acts as our `GridColumn`<br>
The `prd-Card` is the module we can use to style our cards

Again, this allows us to be able to use the `prd-Card` in multiple places because we aren’t adding margin to this to control the spacing when in a list.

```html
<!-- bad -->
<div class="prd-List">
  <a class="prd-Card" href="#">
    ...
  </a>
</div>

<!-- good -->
<div class="prd-List">
  <ul class="prd-List_Items">
    <li class="prd-List_Item">
      <a class="prd-Card" href="#">
        ...
      </a>
    </li>
  </ul>
</div>
```

One place where we can forego the structural elements is when using `display: grid`. Because `grid` offers `gap` on the parent we don’t need the structural children to implement the spacing.

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

Make sure you understand the semantics of the elements you're using. It's worse to use a semantic element in a wrong way than staying neutral.

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
	<head>
	  <meta charset=utf-8>
 	  <title>Contact</title>
    <link rel=stylesheet href=style.css>
  </head>

  <body>
    <h1>Contact me</h1>

    <label>
      Email address:
      <input type=email placeholder=you@email.com required>
    </label>

    <script src=main.js></script>
  </body>
</html>
```

### Accessibility

Accessibility shouldn't be an afterthought. You don't have to be a WCAG expert to improve your website, you can start immediately by fixing the little things that make a huge difference, such as:

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

While defining the language and character encoding is optional, it's recommended to always declare both at document level, even if they're specified in your HTTP headers. Favour UTF-8 over any other
character encoding.

```html
<!-- bad -->
<!doctype html>
<title>Hello, world.</title>

<!-- good -->
<!doctype html>
<html lang="en">
  <meta charset="utf-8">
  <title>Hello, world.</title>
</html>
```

### Performance

Unless there's a valid reason for loading your scripts before your content, don't block the rendering of your page. If your style sheet is heavy, isolate the styles that are absolutely required initially and defer the loading of the secondary declarations in a separate style sheet.

Two HTTP requests is significantly slower than one, but the perception of speed is the most important factor.

```html
<!-- bad -->
<!doctype html>
<meta charset="utf-8">
<script src="analytics.js"></script>
<title>Hello, world.</title>
<p>...</p>

<!-- good -->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src="analytics.js"></script>
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
Fonts are 1 of the few places where we use utility classes.

The purpose behind the font classes are to effectively create “tokens” with which we style all of our text. You can think of these along the lines of “heading 1”, “heading 2”, etc. The reason we don’t call them this is because they offer no context. Seeing `.fz-14_18` immediately tells you what it is.

All styling related to that token should be on the class. Don’t just do `font-size`, `font-weight` & `line-height`.

The syntax for the class names are: `.fz-[desktopFontSize]_[desktopLineHeight]`
```css
.fz-14_18 {
  font-size: 14px;
  font-weight: 500px;
  letter-spacing: 0.3px;
  line-height: 18px;
  text-transform: uppercase;
}
```

Obviously there are exceptions to this rule but for most typographic elements on the site, this is the preferred method.

### Lists of items (horizontal)
Horizontal list of items should have their vertical spacing done by margin-top if using `display: flex;` (If using `display: grid;` this point is negated as you have `grid-gap`).

The reason we use `margin-top` instead of `margin-bottom` is we can guarantee how many cards will be at the start but not at the end.

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
Mixins should not be used anymore. If it looks like there is a case for them then a utility class is almost always better.

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

## CSS
### Inheritance

Don't duplicate style declarations that can be inherited.

```css
/* bad */
.prd-Card_Body {
  ...
}

.prd-Card_Title {
  color: #fff;
  ...
}

.prd-Card_Text {
  color: #fff;
  ...
}

/* good */
.prd-Card_Body {
  color: #fff;
}

.prd-Card_Title {
  ...
}

.prd-Card_Text {
  ...
}
```

### Vendor prefixes

Kill obsolete vendor prefixes aggressively. If you need to use them, insert them before the standard property.

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

Favour transitions over animations. Avoid animating other properties than
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
TBC
