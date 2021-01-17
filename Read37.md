# ES6 Syntax

ECMAScript 2015, also known as ES6, introduced many changes to JavaScript.

## Variables and constant feature comparison

| **Keyword** | **Scope** | **Hoisting** | **Can Be Reassigned** | **Can Be Redeclared** |
|-------------|-----------|-----------|-----------|-----------|
| `var` |  Function scope  | Yes | Yes | Yes |
| `let` |  Block scope  | No | Yes | No |
| `const` |  Block scope  | No | No | No |

## Variable declaration

ES6 introduced the `let` keyword, which allows for block-scoped variables which cannot be hoisted or redeclared.

```
var x = 0
let x = 0
```

## Constant declaration

ES6 introduced the const keyword, which cannot be redeclared or reassigned, but is not immutable.

```
const CONST_IDENTIFIER = 0
```

## Arrow functions

The arrow function expression syntax is a shorter way of creating a function expression. Arrow functions do not have their own `this`, do not have prototypes, cannot be used for constructors, and should not be used as object methods.

```
let func = (a) => {} // parentheses optional with one parameter
let func = (a, b, c) => {} // parentheses required with multiple parameters
```

## Template literals

### Concatenation/string interpolation

Expressions can be embedded in template literal strings.

```
let str = `Release Date: ${date}`
```

### Multi-line strings

Using template literal syntax, a JavaScript string can span multiple lines without the need for concatenation.

```
let str = `This text
            is on
            multiple lines`
```

## Implicit returns

The `return` keyword is implied and can be omitted if using arrow functions without a block body.

```
function func(a, b, c) {
  return a + b + c
}
```

## Key/property shorthand

ES6 introduces a shorter notation for assigning properties to variables of the same name.

```
var obj = {
  a: a,
  b: b,
}
```

```
let obj = {
  a,
  b,
}
```

## Method definition shorthand

The `function` keyword can be omitted when assigning methods on an object.

```
let obj = {
  a(c, d) {},
  b(e, f) {},
}
```

```
obj.a() // call method a
```

## Destructuring (object matching)

Use curly brackets to assign properties of an object to their own variable.

```
var obj = {a: 1, b: 2, c: 3}
```

```
let {a, b, c} = obj
```

## Array iteration (looping)

A more concise syntax has been introduced for iteration through arrays and other iterable objects.

```
var arr = ['a', 'b', 'c']
```

```
for (var i = 0; i < arr.length; i++) {
  console.log(arr[i])
}
```

```
for (let i of arr) {
  console.log(i)
}
```

## Default parameters

Functions can be initialized with default parameters, which will be used only if an argument is not invoked through the function.

```
var func = function (a, b) {
  b = b === undefined ? 2 : b
  return a + b
}
```

```
let func = (a, b = 2) => {
  return a + b
}
```

```
func(10) // returns 12
func(10, 5) // returns 15
```

## Spread syntax

Spread syntax can be used to expand an array.

```
let arr1 = [1, 2, 3]
let arr2 = ['a', 'b', 'c']
let arr3 = [...arr1, ...arr2]
```

```
console.log(arr3) // [1, 2, 3, "a", "b", "c"]
```

Spread syntax can be used for function arguments.

```
let arr1 = [1, 2, 3]
let func = (a, b, c) => a + b + c
```

```
console.log(func(...arr1)) // 6
```

## Classes/constructor functions

ES6 introducess the `class` syntax on top of the prototype-based constructor function.

```
class Func {
  constructor(a, b) {
    this.a = a
    this.b = b
  }

  getSum() {
    return this.a + this.b
  }
}

let x = new Func(3, 4)
```

## Inheritance

The `extends` keyword creates a subclass.

```
class Inheritance extends Func {
  constructor(a, b, c) {
    super(a, b)

    this.c = c
  }

  getProduct() {
    return this.a * this.b * this.c
  }
}

let y = new Inheritance(3, 4, 5)
```

## Modules - export/import

Modules can be created to export and import code between files.

```
<script src="export.js"></script>
<script type="module" src="import.js"></script>
```

## Promises/Callbacks

Promises represent the completion of an asynchronous function. They can be used as an alternative to chaining functions.

```
let doSecond = () => {
  console.log('Do second.')
}

let doFirst = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('Do first.')

    resolve()
  }, 500)
})

doFirst.then(doSecond)
```

# JSX

## Introducing JSX

The smallest React example looks like this:

```
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

Consider this variable declaration:

```
const element = <h1>Hello, world!</h1>;
```

This funny tag syntax is neither a string nor HTML

It is called JSX, and it is a syntax extension to JavaScript. We recommend using it with React to describe what the UI should look like. JSX may remind you of a template language, but it comes with the full power of JavaScript.

### Why JSX

React embraces the fact that rendering logic is inherently coupled with other UI logic: how events are handled, how the state changes over time, and how the data is prepared for display.

### Embedding Expressions in JSX

In the example below, we declare a variable called `name` and then use it inside JSX by wrapping it in curly braces:

```
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

You can put any valid JavaScript expression inside the curly braces in JSX.

In the example below, we embed the result of calling a JavaScript function, `formatName(user)`, into an `<h1>` element.

```
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

### JSX is an Expression Too

After compilation, JSX expressions become regular JavaScript function calls and evaluate to JavaScript objects.

This means that you can use JSX inside of if statements and for loops, assign it to variables, accept it as arguments, and return it from functions:

```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Specifying Attributes with JSX

You may use quotes to specify string literals as attributes or may also use curly braces to embed a JavaScript expression in an attribute:

```
const element = <div tabIndex="0"></div>;
```

### Specifying Children with JSX

If a tag is empty, you may close it immediately with />, like XML:

```
const element = <img src={user.avatarUrl} />;
```

JSX tags may contain children:

```
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## Rendering Elements

An element describes what you want to see on the screen:

```
const element = <h1>Hello, world</h1>;
```

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

### Rendering an Element into the DOM

Let’s say there is a `<div>` somewhere in your HTML file:

```
<div id="root"></div>
```

We call this a “root” DOM node because everything inside it will be managed by React DOM.

Applications built with just React usually have a single root DOM node. If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.

### Updating the Rendered Element

React elements are immutable. Once you create an element, you can’t change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.

Consider this ticking clock example:

```
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

### React Only Updates What’s Necessary

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

## Components and Props

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation. This page provides an introduction to the idea of components.

### Function and Class Components

The simplest way to define a component is to write a JavaScript function:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

This function is a valid React component because it accepts a single “props” (which stands for properties) object argument with data and returns a React element. We call such components “function components” because they are literally JavaScript functions.

You can also use an ES6 class to define a component:

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

The above two components are equivalent from React’s point of view.

### Rendering a Component

elements can also represent user-defined components:

```
const element = <Welcome name="Sara" />;
```

When React sees an element representing a user-defined component, it passes JSX attributes and children to this component as a single object. We call this object “props”.

For example, this code renders “Hello, Sara” on the page:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

Let’s recap what happens in this example:

- We call `ReactDOM.render()` with the `<Welcome name="Sara" />` element.
- React calls the Welcome component with `{name: 'Sara'}` as the props.
- Our Welcome component returns a `<h1>Hello, Sara</h1>` element as the result.
- React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

### Composing Components

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components.

For example, we can create an App component that renders Welcome many times:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

**Extracting Components:** Don’t be afraid to split components into smaller components.

### Props are Read-Only

Whether you declare a component as a function or a class, it must never modify its own props. Consider this sum function:

```
function sum(a, b) {
  return a + b;
}
```

Such functions are called “pure” because they do not attempt to change their inputs, and always return the same result for the same inputs.

## State and Lifecycle

how to make the Clock component truly reusable and encapsulated. It will set up its own timer and update itself every second.

We can start by encapsulating how the clock looks:

```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

However, it misses a crucial requirement: the fact that the Clock sets up a timer and updates the UI every second should be an implementation detail of the Clock.

Ideally we want to write this once and have the Clock update itself:

```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

To implement this, we need to add “state” to the Clock component.

### Converting a Function to a Class

You can convert a function component like Clock to a class in five steps:

1. Create an ES6 class, with the same name, that extends React.Component.
2. Add a single empty method to it called render().
3. Move the body of the function into the render() method.
4. Replace props with this.props in the render() body.
5. Delete the remaining empty function declaration.

### Adding Local State to a Class

We will move the date from props to state in three steps:

1. Replace this.props.date with this.state.date in the render() method:

```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2. Add a class constructor that assigns the initial this.state:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
Note how we pass props to the base constructor:

```
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
```

Class components should always call the base constructor with props.

3. Remove the date prop from the `<Clock />` element:

```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

We will later add the timer code back to the component itself.

## Handling Events

- React events are named using camelCase, rather than lowercase.
- With JSX you pass a function as the event handler, rather than a string.

For example, the HTML:

```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

is slightly different in React:

```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

Another difference is that you cannot return false to prevent default behavior in React. You must call preventDefault explicitly.

When using React, you generally don’t need to call addEventListener to add listeners to a DOM element after it is created. Instead, just provide a listener when the element is initially rendered.

When you define a component using an ES6 class, a common pattern is for an event handler to be a method on the class. For example, this Toggle component renders a button that lets the user toggle between “ON” and “OFF” states:

```
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

### Passing Arguments to Event Handlers

Inside a loop, it is common to want to pass an extra parameter to an event handler. For example, if id is the row ID, either of the following would work:

```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

The above two lines are equivalent, and use arrow functions and Function.prototype.bind respectively.

