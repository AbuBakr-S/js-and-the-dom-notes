# learn-js
Learning material for JavaScript.

# Variables

## Hoisting
- Hoisting is a result of how JavaScript is interpreted by your browser. Essentially, before any JavaScript code is executed, all variables declared with var are "hoisted", which means they're **raised to the top of the function scope.**
- This is bad practice.

## let and const
- Variables declared with `let` and `const` are **scoped to the block**, not to the function. 
- With `var`, variables were either **scoped globally or locally to an entire function scope.**
- Use const as it's safer unless you need to reassign.

If a variable is declared using let or const inside a block of code (denoted by curly braces { }), then the variable is stuck in what is known as the **temporal dead zone** until the variable’s declaration is processed. This behavior prevents variables from being accessed only until after they’ve been declared.

Variables declared with `let` and `const` are **only available within the block they're declared.**

## Example: Variable scope
What do you expect to be the output from running `getClothing(false)`?
```
function getClothing(isCold) {
  if (isCold) {
    const freezing = 'Grab a jacket!';    // Temporal Dead Zone
  } else {
    const hot = 'It’s a shorts kind of day.';
    console.log(freezing);
  }
}
```
**Console:** `ReferenceError: freezing is not defined`

## Rules for using let and const
`let` and `const` also have some other interesting properties.

- Variables declared with `let` can be **reassigned**, but can’t be **redeclared** in the same scope.
- Variables declared with `const` must be **assigned an initial value**, but can’t be **redeclared in the same scope**, and can’t be **reassigned**.

# Destructuring - Extracting
Destructuring borrows inspiration from languages like Perl and Python by allowing you to **specify the elements you want to extract** from an array or object on the left side of an assignment.

## Example: Destructuring values from an array
```
const point = [10, 25, -34];
const [x, y, z] = point;
console.log(x, y, z);
```
**Console:** `10 25 -34`

*TIP: You can also ignore values when destructuring arrays. For example, `const [x, , z] = point;` ignores the y coordinate and discards it.*

## Example: Destructuring values from an object
```
const gemstone = {
  type: 'quartz',
  color: 'rose',
  carat: 21.29
};

const {type, color, carat} = gemstone;

console.log(type, color, carat);
```
**Console:** `quartz rose 21.29`

## Example: Losing access to (.this) property when destructuring
What do you expect to be returned from calling getArea()?

```
const circle = {
  radius: 10,
  color: 'orange',
  getArea: function() {
    return Math.PI * this.radius * this.radius;
  },
  getCircumference: function() {
    return 2 * Math.PI * this.radius;
  }
};

let {radius, getArea, getCircumference} = circle;
```

Calling `getArea()` will return `NaN`. When you destructure the object and store the `getArea()` method into the getArea variable, it **no longer has access to `this` in the circle object** which results in an area that is `NaN`.

# Object Literal Shorthand

## Example: Object Literal
```
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

let gemstone = {type, color, carat};
```

## Example: Method Name
```
let gemstone = {
  type,
  color,
  carat,
  calculateWorth() { ... }    // Drop the function keyword
};
```

# Loops
There are several loops you can use, each with their limitations:
- `for loop`
- `for...in` loop
- Most recently, `for...of` loop

## Examples of Loops - `for loop`
```
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (let i = 0; i < digits.length; i++) {
  console.log(digits[i]);
}
```

Key points about this loop:
- Tracking the Counter
- Tracking the Exit Condition
- Using an Index to Access Values

## Example of Loops - `for...in`
```
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const index in digits) {
  console.log(digits[index]);
}
```

Key points about this loop:
- No longer Tracking the Counter
- No longer Watching the Exit Condition
- Using an Index to Access Values
- Can get you into trouble when adding methods to an array / object because this loops over all enumerable properties.

## Example of Loops - `for...of`
This is used to loop over any type of data that is iterable.

```
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  console.log(digit);
}
```

Key points about this loop:
- Most Concise
- No longer Tracking the Counter
- No longer Watching the Exit Condition
- No longer using an Index to Access Values
- Can Stop or Break a for...of loop at anytime
- Loop over any type of data that is iterable, (meaning it follows the [iterable protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols))
- By default, this includes the data types String, Array, Map, and Set


**TIP:** It’s good practice to use plural names for objects that are collections of values. That way, when you loop over the collection, you can use the singular version of the name when referencing individual values in the collection. For example, `for (const button of buttons) {...}`

**Note:** Objects are not iterable, by default.

## Example of Loops - Using Break

```
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  if (digit % 2 === 0) {
    continue;
  }
  console.log(digit);
}
```
**Console:** 1 3 5 7 9

# Spread operator
The spread operator, written with three consecutive dots ( ... ), is new in ES6 and gives you the ability to **expand, or spread, iterable objects into multiple elements.**

```
/*
Instructions: Use the spread operator to combine the `fruits` and `vegetables` arrays into the `produce` array.
*/

const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];

const produce = [...fruits, ...vegetables];

console.log(produce);
```

# Rest parameter
The rest parameter, ( ... ), allows you to **represent an indefinite number of elements as an array.**

When assigning the values of an array to variables:
```
const order = [20.17, 18.67, 1.50, "cheese", "eggs", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
console.log(total, subtotal, tax, items);

Prints: 20.17 18.67 1.5 ["cheese", "eggs", "milk", "bread"]
```

## Rest Parameter in Variadic functions
Another use case for the rest parameter is when you’re working with variadic functions. Variadic functions are functions that **take an indefinite number of arguments.**

Fortunately, with the addition of the rest parameter, you can rewrite the sum() function to read more clearly.

For example, let’s say we have a function called sum() which calculates the sum of an indefinite amount of numbers. How might the `sum()` function be called during execution?

```
function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```

## Example of a Variadic Function Using Rest Parameter
```
function average(...nums) {
  let total = 0;
  for (const num of nums) {
    total += num;
  }
  return total === 0 ? 0 : total / nums.length;
}

console.log(average(2, 6));
console.log(average(2, 3, 3, 5, 7, 10));
console.log(average(7, 1432, 12, 13, 100));
console.log(average());
```

# The DOM
The DOM stands for "Document Object Model" and is a **tree-like structure** that is a **representation of the HTML document**, the **relationship between elements**, and contains the **content and properties** of the **elements**.

## The DOM is not:
- Part of the JavaScript language

## The DOM is:
- Constructed from the browser
- Globally accessible by JavaScript code using the `document` object

## Browser:
1.  HTML is received
2.  HTML tags are converted to tokens
  - DOCTYPE
  - start tag
  - end tag
  - comment
  - character
  - end-of-file
3.  Tokens are converted to Nodes
4.  Nodes are converted to the DOM

## Select Page Elements

### Select Indivudual Element
- document.getElementById()
  - Returns an Element

### Select Multiple Elements
- document.getElementsByClassName()
  - Returns a HTML Collection
- document.getElementsByTagName()

## Node vs node
`Node` = Class
  - Blueprint / Interface (properties and methods)
  
`node` = Object
  - The real Objects built from the Blueprints
 
### Key Terms:
**Note:** *Our use of "interface" is not related to either a UI or a GUI. Our use of `interface` is a technical, computer science word for a **list of properties and methods that are inherited**.*

- interface = blueprint
- properties = data
- methods = functionality

## Nodes, Elements and Interfaces
Each Interface has it's own set of Properties and Methods which can be inherited.

### Node Interface
#### Event Target  <--Node
[Node Interface](https://developer.mozilla.org/en-US/docs/Web/API/Node)

You can use the `Node.nodeType` Property to return the type of the node. 

#### Example values:
Name	|  Value
ELEMENT_NODE	1
ATTRIBUTE_NODE 	2
TEXT_NODE	3

### Element Interface
- Event Target
- Node
- Document
- Element

#### Event Target  <--Node  <--Element
`Element` Interface is a blueprint for creating elements: [Element Interface](https://developer.mozilla.org/en-US/docs/Web/API/Element)

### Key Points:
- The `Element` Interface is a descendant of the Node Interface.
- `Element` Interface inherits all of the Node Interface's properties and methods.

### HTML Elements Interface
#### Event Target  <--Node  <--Element <--HTMLElement 

The `HTMLElement` interface represents any HTML element. Some elements directly implement this interface, while others implement it via an interface that inherits it. See [here](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement).

To check out all of the different interfaces, check here: [Web API Interfaces](https://developer.mozilla.org/en-US/docs/Web/API).

## Creating Content with JavaScript

### Update Existing Page Content
#### Properties:
- `Element.innerHTML`: get / set an element's (and all of its descendants!) HTML content
- `Element.outerHTML`: get / set the HTML element itself, as well as its children
- `Node.textContent`: get / set the text content of an element and all its descendants
- `HTML.innerText`: Represents the "rendered" text (processes CSS) content of a node and its descendants. As a getter, it approximates the text the user would get if they highlighted the contents of the element with the cursor and then copied it to the clipboard.

### Add New Page Content
#### Methods:
- `document.createElement()`
- `document.createTextNode()`
- `Node.appendChild(childNode)`

#### Insert HTML (Last Child) - `.appendChild()`
  - Must be called on another Element.
  - Adds the specified childNode argument as the last child to the current node. (Add an element to the page)
  - If an element already exists in the DOM and this element is passed to `.appendChild()`, the `.appendChild()` method will move it rather than duplicating it

#### Example - `.appendChild()`
```
const mainHeading = document.querySelector('#main-heading');
const otherHeading = document.querySelector('#other-heading');
const excitedText = document.createElement('span');

excitedText.textContent = '!!!';
mainHeading.appendChild(excitedText);
otherHeading.appendChild(excitedText);  // '!!!' will move here.
```

- It **MUST** be the **LAST CHILD**
- The `.appendChild()` method will move an element from its current position to the new position.

#### Insert HTML (Elsewhere) - `element.insertAdjacentHTML(position, text)`
The .insertAdjacentHTML() method has to be called with two arguments:
1. The location of the HTML
2. The HTML text that is going to be inserted

The first argument to this method will let us insert the new HTML in one of four different locations
- `beforebegin` – inserts the HTML text as a previous sibling
- `afterbegin` – inserts the HTML text as the first child
- `beforeend` – inserts the HTML text as the last child
- `afterend` – inserts the HTML text as a following sibling

A visual example works best, and MDN's documentation has a fantastic example that I'll modify slightly:
```
<!-- beforebegin -->
<p>
    <!-- afterbegin -->
    Existing text/HTML content
    <!-- beforeend -->
</p>
<!-- afterend -->
```

#### Example - `.insertAdjacentHTML()`
```
const mainHeading = document.querySelector('#main-heading');
const htmlTextToAdd = '<h2>Skydiving is fun!</h2>';

mainHeading.insertAdjacentHTML('afterend', htmlTextToAdd);
```

### Remove Page Content
- `Node.removeChild()`:  **Must be called on the parent** 
- `ChildNode.remove()`: Called **directly on the element to delete.**

#### Example - Remove
```
const mainHeading = document.querySelector('h1');
mainHeading.remove();
```

We also learned about the following helpful properties:
- `.firstChild`: Might return whitespace (if there is any)
- `.firstElementChild`: Will always return the first element
- `.parentElement`

### Style Page Content

#### Adding Multiple Styles at Once
`HTMLElement.style`

##### Example - `.style.property`: One at a time
```
const mainHeading = document.querySelector('h1');

mainHeading.style.color = 'blue';
mainHeading.style.backgroundColor = 'orange';
mainHeading.style.fontSize = '3.5em';
```

##### Example - `.style.cssText`: Multiple
```
const mainHeading = document.querySelector('h1');
mainHeading.style.cssText = 'color: blue; background-color: orange; font-size: 3.5em';
```

#### Setting An Element's Attributes
`Element.setAttribute(name, value)`

Sets the value of an attribute on the specified element. If the attribute already exists, the value is updated; otherwise a new attribute is added with the specified name and value.

#### Accessing an Element's Classes
`Element.classList`: returns a **live DOMTokenList collection** of the class attributes of the element. This can then be used to manipulate the class list.

Using classList is a convenient alternative to accessing an element's list of classes as a space-delimited string via `element.className.`

1. HTML
`<h1 id="main-heading" class="ank-student jpk-modal">Learn Web Development at Udacity</h1>`

2. Retrieve Class List
```
const mainHeading = document.querySelector('#main-heading');

// store the list of classes in a variable
const listOfClasses = mainHeading.classList;

// logs out ["ank-student", "jpk-modal"]
console.log(listOfClasses);
```

A `DOMTokenList` representing the contents of the element's class attribute. If the class attribute is not set or empty, it returns an empty DOMTokenList, i.e. a DOMTokenList with the length property equal to 0.

The `.classList` property has a number of properties of its own. Some of the most popularly used ones are:
- `.add()` - to add a class to the list
- `.remove()` - to remove a class from the list
- `.toggle()` - to add the class if it doesn't exists or remove it from the list if it does already exist
- `.contains()` - returns a boolean based on if the class exists in the list or not

## Working with Browser Events
The Chrome browser has a special `monitorEvents()` and `unmonitorEvents()` function that will let us see different events as they are occurring. This should be used for developmental/testing purposed only.
See [Monitor Events](https://developers.google.com/web/tools/chrome-devtools/console/events#monitor_events)

```
// start displaying all events on the document object
monitorEvents(document);

// turn off the displaying of all events on the document object.
unmonitorEvents(document);
```

### The Event Target
The `EventTarget` page says that `EventTarget`:
- is an interface implemented by objects that can receive events and may have listeners for them.
- `Element`, `document`, and `window` are the most common event targets, but other objects can be event targets too…

Each of the following is an "event target":
- the document object
- a paragraph element
- a video element
- etc.

The EventTarget Interface doesn't have any properties and only three methods! These methods are:
- `.addEventListener()`
- `.removeEventListener()`
- `.dispatchEvent()`

#### addEventListener
**Syntax:** `target.addEventListener(type, listener [, options])`

##### So an event listener needs three things:
1. An event target - this is called the target
2. The type of event to listen for - this is called the type
3. A function to run when the event occurs - this is called the listener

##### Example Use of an Event Listener:
```
const mainHeading = document.querySelector('h1');

mainHeading.addEventListener('click', function () {
  console.log('The heading was clicked!');
});
```
See [list of Events](https://developer.mozilla.org/en-US/docs/Web/Events)

#### removeEventListener
**Syntax:** `target.removeEventListener(type, listener[, options])`

The listener function must be the exact same function as the one used in the .addEventListener() call...not just an identical looking function.

#### Example Add/Remove Event Listener
This code will **SUCCESSFULLY** add and then remove an event listener:
```
function myEventListeningFunction() {
    console.log('howdy');
}

// adds a listener for clicks, to run the `myEventListeningFunction` function
document.addEventListener('click', myEventListeningFunction);

// immediately removes the click listener that should run the `myEventListeningFunction` function
document.removeEventListener('click', myEventListeningFunction);
```

This cose will UNSUCCESSFULLY add and then remove an event listener:
```
// adds a listener for clicks, to run the `myEventListeningFunction` function
document.addEventListener('click', function myEventListeningFunction() {
    console.log('howdy');
});

// immediately removes the click listener that should run the `myEventListeningFunction` function
document.removeEventListener('click', function myEventListeningFunction() {
    console.log('howdy');
});
```

This code does not successfully remove the event listener. Again, why does this not work?

1. both `.addEventListener()` and .removeEventListener have the same target
2. both `.addEventListener()` and .removeEventListener have the same type
3. `.addEventListener()` and `.removeEventListener` have their own distinct listener functions...they do not refer to the exact same function (this is the reason the event listener removal fails!)

### Event Phases
There are three different phases during the lifecycle of an event. They are:
1. the capturing phase
2. the at target phase
3. and the bubbling phase

- Most Event Handlers run during the Target Phase.
- Bubbling is useful for lists (e.g. one click handler bubbles up from each `<li>` element to the `<ul>`, `<body>` back up to `<html>`).
- Capturing, on the other hand, lets the parent intercept an event before it reaches a child (e.g. `<html>`, `<body>` then firing at `<ul>` before it reaches `<li>` where it changes to `Target Phase` before bubbling back up the chain).

By default, when `.addEventListener()` is called with only two arguments, the method defaults to using the bubbling phase.

The code below uses `.addEventListener()` with only **two arguments**, so it will **invoke the listener during the bubbling phase:**

#### Example - Event Handler Fires During Bubbling Phase
```
document.addEventListener('click', function () {
   console.log('The document was clicked');
});
```

However, in this code, `.addEventListener()` is called with **three arguments** with the **third argument being true** (meaning it should invoke the listener earlier, during the **capturing phase!**).

#### Example - Event Handler Fires During Capturing Phase
```
document.addEventListener('click', function () {
   console.log('The document was clicked');
}, true);
```




