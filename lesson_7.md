# Lesson 7 - state

*props* should be seen as data that is *passed in* to a component (think:
function parameters). While you can modify this data, it may change out from
under you if different props are passed into the component between renders.

To help with this, React provides a [*state*
system](https://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#components-are-just-state-machines):

> *In React, you simply update a component's state, and then render a new UI
> based on this new state. React takes care of updating the DOM for you in the
> most efficient way.*

Unlike props, you cannot pass state into a component. It is designed to be
entirely internal to a component. Think of it as variables that are local to the
component's imaginary closure.

## Stateful Components

The `MenuItem` component we've made is a _Pure Functional Component_, which
means it cannot have state. React provides another method of creating components
that is more verbose, but which supports state. Confusingly it's done by calling
`React.createClass`:

```javascript
let MyComponent = React.createClass({
  render: function() {
    // ...
  }
})
```

Every component of this form must have a `render` method. This `render` method
is where we return our `React.createElement` or JSX like before. React will use
this as the thing to render.

To convert our `MenuItem` component, we could do:

```javascript
let MenuItem = React.createClass({
  render: function() {
    return <li>Menu Item</li>
  }
})
```

But now we're missing our props, the values passed into the component. React
takes care of this by providing props as `this.props`.

Here is a hypothetical Warning component:

```javascript 
let Warning = React.createClass({
  render: function() {
    let style = {
      backgroundColor: this.props.isError ? 'red' : 'yellow'
    }

    return <div style={style}>{this.props.message}</div>
  }
})
```

And to set default prop values, we can use the `getDefaultProps` function:

```javascript
let Warning = React.createClass({

  getDefaultProps: function() {
    return {
      isError: false,
      message: 'Oops! Something went wrong!'
    }
  },

  render: function() {
    let style = {
      backgroundColor: this.props.isError ? 'red' : 'yellow'
    }

    return <div style={style}>{this.props.message}</div>
  }
})
```

## Challenge 6

Convert the Pure Function Component `MenuItem` to a Stateful Component as
demonstrated above.

Check to make sure you conversion works by rebuilding (`npm run build`),
restarting your server (`npm start`), and refreshing the webpage.

Hints:

* Only `src/js/menu-item.js` has to be modified
* Be sure to setup a default prop value for `isActive`
* See the [`React.createElement`
  docs](https://facebook.github.io/react/docs/top-level-api.html#react.createelement)

---

## Using State

As with default _props_, comonents can have default _state_. A common pattern
(although [one which is not always
recommended](https://facebook.github.io/react/tips/props-in-getInitialState-as-anti-pattern.html))
is to base initial state on the passed in props thanks to the [`getInitialState`
method](https://facebook.github.io/react/docs/component-specs.html#getinitialstate)

```javascript
let Warning = React.createClass({

  getInitialState: function() {
    return {
      // Default to showing the details when it's an error, but only show
      // summary when it's a warning
      detailsVisible: this.props.isError
    }
  },

  // ...
})
```

You can access state inside any component via `this.state`, and you can update
it by calling `this.setState()` with an object of the state changes. For
example:

```javascript
let Warning = React.createClass({

  // ...

  render: function() {
    let style = {
      backgroundColor: this.props.isError ? 'red' : 'yellow'
    }

    return (
      <div style={style}>
        {/* Always show the message */}
        {this.props.message}

        {/* Only show details if `this.state.displayDetails` is truthy */}
        {this.state.displayDetails ? (
          <p>
            {this.props.details}
          </p>
        ) : false}
      </div>
    )
  }
})
```

## Challenge 7

Update `MenuItem` to set initial state for `subMenuVisible` based on if the
props indicate the item is Active (ie; show sub menus when set to active). Then
use the state in the `render()` method in place of `this.props` to render a
dummy sub menu item.

---

Awesome, but now how do we alter the state?

Let's start by defining under what circumstances you would want to change the
state. Let's say it's based on some event. React comes with [a bunch of events
we can set as
props](https://facebook.github.io/react/docs/events.html#supported-events).

Changing some state on Click sounds like a good approach. Therefore, the one we
want is `onClick`:

```javascript
let MyComponent = React.createClass({
  render: function() {
    return <div onClick={}>This is awesome!</div>
  }
})
```

And we want to pass in a function to that prop so we can change the state:

```javascript
let MyComponent = React.createClass({
  handleClick: function() {
    console.log('clicked')
  },
  render: function() {
    return <div onClick={this.handleClick}>This is awesome!</div>
  }
})
```

_Note: `this` inside a component will always refer to the component itself._

Great, now we're capturing clicks on that div and outputting `clicked` to the
console when they occur. To set the state, we can use `setState`:

```javascript
let MyComponent = React.createClass({
  handleClick: function() {
    this.setState({
      isAwesome: !this.state.isAwesome // toggle
    })
  },
  // ...
})
```

_Note: When you call `setState()`, the component's `render()` will automatically
be executed for you._

## Challenge 8

Since we've been building a menu, we probably want to show some sort of hover
state.

Add functionality to `MenuItem` which makes the text green on hover, and turns
it back into black when not hovering.

Hints:

* You'll need 2 [event
  listeners](https://facebook.github.io/react/docs/events.html#supported-events)
  - one for when the mouse moves over the element, and one for when the mouse is
  no longer over the element.
* Use `this.setState` to set an `isHovering` boolean, then use that in your
  `render()` method

## Challenge 9

Add functionality to `MenuItem` so that when clicked, the sub menu visibility is
toggled.

Hints:

* You'll need a click event listener.

---

An important point to note about state and props: They are unique to each
component instance. This allows you to reuse the same component definition
multiple times, and have them manage all their own state. By this method of
encapsulating the state and functionality, we're building truly reusable
components.

---

[Lesson 6 - props «](lesson_6.md) | [Home](README.md) | [» Lesson 8 - children](lesson_8.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* [Lesson 4 - modules](lesson_4.md)
* [Lesson 5 - jsx](lesson_5.md)
* [Lesson 6 - props](lesson_6.md)
* **Lesson 7 - state**
* [Lesson 8 - children](lesson_8.md)
* [Lesson 9 - reusability](lesson_9.md)
