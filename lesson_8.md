# Lesson 8 - children

Now we've got menu items which receive props telling them if they're active, and
will internally change their state if they're hovered or clicked.

How do we change which menu item is currently active?

The currently active menu sounds like it should be part of a state for some
`Menu` component: it's something that can change over time, but not something
that should be passed down to `MenuItem` components.

We already have the beginnings of a `Menu` component in our `src/js/index.js`,
the `<ul>` element.

# Challenge 10

Extract the `<ul>` element out into a new file `menu.js` which exports a `Menu`
component.

Notes:
* Don't forget to `require` `MenuItem`.

---

Now that we have multiple menu items, it might make sense to be able to pass in
the menu's text as a prop.

# Challenge 11

Pass a prop `name` to each `MenuItem` to be rendered as the text of that menu.

Notes:

* When rendering you need to escape the prop by wrapping it in curly braces
* Make sure you set a default prop value

---

`MenuItem` is a very generic component - all of its display is now controlled by
either passing in props, or reacting to events (hovering / clicks).

This allows us to make the display of these components data driven. Ie; let's
move the required info into an array:

`src/js/menu.js`
```javascript
let items = [
  'Home',
  'Search',
  'About',
  'Contact'
]
// ...
```

Then we can loop over this array and convert each item into a new `MenuItem`:

`src/js/menu.js`
```javascript
// ...
  render: function() {
    let menuItems = items.map(function(item, index) {
      return <MenuItem key={index} name={item} />
    })
    // ...
  }
```

*If you're not familiar with the functional paradigm `map`, see the docs for
Javascript's implementation
[here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)*

You can see we're passing in an extra prop `key` to each `MenuItem` instance.
This correlates to the index of the array, but could be any unique identifier
for this collection. React uses the `key` prop internally to identify this
component as unique from the others.

`menuItems` is now an array of `MenuItem` components. We can render this the
same way we render any other javascript value, by wrapping it in curly braces
(`{menuItems}`). React is smart enough to detect it as an array and render each
item it contains individually.

# Challenge 12

Use the array format from above to render the menu items.

Note:

* Don't foget to add the `key` prop, or you'll get errors in the DevTools
  console.

---

Uh-oh, looks like we've lost the ability to set `isActive` on one of the menus.
As posed at the top of this lesson: *How do we change which menu item is currently
active?*

The `Menu` component is built on the same state system as `MenuItem`, so we can
take advantage of that to store which is currently active.

When rendering each item, we can pass in the `isActive` prop like so:

```javascript
// ...
  render: function() {
    let activeMenu = this.state.activeMenu
    let menuItems = items.map(function(item, index) {
      if (activeMenu === index) {
        return <MenuItem key={index} name={item} isActive={true} />
      } else {
        return <MenuItem key={index} name={item} />
      }
    })
    // ...
  }
```

# Challenge 13

Set an initial state for `activeMenu` to `1` (ie; the second menu item in the
array), and correctly set the prop `isActive` on the corresponding menu item as
per above

---

[Lesson 7 - state «](lesson_7.md) | [Home](README.md) | [» Lesson 9 -
reusability](lesson_9.md)

# TOC

* [Home](README.md)
* [Lesson 1 - setup](lesson_1.md)
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* [Lesson 4 - modules](lesson_4.md)
* [Lesson 5 - jsx](lesson_5.md)
* [Lesson 6 - props](lesson_6.md)
* [Lesson 7 - state](lesson_7.md)
* **Lesson 8 - children**
* [Lesson 9 - reusability](lesson_9.md)
