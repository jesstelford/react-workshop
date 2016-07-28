# Lesson 4 - modules

We've created our first component (`MenuItem`). Next we'll create another new
component (`Menu` which contains a series of `MenuItem`'s). But before we do,
let's think about what our code is beginning to look like.

We've been jamming all the JS into a single file so far, and that's going to get
messy pretty quick. So let's split some things out.

## Challenge 2

Move the component `MenuItem` into a new file called `lib/menu-item.js` (be sure to also include the code that renders the component itself). Then, modify `index.html` so that it loads this new file as a script.

---

Great, now we have 2 Javascript files: `lib/app.js`, and `lib/menu-item.js`.
This is a huge boon for code reusability and maintenance (not to mention the
`git` diffs).

If we continued to follow this pattern, we would end up with loads of `<script>`
tags on the page, and a whole bunch of global variables (`function MenuItem`
creates a public, mutable global variable). There is also a large performance
penalty in terms of network requests for every JS file we include in the page.\*

To solve the global variables problem, node.js defines a **module pattern**
using `require` and `module.exports`

## Modules

Modules are a way of *exporting* arbitrary code (functions, JSON,
variables, etc) for consumption without polluting the global namespace. Every
module file comes with 2 language constructs: `require` and `module.exports`.

### `module.exports`

To export anything from a module, you must assign it to `module.exports`:

`my-module.js`
```javascript
module.exports = 'foo'
```

### `require`

`require` is a function which you pass a module name to import:

`output.js`
```javascript
let myModule = require('./my-module')
```

*You can read the specifics of how the `require` resolution algorithm works
[here](https://nodejs.org/api/modules.html#modules_all_together).*

Now `myModule` will have the value `'foo'`, so we could output it:

`output.js`
```javascript
let myModule = require('./my-module')
console.log(myModule) // foo
```

## Modules in the browser

Modules are not yet built into any of our JavaScript environments (this may
change in ES2017, but we're still in 2016 today).

So we need a build tool which can mimic this behaviour for us. Enter
[Browserify](http://browserify.org/).

> *Browserify lets you `import` in the browser by bundling up all of
> your dependencies.*

Browserify will take a single entry file (eg; `src/js/index.js`), and look for
all calls to `require()`. For every call, it automatically bundles the
`require`'d file together with the entry file then saves it to a single output
file. Along the way you can add extra build tools for certain tasks that need
performing (minification for example).

Browserify is fairly straight forward to use (and has [**amazing**
documentation](https://github.com/substack/browserify-handbook)).

### Setting up Browserify

First, we'll have to install it into our project:

```bash
npm install --save-dev browserify
```

There is also a piece of configuration we now need to add:

#### The Build task

Now that the tool is setup, we can create a task which will utilize them.

We'll add that task along side the `"start"` task in our `package.json`:

`package.json`
```json
  "build": "browserify src/js/index.js -o lib/app.js"
```

The Browserify command can be broken up in `browserify <input file> -o <output
file>`

Since we want `lib/app.js` to be our output file, but it's currently got our
code in it, let's move it to `src/js/index.js` to make it the input file:

```
mkdir -p src/js
mv lib/app.js src/js/index.js
```

We also want to move our new `lib/menu-item.js` file too:

```
mv lib/menu-item.js src/js/menu-item.js
```

Now our folder structure should look like this:

```
.
├── lib
│   └── app.js
│   └── index.html
├── node_modules
│   └── ...
├── src
│   └── js
│       ├── index.js
│       └── menu-item.js
└── package.json
```

---

Before we can use browserify, we need to `require()` the `menu-item.js` file
inside of `src/js/index.js` so we can use what it _exports_.

### Exporting

Inside of `src/js/menu-item.js`, we have a single function `MenuItem`, which we
want to _export_ so our other code can use it in the browser without having the
extra `<script>` tags.

To do so, we can add the following line to the end of `src/js/menu-item.js`:

```javascript
function MenuItem() {
  return React.createElement('li', null, 'Menu Item')
}

module.exports = MenuItem
```

### `require`ing

Now to use the `MenuItem` component, which no longer available globally, we can
`require()` the `src/js/menu-item.js` file into our `src/js/index.js` file.

Add the following line to the top of your `src/js/index.js` file:

```javascript
let menuItem = require('./menu-item')
```

_Note that we used a relative path here (`./`) as the two files are in the same
directory._

_Note also we don't need to add the `.js` file extension - browserify is smart
enough to figure that out for us._

### Building

To run the build command, in your terminal execute:

```bash
npm run build
```

This will give us an output file saved to `lib/app.js`.

Restart your server (`npm start`), and refresh the webpage. You should now see
the same output we had before.

### Cleanup

The last piece is to remove the now unnecessary `<script src="menu-item.js">` code from `index.html`. We no longer need it since Browserify has bundled all that up together into `lib/app.js` for us!

---

_If you're interested, you can see what the output file looks like in
`lib/app.js`, but keep in mind it's not meant to be human-readable._

### Resources

* [_task automation with npm run_](http://substack.net/task_automation_with_npm_run)
* [`npm` docs for `"scripts"`](https://docs.npmjs.com/misc/scripts)

---

\* *Unless when you're talking about HTTP/2, but that's a whole other workshop!*

---

[Lesson 3 - components «](lesson_3.md) | [Home](README.md) | [» Lesson 5 - jsx](lesson_5.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* **Lesson 4 - modules**
* [Lesson 5 - jsx](lesson_5.md)
* [Lesson 6 - props](lesson_6.md)
* [Lesson 7 - state](lesson_7.md)
* [Lesson 8 - children](lesson_8.md)
* [Lesson 9 - reusability](lesson_9.md)
