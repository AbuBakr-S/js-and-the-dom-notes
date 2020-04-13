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
 
### Key Terms
**Note:** *Our use of "interface" is not related to either a UI or a GUI. Our use of `interface` is a technical, computer science word for a **list of properties and methods that are inherited**.*

- interface = blueprint
- properties = data
- methods = functionality

