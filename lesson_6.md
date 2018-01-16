# Lesson 6 - props

Similar to HTML, you can set attributes on JSX components. Unlike HTML, these
attributes can be entirely arbitrarily named, and have Javascript values
(strings, numbers, functions, object, etc). Since the values are Javascript, you
have to escape them by wrapping in single curly braces:

```javascript
React.render(
  <MyComponent someAttribute={'a-string'} isAwesome={true} />,
  document.getElementById('app')
)
```

In JSX, these attributes are known as *props*, and are available from within the
component as the first function parameter:

```javascript
function MyComponent(props) {
  if (props.isAwesome) {
    return <div>This is awesome!</div>
  } else {
    return <div>This is great!</div>
  }
}
```

Because JSX tries to be close to HTML, you can pass any HTML attribute to `div`,
`li`, etc. One of the useful ones here is the `style` attribute:

```javascript
function MyComponent(props) {
  let style = {
    color: props.isAwesome ? 'red' : 'green'
  }

  return <div style={style}>This is awesome!</div>
}
```

*Note that instead of a string (like in HTML), the JSX `style` prop is a keyed
object of style attributes to their values*

## Challenge 4

Modify `MenuItem` to accept a prop named `isActive`, which is used to set the
style of the `<li>` to be bold.

Then, pass that prop to *one* of the `<MenuItem>` elements in `src/js/index.js`.

Rebuild, restart the server, and refresh your browser, then if everything is
working correctly, you should now see one of your menu items as bolded.

Hints:

* You need to convert CSS style names fom `kebab-case` to `camelCase` when used
  in a React `style` object. [Read more here](https://facebook.github.io/react/tips/inline-styles.html)

---

What happens when you don't pass a prop in? What value does `props.isActive`
have? By default, it is `undefined`.

We want to be a bit more explicit and set a default prop value. Thankfully ES6
provides us with that functionality via [default
parameters](mdn.io/default+parameters) and :

```javascript
// If .isAwesome is not set, it will get the value `false`
function MyComponent({isAwesome = false}) {
  // Notice we can now access `isAwesome` directly without `props.` first
  if (isAwesome) {
    return <div>This is awesome!</div>
  } else {
    return <div>This is great!</div>
  }
}
```

## Challenge 5

Set the default value of `MenuItem`'s `isActive` prop to `false`.

Try changing it to `true` and see what effect it has on the element you *didn't*
pass `isActive={true}` to. (Don't forget to change it back to `false`)

### Resources

* Everything you could ever need to know about [destructuring](http://www.2ality.com/2015/01/es6-destructuring.html)
* [React Components, Elements, and Instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

---

[Lesson 5 - jsx «](lesson_5.md) | [Home](README.md) | [» Lesson 7 - state](lesson_7.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* [Lesson 4 - modules](lesson_4.md)
* [Lesson 5 - jsx](lesson_5.md)
* **Lesson 6 - props**
* [Lesson 7 - state](lesson_7.md)
* [Lesson 8 - children](lesson_8.md)
* [Lesson 9 - reusability](lesson_9.md)
