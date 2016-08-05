React is a popular framework with many reasons behind its popularity. In this tutorial, I am going to show you one of those reasons: the efficient way with which React communicates with browsers. We'll use a simple comparison with a native JavaScript example to illustrate the difference. To follow along with this demo, you only need a browser and a code editor! You can actually also use an online coding playground application like [jsbin.com](http://jsbin.com/) or [plnkr.io](http://plnkr.co/), but I'll use local files in a browser here:

Start by creating a new directory for this demo, and launch your favorite editor there:

```
mkdir react-demo
cd react-demo
atom .
```

Create an `index.html` file in this directory, and put a standard HTML template in there:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>React Demo</title>
  </head>
  <body>
  </body>
</html>
```

Open the `index.html` file in your browser, and make sure you can access it and see the empty template.

```
open index.html # On Mac
explorer index.html # On Windows
```

Include a `script.js` file in the HTML document, this is where we'll write all the JavaScript code for this demo.

```
<script src="script.js"></script>
```

Create this `script.js` file, and in there, `console.log` something to make sure that the include statement worked:

```js
console.log("Loading script.js");
```

Check your browser console and make sure you see the "Loading script.js" message.

Now, let's bring in the React library itself, which we can include from the [Reactjs website](https://facebook.github.io/react/). Go to the downloads page there, copy both `react` and `react-dom` scripts, and include them in `index.html`:

```html
<script src="https://npmcdn.com/react@15.3.0/dist/react.js"></script>
<script src="https://npmcdn.com/react-dom@15.3.0/dist/react-dom.js"></script>
```

We're including 2 scripts here for an important reason. The React library itself can be used without a browser. To use it with a browser, we need the ReactDOM library. DOM is the abbreviation for the Document Object Model which browsers use to work with HTML documents.

When we refresh the browser now. We should see both React and ReactDOM available on the global scope.

![consel.log](https://github.com/samerbuna/react-virtual-dom-demo/raw/master/images/console.png)

To insert HTML dynamically in the browser we can simply use pure JavaScript and the DOM Web API itself. Let's create a `div` element to host our JavaScript HTML content, and give it the id "js". In the body element of `index.html`, add:

```html
<div id="js"></div>
```

Now in `script.js`, let's grab this new `div` element by its id, and put it in a constant. Name this constant `jsContainer`, and we can use `document.getElementById` to grab the `div` from HTML:

```js
const jsContainer = document.getElementById("js");
```

To control the content of this `div`, we can use the `innerHTML` setter call on the `div` element directly. We can use this call to supply any HTML template that we want inserted in the DOM. Let's insert another `div` element with a class of "demo" and the string "Hello JS" as its content:

```js
jsContainer.innerHTML = `
  <div class="demo">
    Hello JS
  </div>
`;
```

Make sure this works in the browser. You should see the "Hello JS" line on the screen now.

Both `document.getElementById` and `element.innerHTML` are actually part of the native DOM Web API. We are communicating with the browser directly here using the supported APIs of the Web platform.

When we write React code, however, we use the React API instead, and we let React communicate with the browser using the DOM Web API. React acts like our _agent_ for the browser, we mostly need to communicate with just React, our agent, and not the browser itself.

Let's create another `div` element to do the exact same code we have so far but with React API this time. Give this `div` an id of "react". In `index.html`, right under `div#js`, add:

```html
<div id="react"></div>
```

Now, in `script.js`, we still need a container constant, same way but this time pick the new react `div`:

```js
const reactContainer = document.getElementById("react");
```

Then we use the ReactDOM library to `render` React's version of the HTML template to the container element we just picked:

```js
ReactDOM.render(
  /* TODO: React's version of the HTML template */,
  reactContainer
)
```

Instead of working with strings (as we did in the native Javascript example above), using React, we work with _objects_. Any HTML string will be represented as an object using a `React.createElement` call (which is the core function in the React API). `React.createElement` has many arguments:
* The first argument is the HTML tag, which is `div` in our example.
* The second argument is an object that represents any attributes we want this tag to have. To match the native JS example we use ` { className: "demo" } `.
* The third argument is the content of the element. Let's put a "Hello React" string in there.

```js
  ReactDOM.render(
    React.createElement(
      "div",
      { className: "demo" },
      "Hello React",
    ),
    reactContainer
  );
```

Note how we used `className` instead of `class` here in the attributes because with React it's all JavaScript that matches the Web API, not HTML itself.  (In the WEB API, we also use `className` to control the class attribute of HTML elements.)

We can test this change now. The browser should render both "Hello JS" and "Hello React".

![Hellos](https://github.com/samerbuna/react-virtual-dom-demo/raw/master/images/boxes.png)

Let's style the demo divs as a box, using this CSS, just so that we can visually split the screen. In `index.html`:

```html
<style media="screen">
  .demo {
    border: 1px solid #ccc;
    margin: 1em;
    padding: 1em;
  }
</style>
```

We now have 2 nodes, one being controlled with the DOM Web API directly, and another being controlled with the React API (which in turn uses the DOM Web API).

The only major difference between the 2 ways we've written HTML to the browser is really that, in the JS version, we used a string to represent the content, while in the React version, we used pure JavaScript calls and represented the content with an object instead of a string.

To nest elements in our HTML template, it's a straight forward thing in the JS version. For example, to make the `div` render a text input element as well, we do:

```js
jsContainer.innerHTML = `
  <div class="demo">
    Hello JS
    <input />
  </div>
`;
```

We can do the same with React by adding more arguments after the 3rd argument for `React.createElement`. To match what we did in the native JS example, we can add a 4th argument for another `React.createElement` call that renders an `input` element:

```
ReactDOM.render(
  React.createElement(
    "div",
    { className: "demo" },
    "Hello React",
    React.createElement("input"),
  ),
  reactContainer
);
```

Let's also render a timestamp in both versions. In the JS version, let's put the timestamp in a paragraph element. We can use a call to `new Date()` to display a simple timestamp.

```js
jsContainer.innerHTML = `
  <div class="demo">
    Hello JS
    <input />
    <p>${new Date()}</p>
  </div>
`;
```

To do the same in React, we add a 5th argument to the top-level `div` element. This new 5th argument is another `React.createElement` call, this time using a `p` tag, with no attributes, and the `new Date()` string for content:

```js
ReactDOM.render(
  React.createElement(
    "div",
    { className: "demo" },
    "Hello React",
    React.createElement("input"),
    React.createElement(
      "p",
      null,
      new Date().toString()
    )
  ),
  reactContainer
);
```

Both JS and React versions are still rendering the exact same HTML in the browser.

![Snapshot](https://github.com/samerbuna/react-virtual-dom-demo/raw/master/images/timestamp.png)

Now, let's do something more interesting. Let's make the timestamp tick every second! We can easily repeat a function every second in a browser using a `setInterval` call.

Let's put all of our DOM manipulations for both JS and React versions in a function, call it `render`, and use it in a `setInterval` call to make it repeat every second.

Here's the full final code in `script.js`:

```js
const jsContainer = document.getElementById("js");
const reactContainer = document.getElementById("react");

const render = () => {
  jsContainer.innerHTML = `
    <div class="demo">
      Hello JS
      <input />
      <p>${new Date()}</p>
    </div>
  `;

  ReactDOM.render(
    React.createElement(
      "div",
      { className: "demo" },
      "Hello React",
      React.createElement("input"),
      React.createElement(
        "p",
        null,
        new Date().toString()
      )
    ),
    reactContainer
  );
}

setInterval(render, 1000);
```

When we refresh the browser now, the timestamp string should be ticking every second in both versions.

_This is the moment when React will potentially blow your mind_, if you try to type something in the text box of the JS version, you won't be able to. This is very much expected because we're basically throwing away the DOM node on every tick and regenerating it. However, if you try to type something in the text box that's rendered with the React version, you can certainly do so! Although the whole React rendering code is within our ticking timer, React has a smart diffing algorithm to only regenerate in its DOM node what actually needs to be regenerated, and it keeps everything else as is in the DOM node. This is why the text input box was not regenerated and we are able to type in it.

You can see this difference visually if you inspect the 2 DOM nodes in a Chrome dev tools elements panel. The Chrome div tools highlights any HTML elements that get updated. You'll see how we are regenerating the whole "js" div on every tick, while React is smartly only regenerating the paragraph with the timestamp string. This is all possible because of React's Virtual DOM which keeps the DOM versions in memory and only takes to the browser the difference between new and old versions.

![DOM](https://github.com/samerbuna/react-virtual-dom-demo/raw/master/images/react-dom.gif)

_You can see the source code of the demo [here](https://github.com/samerbuna/react-virtual-dom-demo/tree/gh-pages/demo), and you can see the demo running [here](https://samerbuna.github.io/react-virtual-dom-demo/demo/)_

Remember, this is only one reason why React is popular, but it's what I wanted to show you in this tutorial. I hope you've enjoyed it!
