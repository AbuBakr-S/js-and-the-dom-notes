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

## Example
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
