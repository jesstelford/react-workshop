# Lesson 3 - components

## Elements

So far we've created a single *element*. This is roughly equivalent to a single
HTML DOM node containing only text.

What if we want to make the word "Hello" italic:

> *Hello*, react!

We can start nesting elements:

```javascript
ReactDOM.render(
  React.createElement('span', null,
    React.createElement('i', null, 'Hello'),
    ', react!'
  ),
  document.getElementById('app')
)
```

If you pop that into our `lib/app.js`, you'll see: *Hello*, react!

Excellent, we can render static text in a hierarchical way.

If we had more than one of these elements, say in a list, we can render them out
like:

```javascript
ReactDOM.render(
  React.createElement('ul', null,
    React.createElement('li', null,
      React.createElement('i', null, 'Hello'),
      ', react!'
    ),
    React.createElement('li', null,
      'And hello ',
      React.createElement('b', null, 'again')
    )
  ),
  document.getElementById('app')
)
```

It looks like we're building up a menu, one which needs a little more
functionality such as selecting an item, hovering, etc.

To add interactive functionality in React, there is the concept of *components*.

## Components

We're going to start with what react calls _Pure Functional Components_. These
are very similar to the _View Template Functions_ we're already familiar with.

```javascript
function MyComponent() {
  return // ....
}
```

Note that component names must start with an upper case, and must return a
single element:

```javascript
function MenuItem() {
  return React.createElement('li', null, 'Menu Item')
}
```

Now we can use `MenuItem` in place of the `'li'` in `React.createElement('li', ...`.

It's about time for our first challenge!

## Challenge 1

Can you create a `<ul>` containing 2 menu items using this new component?

```html
<ul>
  <li>Menu Item</li>
  <li>Menu Item</li>
</ul>
```

### Resources

* [*Learn Raw
  React*](http://jamesknelson.com/learn-raw-react-no-jsx-flux-es6-webpack/) by
  James K Nelson

Once all done, keep going with Lesson 4 - modules.

---

[Lesson 2 - react «](lesson_2.md) | [Home](README.md) | [» Lesson 4 - modules](lesson_4.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* **Lesson 3 - components**
* [Lesson 4 - modules](lesson_4.md)
* [Lesson 5 - jsx](lesson_5.md)
* [Lesson 6 - props](lesson_6.md)
* [Lesson 7 - state](lesson_7.md)
* [Lesson 8 - children](lesson_8.md)
* [Lesson 9 - reusability](lesson_9.md)
