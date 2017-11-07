# Lesson 5 - jsx

Let's re-visit our new react component `MenuItem`:

`src/js/menu-item.js`
```javascript
function MenuItem() {
  return React.createElement('li', null, 'Menu Item')
}

module.exports = MenuItem
```

So far we've been defining all our HTML in Javascript functions
(`React.createElement` calls). This feels a bit too far removed from what the
actual output is: HTML (and besides, we're already used to writing HTML anyway,
right?).

That's why React also supports a language called
[*JSX*](https://facebook.github.io/react/docs/jsx-in-depth.html):

> *JSX is a JavaScript syntax extension that looks similar to XML. You can use a
> simple JSX syntactic transform with React.*
>
> *You don't have to use JSX with React. You can just use plain JS. However, we
> recommend using JSX because it is a concise and familiar syntax for defining
> tree structures with attributes.*
> 
> *It's more familiar for casual developers such as designers.*
> 
> *XML has the benefit of balanced opening and closing tags. This helps make
> large trees easier to read than function calls or object literals.*

## Using JSX

Let's use this XML-ish syntax in our `MenuItem` component:

`src/js/menu-item.js`
```javascript
function MenuItem() {
  return <li>Menu Item</li>
}

module.exports = MenuItem
```

Ahhh, much better!

But! If you rebuild now (`npm run build`), you'll see an error:

```
src/js/menu-item.js:2
  return <li>Menu Item</li>
         ^
ParseError: Unexpected token
```

Browserify doesn't undestand the `<li>`. Ie; it doesn't understand JSX.

### Understanding JSX

We can harnesses the power of [Babel](https://babeljs.io/) to convert those JSX
tags back into calls to `React.createElement()`, meaning the code ends up being
identical when executed, but is much easier to read when coding!

Let's install some required tools to get us up and running:

```bash
npm install --save-dev babel-core babelify babel-preset-react babel-preset-es2015
```

- `babel-core` is the core Babel compiler
- `babelify` is a tool which tells Browserify how to handle this code when it
  is seen
- `babel-preset-react` tells Babel that we want to handle react-specific (ie;
  JSX) code
- `babel-preset-es2015` tells Babel that we also want to handle all the new ES6
  code we're using. This will make our code work in older browsers too!

### Configuration

There are 2 pieces of configuration we now need to add:

1. Babel configuration
2. Browserify configuration

### 1. Babel Configuration

Babel can understand JSX (and all of ES6, aka ES2015) for us, but we need to
tell it _how_ through configuration in `package.json`:

```json
  "babel": {
    "presets": [
      "es2015",
      "react"
    ]
  }
```

This section should be added to the end of `package.json` as the last thing in
the object just before the closing `}`.

### 2. Browserify configuration

Reminder: Browserify takes any number of `npm` modules or node.js `require()`s,
and converts them into a single JS bundle which can be run in the browser.

We tell Browserify what exactly to do in the `package.json` file. Note the
`babelify` _transform_ is telling Browserify we want to use Babel for
understanding JSX, etc, 

```json
  "browserify": {
    "transform": [
      "babelify"
    ]
  }
```

This section should be added to the end of `package.json` as the last thing in
the object just after the `"babel"` configuration from above.

---

Now, building should work without errors:

```bash
npm run build
```

Restart your server, and check out the results (it should be identical to
before).

## Challenge 3

Can you also update `src/js/index.js` to use the JSX syntax?

Hints:

* You nest items the same way you do in HTML.
* If your component was named `MyComponent`, you use it in JSX as
  `<MyComponent />`
* Components *must* start with an upper case letter!

---

[Lesson 4 - modules «](lesson_4.md) | [Home](README.md) | [» Lesson 6 - props](lesson_6.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* [Lesson 4 - modules](lesson_4.md)
* **Lesson 5 - jsx**
* [Lesson 6 - props](lesson_6.md)
* [Lesson 7 - state](lesson_7.md)
* [Lesson 8 - children](lesson_8.md)
* [Lesson 9 - reusability](lesson_9.md)
