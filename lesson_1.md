# Lesson 1 - Setup

Since you've already [got the prerequisites setup](README.md#prerequisites), you
can now start creating a new project that we'll use to build our React
application.

## Initialize a project

First, create a new folder where you want the project to live. In my case, I
created it on my machine at `~/dev/react-workshop`.

In a terminal, change into that directory:

```bash
cd ~/dev/react-workshop
```

Now there, we'll setup a blank project using `npm`:

```bash
npm init -y
```

We now have the required `package.json` file.

## Setup Files

There is a folder we want to store our code in, let's create it now;

```bash
mkdir -p lib
```

Create a new file at `lib/index.html`, and set its contents to be:

`lib/index.html`
```html
<!doctype html>
<div id="app">Loading...</div>
<script src="app.js"></script>
```

(_this is a very barebones, fully valid HTML5 file_)

You'll see it's referencing a script file, so let's create that one at
`lib/app.js`, and give it the contents:

`lib/app.js`
```javascript
document.querySelector('#app').innerHTML = 'Hello World'
```

## Test it out

Open the `lib/index.html` in your browser. If everything works as expected, you
should see _"Hello World"_ printed.

## Add it all to git

```bash
git init
git add .
git commit -m "Initial Commit"
```

---

A basic project is now setup. The folder structure should look like:

```
.
├── lib
│   └── index.html
└── package.json
```

---

[Home](README.md) | [» Lesson 2 - react](lesson_2.md)

# TOC

* [Home](README.md)
* **Lesson 1 - setup**
* [Lesson 2 - react](lesson_2.md)
* [Lesson 3 - components](lesson_3.md)
* [Lesson 4 - modules](lesson_4.md)
* [Lesson 5 - jsx](lesson_5.md)
* [Lesson 6 - props](lesson_6.md)
* [Lesson 7 - state](lesson_7.md)
* [Lesson 8 - children](lesson_8.md)
* [Lesson 9 - reusability](lesson_9.md)
