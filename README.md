# learn-js
Learning material for JavaScript.

# Variables

## Hoisting
- Hoisting is a result of how JavaScript is interpreted by your browser. Essentially, before any JavaScript code is executed, all variables declared with var are "hoisted", which means they're raised to the top of the function scope.
- This is bad practice.

## let and const
- Variables declared with let and const are scoped to the block, not to the function. 
- With var, variables were either scoped globally or locally to an entire function scope.
- Use const as it's safer unless you need to reassign.

If a variable is declared using let or const inside a block of code (denoted by curly braces { }), then the variable is stuck in what is known as the **temporal dead zone** until the variable’s declaration is processed. This behavior prevents variables from being accessed only until after they’ve been declared.

Variables declared with let and const are only available within the block they're declared.

## Example: Variable scope
What do you expect to be the output from running `getClothing(false)`?
```
function getClothing(isCold) {
  if (isCold) {
    const freezing = 'Grab a jacket!';
  } else {
    const hot = 'It’s a shorts kind of day.';
    console.log(freezing);
  }
}
```
Console: `ReferenceError: freezing is not defined`

## Rules for using let and const
let and const also have some other interesting properties.

- Variables declared with let can be reassigned, but can’t be redeclared in the same scope.
- Variables declared with const must be assigned an initial value, but can’t be redeclared in the same scope, and can’t be reassigned.




# Destructuring
Destructuring borrows inspiration from languages like Perl and Python by allowing you to specify the elements you want to extract from an array or object on the left side of an assignment.

## Example: Destructuring values from an array
```
const point = [10, 25, -34];
const [x, y, z] = point;
console.log(x, y, z);
```
Console: `10 25 -34`

TIP: You can also ignore values when destructuring arrays. For example, const [x, , z] = point; ignores the y coordinate and discards it.

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
Console: `quartz rose 21.29`

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

Calling getArea() will return NaN. When you destructure the object and store the getArea() method into the getArea variable, it no longer has access to this in the circle object which results in an area that is NaN.

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
  calculateWorth() { ... }
};
```

# Loops

There are several loops you can use, each with their limitations:
- for loop
- for...in loop
- Most recently, for...of loop

## Strengths
- Loop over any type of data that is iterable, (meaning it follows the [iterable protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols))
- By default, this includes the data types String, Array, Map, and Set.
— Objects are not iterable, by default.

## Weaknesses
- Tracking the counter
- Tracking the exit condition
- The forEach loop is limited to Arrays

