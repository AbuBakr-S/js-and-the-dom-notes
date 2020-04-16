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

### The Event Object
When an event occurs, the browser includes an `event object`. This is just a regular JavaScript object that includes a ton of information about the event itself. The `.addEventListener()`'s listener function receives a notification (an object that implements the Event interface) when an event of the specified type occurs.

Let's add a parameter so we can store this important information:
```
document.addEventListener('click', function (event) {  // ← the `event` parameter is new!
   console.log('The document was clicked');
});
```

Notice the new event parameter that's been added to the listener function. Now when the listener function is called, it is able to store the event data that's passed to it!

### Default Action and Preventing

#### Example using `Event.preventDefault()`:
```
const links = document.querySelectorAll('a');
const thirdLink = links[2];

thirdLink.addEventListener('click', function (event) {
    event.preventDefault();
    console.log("Look, ma! We didn't navigate to a new page!");
});
```

### Avoid Too Many Events
- Separate functions from loops (Single Function).
- Append Element inside the loop, with each iteration, to a Variable stored outside.
- Add Event Listener to the Variable (Single Event Listener).
- Append the Variable to the Document

```
const myCustomDiv = document.createElement('div');

function respondToTheClick() {
    console.log('A paragraph was clicked.');
}

for (let i = 1; i <= 200; i++) {
    const newElement = document.createElement('p');
    newElement.textContent = 'This is paragraph number ' + i;

    myCustomDiv.appendChild(newElement);
}

myCustomDiv.addEventListener('click', respondToTheClick);

document.body.appendChild(myCustomDiv);
```

However, we've lost access to the individual paragraphs.

### Event Delegation
Event Delegation is the process of delegating to a parent element the ability to manage events for child elements. We are able to do this by making use of:

- the event object and its .target property
- the different phases of an event

1. a paragraph element is clicked
2. the event goes through the capturing phase
3. it reaches the target
4. it switches to the bubbling phase and starts going up the DOM tree
5. when it hits the `<div>` element, it runs the listener function
6. inside the listener function, `event.target` is the element that was clicked

So `event.target` gives us direct access to the paragraph element that was clicked. Because we have access to the element directly, we can access its .textContent, modify its styles, update the classes it has - we can do anything we want to it!

```
const myCustomDiv = document.createElement('div');

function respondToTheClick(evt) {
    console.log('A paragraph was clicked: ' + evt.target.textContent);
}

for (let i = 1; i <= 200; i++) {
    const newElement = document.createElement('p');
    newElement.textContent = 'This is paragraph number ' + i;

    myCustomDiv.appendChild(newElement);
}

document.body.appendChild(myCustomDiv);

myCustomDiv.addEventListener('click', respondToTheClick);
```

#### Checking the Node Type in Event Delegation
- The above works perfectly as the `<p>` tags are direct children of `<div>`
- However there is nothing to ensure that it was actually a <p> tag that was clicked
- We can use `Node.nodeName` to verify the Node Type is as expected
  
##### Example of More Complex HTML with Nested Nodes    
```
<article id="content">
  <p>Brownie lollipop <span>carrot cake</span> gummies lemon drops sweet roll dessert tiramisu. Pudding muffin <span>cotton candy</span> croissant fruitcake tootsie roll. Jelly jujubes brownie. Marshmallow jujubes topping sugar plum jelly jujubes chocolate.</p>

  <p>Tart bonbon soufflé gummi bears. Donut marshmallow <span>gingerbread cupcake</span> macaroon jujubes muffin. Soufflé candy caramels tootsie roll powder sweet roll brownie <span>apple pie</span> gummies. Fruitcake danish chocolate tootsie roll macaroon.</p>
</article>
```

##### Example of an Unsuccessful Event listener
```
document.querySelector('#content').addEventListener('click', function (evt) {
    console.log('A span was clicked with text ' + evt.target.textContent);
});
```

##### Example of a Successful Event Listener - Using `.nodeName`
```
document.querySelector('#content').addEventListener('click', function (evt) {
    if (evt.target.nodeName === 'SPAN') {  // ← verifies target is desired element
        console.log('A span was clicked with text ' + evt.target.textContent);
    }
});
```

Remember that every element inherits properties from the Node Interface. One of the properties of the Node Interface that is inherited is .nodeName. We can use this property to verify that the target element is actually the element we're looking for. When a <span> element is clicked, it will have a .nodeName property of "SPAN", so the check will pass and the message will be logged. However, if a <p> element is clicked, it will have a .nodeName property of "P", so the check will fail and the message will not be logged.
  
#### The nodeName's Capitalization
The `.nodeName` property will return a capital string, not a lowercase one. So when you perform your check make sure to either:
- check for capital letters
- convert the .nodeName to lowercase

##### CAPS
```
// check using capital letters
if (evt.target.nodeName === 'SPAN') {
    console.log('A span was clicked with text ' + evt.target.textContent);
}
```

##### Lowercase:
```
// convert nodeName to lowercase
if (evt.target.nodeName.toLowerCase() === 'span') {
    console.log('A span was clicked with text ' + evt.target.textContent);
}
```

### Know When the DOM is Ready

#### The DOM is Built Incrementally
When the HTML is received and converted into tokens and built into the document object model, is that this is a sequential process. When the parser gets to a `<script>` tag, it must **wait to download the script file and execute that JavaScript code.** This is the important part and the key to why the placement of the JavaScript file matters!

#### Issues with JS Code in the `<head>`
##### Example of Calling a Node in JS that hasn't been Parsed by the DOM
```
<!DOCTYPE html>
<html lang="en">
<head>
  <link rel="stylesheet" href="/css/styles.css" />
  <script>
    document.querySelector('footer').style.backgroundColor = 'purple';  // This will return null
  </script>
```

- This is like running: `null.style.backgroundColor = 'purple';`
- `null` doesn't have a .style property, so thus our error is born.

##### There are two solutions to this:
1. Well, if the DOM is built sequentially and the JavaScript code is moved to the very bottom of the page, then by the time the JavaScript code is run, all DOM elements will already exist!
2. 
However, an alternative solution would be to use **browser events!**

### The Content Is Loaded Event - `DOMContentLoaded`
When the document object model has been fully loaded, the browser will fire an event. This event is called the `DOMContentLoaded` event, and we can listen for it the same way we listen to any other events:

```
document.addEventListener('DOMContentLoaded', function () {
    console.log('the DOM is ready to be interacted with!');
});
```

#### Resolving Issues with JS Code in the `<head>`
```
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="/css/styles.css" />
    <script>
      document.addEventListener('DOMContentLoaded', function () {
          document.querySelector('footer').style.backgroundColor = 'purple';
      });
    </script>
```

**Tip:**
Even though this works, it's best to move the code to the bottom of the HTML file just before the closing `</body>` tag.

**Tip:**
JavaScript code in the `<head>` will run before JavaScript code in the `<body>`, so if you do have JavaScript code that needs to run as soon as possible, then you could put that code in the <head> and wrap it in a DOMContentLoaded event listener. This way it will run as early as possible, but not too early that the DOM isn't ready for it.

**Tip:**
If you're looking at somebody else's code, you may see that their code listens for the load event being used instead (e.g. `document.onload(...))`. load fires later than `DOMContentLoaded` -- load waits until all of the images, stylesheets, etc. have been loaded (everything referenced by the HTML.) 

## Performance 

### Add Page Content Efficiently

#### Testing Code Performance - `performance.now()`
The standard way to measure how long it takes code to run is by using `performance.now()`. `performance.now()` returns a timestamp that is measured in milliseconds, so it's extremely accurate. How accurate? Here's what the its documentation page says: *accurate to five thousandths of a millisecond (5 microseconds)*

The browser's `performance.now()` method starts measuring from the time the page loaded.

Usage of `performance.now()`:
1. Use performance.now() to get the an initial start time for the code
2. Run the code you want to test
3. Execute performance.now() to get another time measurement
4. Subtract the initial time from the final time

#### Using a Document Fragment
- Creating an extaneous `<div>` elemeent just to hold all `<p>` tags is wasteful.
- The browser is constantly working to make the screen match the DOM. When we add a new element, the browser has to run through a reflow calculation (to determine the new screen layout) and then repaint the screen. This takes time.

##### A DocumentFragment:
- Represents a minimal document object that has **no parent**. It is used as a lightweight version of Document that stores a segment of a document structure comprised of nodes just like a standard document.
- As the DocFrag is not part of the active DOM, changes made to the fragment don't affect the document, cause reflow, or incur any performance impact that can occur when changes are made.

We can use the `.createDocumentFragment()` method to create an empty `DocumentFragment` object. 

`const myDocFrag = document.createDocumentFragment();`

##### Example Useage of A Document Fragment
```
const myDocFrag = document.createDocumentFragment(); 

for (let i = 1; i <= 200; i++){
    const newEl = document.createElement('p');
    newEl.textContent = "This is paragraph number " + i;
    myDocFrag.appendChild(newEl);
}
```

`document.body.appendChild(myDocFrag);`

### Reflow and Repaint
#### Reflow 
The process of the browser laying out the page. It happens when you first display the DOM (generally after the DOM and CSS have been loaded), and happens again every time something could change the layout. This is a fairly expensive (slow) process.

#### Repaint 
Happens after reflow as the browser draws the new layout to the screen. This is fairly quick, but you still want to limit how often it happens.

For example, if you **add a CSS class** to an element, the browser often recalculates the layout of the entire page—that's **one reflow and one repaint!**

- Hiding an Element that doesn't change layout (1 Repaint)
- Show a Hidden Element - (1 Reflow, 1 Repaint)
- **Hide, Change All, Show** is a good pattern to follow

### The Call Stack
#### Single Threading
- JavaScript can "process" one command at a time
- Run to completion nature
- All of the code in the function gets executed
- Function A gets invoked, completes, returns, then Function B gets invoked...

#### The Call Stack
JavaScript keeps track of what functions are running by using the Call Stack.





