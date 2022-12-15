# Function

We've already looked over how to call functions, but we don't know how to define a function yet. This lesson will go over how to define functions and some important things to know about working with them.

## Defining Functions

The following code creates an empty function named `f` that does nothing:

```js
function f() {}
```

This is the template for a function declaration. We can now put other code inside. For example, let's make a function to show a greeting:

```js
function greet() {
    console.log("Hello!");
}
```

On its own, this code does nothing, but if we put `greet();` below, then it will print `Hello!` to the screen. This is because we call the `greet` function, which runs the code contained within the `greet` definition. This can be useful for code you repeat often - if we need to send another greeting, we can just call `greet();` again, and `greet` can be as complex as needed and the call will still just be one simple expression.

---

We can also make our function take arguments, which will be stored into function-scoped variables supplied by the caller:

```js
function greet(username) {
    console.log(`Hello, ${username}!`);
}
```

Recall that <code>\`...\`</code> is a _template string_ where the contents of `${...}` are evaluated as JavaScript code and inserted into the string. Thus, <code>\`Hello, ${username}!\`</code> inserts the value of `username`, which is defined in the function based on what the caller supplies, into the string.

If we call `greet("Steve");`, we get `Hello, Steve!` on the screen. Within the function, `username` is set to `"Steve"`. If we call `greet();`, we get `Hello, undefined!`, because in JavaScript, supplying fewer arguments to a function that required results in the last few becoming `undefined` (and not an error). Finally, just as a reminder, if we do `greet("Bob", "Steve");`, we get `Hello, Bob!` and no errors.

---

Functions can take multiple arguments:

```js
function greet(sender, receiver) {
    console.log(`${sender} says hello to ${receiver}!`);
}
```

In this case, `greet("Bob", "Steve");` will display `Bob says hello to Steve!`. `greet();` will now say `undefined says hello to undefined!`. If we want to make sure that we aren't getting undefined values, we can use `assert`, which is a function that takes one argument and does nothing if it's truthy and errors if it's falsy:

```js
function greet(sender, receiver) {
    assert(sender !== undefined && receiver !== undefined);
    console.log(`${sender} says hello to ${receiver}!`);
}
```

Now, `greet();` or `greet("Steve");` will both throw an error. We'll talk more later about how errors work and ways of handling them.

---

If you want your function to evaluate to a value and not result in `undefined`, you will need to `return` it. `return` is a statement that is optionally followed by a value and when the function is called, it will evaluate to the `return` value.

```js
function add(x, y) {
    return x + y;
}
```

Doing `return;` is the same as not having it, and it will make the function evaluate to `undefined`. However, note that `return` does not just set the value to be returned on exit, it also exits the function, so if we put `console.log("test");` on the line below `return x + y;`, it will be skipped.

`return;` also exits immediately, so this can be used to control the flow of the program. For now, since we have not learned about control flow yet, we can't really use this, but we will discuss this in a few lessons.

---

How does `console.log` accept arbitrarily many arguments? Within a function, `arguments` is initialized to the list of all arguments. It is not actually an array but an `Arguments` object. However, it works basically the same way as an array, and you can iterate through it and access its elements. For example:

```js
function f() {
    console.log(arguments.length);
    console.log(arguments[0]);
    console.log(arguments[1]);
}

f(1, 2, 3, 4, 5); // 5, 1, 2
```

---

The above can also be done using a "rest parameter" (a parameter that takes the rest of the values):

```js
function f(...x) {
    console.log(x.length);
    console.log(x[0]);
    console.log(x[1]);
}
```

This function works identically to the above. In this case, `x` is just an array, so we can do the following:

```js
function f(...x) {
    x.forEach(console.log);
}

f(1, 2, 3, 4);
/*
1 0 [1, 2, 3, 4]
2 1 [1, 2, 3, 4]
3 2 [1, 2, 3, 4]
4 3 [1, 2, 3, 4]
*/
```

### Default Arguments

You can also specify default arguments, which are assigned if the caller does not supply enough arguments (if the caller supplies `undefined`, the value will stll be `undefined`):

```js
function add(x, y = 1) {
    return x + y;
}

add(1, 2); // 3
add(1); // 2
add(1, undefined); // NaN
```

The default value is not evaluated unless it is needed, and it is re-evaluated each time, so between calls, the default value is different:

```js
function f(x = []) {
    x.push(1);
    console.log(x);
}

f(); // [1]
f(); // [1]
```

Rest parameters cannot have default initializations.

### Hoisting

Functions defined this way are function-scoped and the existence of the value is hoisted to the top, but the actual function is hoisted to the top of the block. So for example:

```js
function f() {
    g(); // error, because `g` is `undefined` here
    {
        g(); // works, because `g` is hoisted to the top of the block
        function g() {}
        g(); // works, because `g` is now defined here
    }
    g(); // works, because `g` is defined within function scope
}

f();
g(); // errors, because `g` stops existing outside of `f`'s scope
```

This differs from `var`'s hoisting because the value is hoisted up, so you can use functions _before_ you define them.

## Anonymous Functions

The function name is optional. If you leave it out, it will evaluate to a function, which you can either pass directly to something else, or assign to a variable. For example:

```js
[1, 2, 3].forEach(function (x) {
    console.log(x);
});
```

This will output `1`, then `2`, then `3` (as opposed to `1 0 [1, 2, 3]` then `2 1 [1, 2, 3]` then `3 2 [1, 2, 3]`, which happens because `console.log` takes all three arguments given by `forEach`).

You _can_ specify the name here, but it will not be defined anywhere, so it is useless in this situation.

You can also assign it to a variable:

```js
let greet = function (name) {
    console.log(`Hello, ${name}!`);
};

greet("Steve"); // Hello, Steve!
```

This may be useful for controlling the scope. Recall that the default function declaration is function-scoped and the value is hoisted to the top of the block.

---

```js
let x = function f() {};
```

Given this code, `x` will be defined as a function named `f`, but `f` will be undefined. This is slightly strange, but this is the result:

```js
let x = function f() {};
console.log(x); // [Function: f]
console.log(f); // ReferenceError: f is not defined
```

There is little reason to do this.

## Lambdas (Arrow Functions)

The name "lambda" comes from other parts of computing, but these are more commonly referred to as arrow functions in JavaScript because of the use of `=>`.

```js
let greet = (name) => console.log(`Hello, ${name}!`);
```

Note that these may not work on very outdated systems, but they are not particularly new anymore so you generally should not worry about that.

Here, <code>(name) => console.log(\`Hello, ${name}!\`);</code> evaluates to an anonymous (arrow) function that takes an argument and outputs a greeting. Assigning it to `greet` names it `greet` (otherwise it would be called `[Function (anonymous)]`). In this situation, the part before `=>` is the exact same argument syntax as a normal function's argument list, including default initialization and rest parameters. If there is exactly one argument, the brackets can be excluded, so the above example could be `name => ...`.

Within an arrow function, `arguments` is not defined, so we need to use rest parameters instead.

---

We also do not have to name these:

```js
[1, 2, 3, 4].forEach((x) => console.log(x));

/*
1
2
3
4
*/
```

---

We can also use a block to write multiple statements in the arrow function:

```js
[1, 2, 3].forEach((x, y) => {
    console.log(x);
    console.log(y);
});

/*
1
0
2
1
3
2
*/
```

Within these, we can also use `return` statements:

```js
let f = (x, y) => {
    return x + y;
};

f(1, 2); // 3
```

---

However, the above can be shortened. If there is only one expression and it is returned immediately, we can drop the block and leave out the `return`:

```js
let f = (x, y) => x + y;
f(1, 2); // 3
```

This will return the `x + y`. If you want to call another function but not return its value, the easiest way would be to just bring back the `{ }` and not use `return`.

---

This lesson covered how to define functions, their mechanisms with scoping and hoisting, anonymous functions, and arrow functions. We'll be using functions a lot in the future, so if it doesn't fully make sense now, it will sooner or later. Next time, we'll cover defining custom objects as well as destructuring assignment, which will continue a bit on functions.