# Variables and Scope

This lesson covers variables, which are named values that you can call functions on and manipulate, and scopes, which dictate in what section of code a variable lives. This will be a bit of a shorter lesson but its contents are extremely important and it is crucial that you have a good understanding of these concepts.

## Variables

Vaguely speaking, a variable is a named container that can hold a value. To use a variable, you first need to declare it, thereby creating the box:

```js
var x; // creates a box called "x"
let y; // creates a box called "y"
```

We'll talk about the difference between `var` and `let` later. Once we have a box, we can put items into it:

```js
x = 3; // the "x" box now contains 3
y = "Hello"; // the "y" box now contains "Hello"
```

We can also switch what item is in it:

```js
x = [1, 2, 3]; // the "x" box now contains [1, 2, 3] and the 3 is gone
```

Then, we can use its name like any other value, both passing it into other things and manipulating it directly:

```js
console.log(x); // prints [1, 2, 3]
x.push(4); // x is now [1, 2, 3, 4]
```
Before we talk about what the difference between `let` and `var` are and also introduce `const` and mention declaring variables implicitly, we need to talk about scope.

## Scope

Speaking from a high level, the scope is just the section of code in which a variable lives or is defined. JavaScript has three types of scope, _block scope_, _function scope_, and _global scope_. Block scope is the immediate surround `{ ... }`. We'll talk about the usage of blocks in things like if statements more next time, but just to demonstrate scope, take a look at this example:

```js
// code 1

{
    // code 2
}

{
    // code 3
}

// code 4
```

Here, `code 1` and `code 4` are in the global scope, and `code 2` and `code 3` are in separate block scopes. We'll discuss functions in more depth later on but all you need to know for now is that `function func_name() { ... }` defines a function:

```js
// code 1

function f() {
    // code 2

    {
        // code 3
    }
}

{
    // code 4
}
```

Here, `code 1` is in the global scope, `code 4` is in a block scope, `code 2` is in function scope, and `code 3` is in a block scope contained within a function scope.

Now, we can talk about the types of declarations.

## Types of Variable Declarations

Let's begin with `var`. It is initialized to the innermost function scope, or if there is none, the global scope. So, in the following example:

```js
{
    var a = 3;
}

console.log(a);
```

The above will print `3`.

```js
function f() {
    {
        var a = 3;
    }

    console.log(a);
}

f();
console.log(a);
```

However, in the above, when `f` is called, the first `console.log` will print `3`. However, in the second one on the last line, that would give an error as `a` is constrained to its function scope, `f`.

`let` is similar, but it is constrained to the block scope. So, if we replaced `var` with `let` in the above two examples, all of the `console.log`s would fail, because `a` only lives within a scope that the `console.log` calls are not in.

I recommend `let`. `var` can be confusing, and if you want to intentionally have a variable be function-scoped, you can declare it at the top of the function with `let`.

`const` is just like `let` but you cannot change the value of the variable. You can still mutate the value itself; for example, `const x = [1, 2, 3]; x.push(4);` is valid, but `const x = 1; x = 2;` is not. You also cannot declare `const`s with no value (such as `const x;`); they must have a value, even if you do `const x = undefined;`.

Finally, if you assign a variable that was not previously defined, it just automatically goes to the global scope. So, in the following example, all of the `console.log`s will pass:

```js
function f() {
    {
        a = 3;
        console.log(a);
    }

    console.log(a);
}

console.log(a);
```

You should absolutely not do this as it can cause weird bugs and does not clearly show what your code is supposed to do. Use top-level `let` statements instead. Also, in strict mode (which we will not cover), this errors instead.

## Hoisting

Another reason not to use `var` is that it _hoists_ variables, meaning they exist _before_ `var`. Within the scope that a variable defined by `var` exists in, that variable will exist and be `undefined` until it is assigned. For example:

```js
{
    console.log(a); // undefined
    var a = 3;
    console.log(a); // 3
}
```

If you are using a variable before declaring it, that is probably a bug, and you want to get an error message for that. If you used `let` instead, you would get `ReferenceError: cannot access 'a' before initialization`. This is different from `ReferenceError: a is not defined`, which you would get if you tried to use a variable that doesn't exist. This is because `let` and `const` _also_ hoist, but in their scope before the declaration, instead of being `undefined` like with `var`, they are blocked. The following also fails:

```js
{
    a = 3; // cannot set `a` before its initialization
    let a;
}
```

As does this:

```js
{
    console.log(a); // cannot read `a` before its declaration
    const a = 3;
}
```

This way, if you try using a variable before it exists, you get a very clear error.

---

As a summary of variables, they are basically named boxes that can contain any value, and you can put values into them and access them by name. This means that you can keep track of a changing (or constant) value throughout your program; for example, to keep a count of the number of times an action has happened, or to store the intermediate results of some computation. You went over `var`, `let`, `const`, and implicit declaration, and their differences. Generally, you should only ever use `let` and `const`; if you want a variable to be in the function or global scope, just put the `let` declaration there.

Next time, we will talk about operators, which allow us to perform basic computations with these values.