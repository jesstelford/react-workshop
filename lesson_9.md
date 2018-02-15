# Lesson 9 - reusability

The `MenuItem` component is already highly re-usable, but we've still got some
specific data stuck inside the `Menu` component. In particular: the list of menu
items.

# Challenge 14

Extract the `items` array out of the `Menu` component, and instead, pass it in
as a prop.

Also, pass in the initially active menu index as a prop `activeMenu`. You'll
need to update the `getInitialState` method to reference
`this.props.activeMenu`.

---

`Menu` is starting to look pretty re-usable now; We can give it any array of
things and it'll render them, as well as passing down which is the currently
active element.

If we think about it some more, the `Menu` component really is only interested
in displaying a list, and tracking the currently active one. So, it's not really
a `Menu`, but probably more likely a `SelectableGroup`.

# Challenge 15

Rename the `menu.js` file to `selectable-group.js`, and update the `require`
call in `src/js/index.js`.

Also rename `Menu` to `SelectableGroup` to avoid confusion in `src/js/index.js`.

---

How can we extract out the dependency of `MenuItem` contained within
`SelectableGroup`?

React provides a special prop to every component that has any nested children:
`this.props.children`. For example, in this case:

```
<ul>
  <li>Hi</li>
  <li>Ho</li>
</ul>
```

The `this.props.children` of the `ul` component would be:

```
[
  <li>Hi</li>,
  <li>Ho</li>
]
```

This allows us to pass the `MenuItem` components as the `children` prop to our
`SelectableGroup` component:

```
<SelectableGroup>
  <MenuItem name={'Hi'} />
  <MenuItem name={'Ho'} />
</SelectableGroup>
```

# Challenge 16

Move the `.map` conversion of array -> `MenuItem`'s out of
`src/js/selectable-group.js` and into `src/js/index.js`. Then render the
`menuItems` array as the children of `<SelectableGroup>`.

Notes

* You'll need to add `{this.props.children}` to the `SelectableGroup`'s `render`
  method. This will loop over the childen you've passed in and actually render
  them. Without this, nothing will be rendered.
* It may be easier to comment out the logic for `isActive` within
  `SelectableGroup` until the next challenge to get this one running.

---

But we've lost our `isActive` prop again! Once again React comes to the rescue,
allowing us to set the props from within the `SelectableGroup` component with
the
[`React.cloneElement`](https://facebook.github.io/react/blog/2015/03/03/react-v0.13-rc2.html#react.cloneelement)
method:

> ```
> ReactElement cloneElement(
>   ReactElement element,
>   [object props],
>   [children ...]
> )
> ```
> *Clone and return a new ReactElement using element as the starting point. The
> resulting element will have the original element's props with the new props
> merged in shallowly.*

It is used [like
this](https://facebook.github.io/react/blog/2015/03/03/react-v0.13-rc2.html#react.cloneelement):

```javascript
let newChildren = React.Children.map(this.props.children, function(child, index) {
  return React.cloneElement(child, { foo: true })
})
```

_Where `foo: true` is setting a new prop on the child._

# Challenge 17

Use `React.cloneElement` within `SelectableGroup` to set `isActive` on the
correct element.

Notes:

* Compare the `activeMenu` state value against the `index` to see if it's
  the active one.

---

Now `SelectableGroup` is 100% generic, passing through an `isActive` prop, and
allowing rendering of arbitrary children which could be absolutely any type of
component.

Finally, we want to be able to change which is the currently active item.

To do this, we can pass an extra prop to the children in `React.cloneElement`.
This prop should probably be called `onActivate`, and is a function which sets
the state of `activeMenu`.

If we wanted to activate menu items on click, then we can do so within
`MenuItem` by triggering the `this.props.onActivate` method within a click
handler.

# Challenge 18

Similarly to the hover [event
handler](https://facebook.github.io/react/docs/events.html#supported-events),
add a click event handler. This handler should execute `this.props.onActivate`,
and pass in `this.props.key` as its only argument.

Notes:

* The `onActivate` method's first parameter is the key of the currently active
  menu, so you can set that to the `activeMenu` state within `SelectableGroup`

---

Success! We have now built up a really generic component (`SelectableGroup`)
which renders children for us and updates its state to track the currently
selected child. We also have a slightly more specific `MenuItem` component which
can react to hovers and clicks!

In this way, it's possible to build up a library of very powerful, but still
quite lean and well encapsulated components.

---

[Lesson 8 - children «](lesson_8.md) | [Home](README.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* [Lesson 4 - modules](lesson_4.md)
* [Lesson 5 - jsx](lesson_5.md)
* [Lesson 6 - props](lesson_6.md)
* [Lesson 7 - state](lesson_7.md)
* [Lesson 8 - children](lesson_8.md)
* **Lesson 9 - reusability**
