# Introduction

This tutorial will teach you the basics of Node.JS as well as some more advanced concepts, tips, and tricks. Throughout this tutorial, we will make three projects - a number guessing game, a simple calculator app, and a tic-tac-toe game.

## Setup

### Downloading Node.JS

You will need to install node on your computer first. You can download it on the official website at https://nodejs.org/en/download/ or follow the tutorial at https://nodejs.dev/learn/how-to-install-nodejs.

Now, open up your terminal. Running `node` should bring up a REPL. Try typing `1 + 2` and hitting <kbd>Enter</kbd>. You should see `3` appear below. Now, hit <kbd>Ctrl</kbd> + <kbd>D</kbd> and the repl should close (alternatively, you can run `process.exit(0)` for the programmatic method of exiting).

You should have also gotten `npm` (Node Package Manager). You will need this to install libraries. Try `npm --version` to make sure that it works; if it doesn't, try going through the instructions at https://docs.npmjs.com/downloading-and-installing-node-js-and-npm.

### Setting up an environment

I personally use VSCode. You can use any editor and should stick with what you're comfortable with, but if you don't have anything yet, I would recommend VSCode.

Create a new workspace in a new folder for following along with this tutorial. This can be a git repository if you're familiar with how to use git, or just a normal folder.

To begin, open your console in this folder. On Windows, you can right click in File Explorer inside the folder and click "Open Terminal Here" (if it doesn't appear, try <kbd>Shift</kbd> + right click). Other operating systems probably have a similar option, or you can just navigate there using `cd`.

Let's start by creating a file in your workspace folder called `hello.js`. Enter `console.log("Hello, World!");` into your file and save.

Now, go back to your terminal and run `node hello.js`. You should see `Hello, World!` appear in the console.

## Asynchronous Programming

An important concept to understand for JavaScript is asynchronous programming. As a massive oversimiplification, asynchronous programming is where you are able to run multiple things at once via time-sharing, where the processor switches between running multiple things to give the illusion that they are happening at the same time. Since JavaScript was originally made for websites, this is a very important concept as it would be incredibly bad if every action the user performed caused everything else to freeze until it was finished. This way, if the user clicks a button and your website needs to fetch another web resource, things like animations can keep playing while it waits instead of everything halting until the resource is fetched.

In JavaScript, this is done with something known as `Promise`s. A `Promise` is an object that indicates the computation is still in progress and that it will eventually return a response, whether it is the answer or an error. Note that despite being called a "promise", there is technically no guarantee that it will ever resolve, but usually this doesn't happen and you typically shouldn't worry about handling this situation.

There are four things you can do with `Promise` objects. If you call `.then(...)` on it with a function, that function will be run on the result when it returns a result. If you call `.catch(...)` on it with a function, that function will be run on the error if the `Promise` resolves unsuccessfully. You can also just pass it on directly to something else - a `Promise`, despite being an indication that the result is still being computed, is an object itself and can be treated as such. Finally, you can use `await` on it. We will go into this right now.

More recently in JavaScript, instead of nesting `.then()` calls, you can just use `await`. This must be done either at the top level or within an `async` function. It essentially converts asynchronous programming back into _synchronous_ programming (where actions wait for each other) by waiting for the promise to resolve first. Here's a basic example:

```js
function f() {
    fetch_web_resource().then(function (result) {
        console.log(result);
    });
}

async function f() {
    console.log(await fetch_web_resource());
}
```

The two functions are essentially the same. The only difference is that the first one will exit immediately, because it is not waiting for `fetch_web_resource()`, whereas the second will wait for it to finish before exiting. The second function also returns a `Promise` - you can return promise objects manually, but `async` functions will also return `Promise`s.

For example, in the following code:

```js
async function f() {
    return 1;
}

console.log(f());
```

Even though `f()` just returns 1 immediately and isn't actually waiting for anything, because it is declared as an `async` function, you will see the output be a `Promise` object instead of `1`.

When you `await` a `Promise`, if it resolves unsuccessfully, it will just throw the error directly, so you would handle it using `try`/`catch`, which we will discuss later.

To see why `async`/`await` is so important, just consider the following code:

```js
async function() {
    await f();
    await g();
    await h();
}
```

Using `.then` calls, this would be:

```js
function() {
    f().then(function(a) {
        g().then(function(b) {
            h().then(function(c) {
                ...
            })
        })
    })
}
```

## Basic JavaScript fundamentals

JavaScript is dynamically typed, meaning variables don't have a fixed type, and the type of objects is determined at runtime. Note that if you want compile-type static-typing, you can use TypeScript, which is very similar to JavaScript except it support type declarations. This tutorial will not cover TS.

JavaScript uses C-style braces and optional semicolons. I recommend putting semicolons at the end of lines. By optional, I mean that the interpreter will automatically determine where statements end, but it does not always behave how you would expect and you can run into bugs that are very difficult to identify if this happens.

### Value Types

In JavaScript, you will mostly be working with the following types:

- integers - whole numbers, which do have limited precision/range unlike Python
- floats - floating-point numbers, which have limited precision like almost all other languages
- strings - strings of characters, so just text
- arrays - an ordered and mutable collection of other objects
- others - you will probably run into other types like dates and maps

There are two special values, `null` and `undefined`. They both represent the absence of a value but they do slightly differ; `null` roughly means "this value is defined to be absent", whereas `undefined` means, as the name implies, "the definition of this value is missing". Thus, you should use `null` to indicate an object is intentionally blank, whereas if you attempt to refer to a property of an object that does not exist, it will give you `undefined`.

### Operators

JavaScript's operators are mostly logical and similar to other languages, so if you've programmed before it should be fairly intuitive.

One set of operators that is fairly uncommon that JavaScript has is null-related operators. We will go over these at the end as they are more complicated.

Here is a table of the full list of operators and how to use them:

|   Operator   |                                                         Description                                                         |
| :----------: | :-------------------------------------------------------------------------------------------------------------------------: |
|     `+`      |                         Addition (can concatenate strings including adding them with other objects)                         |
|     `-`      |                                                         Subtraction                                                         |
|     `*`      |                            Multiplication (cannot repeat strings, use `"...".repeat(x)` instead)                            |
|     `**`     |                                              Exponentiation (added in ES 2016)                                              |
|     `/`      |                                                          Division                                                           |
|     `%`      |                                                     Modulus (Remainder)                                                     |
|     `++`     | Increment (`++x` and `x++` both increase `x` by one; the former returns the new value and the latter returns the old value) |
|     `--`     |                                      Decrement (same as above but decrease `x` by one)                                      |
|     `==`     |               Equal To (note: this will convert between types sometimes unexpectedly; `0 == ""` for example)                |
|    `===`     |                Equal Value and Equal Type (it is not the case that `0 === ""`, so be careful which you use)                 |
|     `!=`     |                                               Not Equal To (opposite of `==`)                                               |
|    `!==`     |                                     Not Equal Value and Equal Type (opposite of `===`)                                      |
|     `>`      |                                                        Greater Than                                                         |
|     `<`      |                                                          Less Than                                                          |
|     `>=`     |                                                  Greater Than or Equal To                                                   |
|     `<=`     |                                                    Less Than or Equal To                                                    |
|    `? :`     |                  Ternary Operator; `a ? b : c` evaluates to `b` if `a` is a truthy value and `c` otherwise                  |
|     `&&`     |                               Logical AND; `a && b` is `a` if `a` is falsy and `b` otherwise                                |
|   `\|\| `    |                                 Logical OR; `a \|\| b`is`a`if`a`is truthy and`b` otherwise                                  |
|     `!`      |                          Logical NOT; `!a` is `false` if `a` is truthy and `true` if `a` is falsy                           |
|   `typeof`   |                                          `typeof a` returns `a`'s type as a string                                          |
| `instanceof` |                                  `a instanceof B` returns whether or not `a`'s type is `B`                                  |
|     `&`      |                                                         Bitwise AND                                                         |
|     `\|`     |                                                         Bitwise OR                                                          |
|     `~`      |                                                         Bitwise NOT                                                         |
|     `^`      |                                                         Bitwise XOR                                                         |
|     `<<`     |                                                         Left Shift                                                          |
|     `>>`     |                                                         Right Shift                                                         |
|    `>>>`     |                                                    Unsigned Right Shift                                                     |
|     `[]`     |                                               Index Access (`object[index]`)                                                |
|     `.`      |                                                Field Access (`object.field`)                                                |

Bitwise operators are not too commonly used, but are still important to know. This tutorial will not cover bit arithmetic, but the W3Schools page on JavaScript operators has some good information on them: https://www.w3schools.com/js/js_bitwise.asp.

Also, `[]` and `.` are not really operators. `x[y]` will return the `y`th element of `x` (note that `x[0]` is the first element), or can be used to access properties of objects; for example, `x["name"]` is the same as `x.name` in JavaScript. `.` access a property, so if `x` has a `name` property, `x.name` refers to it.

Two operators that are somewhat distinct in JavaScript are `??` and `?.`. Recall that `null` and `undefined` are special values indicating the lack of a value. Let's say you have a variable `x` and want to return `3` if it's missing. You could do `x || 3` as `null` and `undefined` are considered falsy; however, if `x` is `0` then this would also return `3` even though `x` is not undefined.

This is where `??` comes into play - `x ?? 3` would do what is wanted. `x ?? y` returns `y` if `x` is `null` or `undefined`, and `x` otherwise, essentially allowing you to define a fallback for if `x` is absent.

`?.` is somewhat similar. If you go into your `node` terminal and type `"hello".name`, you will notice it gives `undefined` instead of an error. This is intended behavior of JavaScript. However, if you do `null.name` or `undefined.name` you will get an error (`Uncaught TypeError: Cannot read properties of null/undefined`). To avoid this error, you can instead do `null?.name`. Instead of an error, this returns `undefined` as well.

Thus, `x?.y` essentially means "return `x.y` if `x` exists, and `undefined` otherwise". Normally, `x.y` will error if `x` is `null` or `undefined`, but this allows you to avoid that. Note that you do not need to repeat this if you are accessing a nested property; for example, instead of `x?.parent?.parent?.name`, you would only need to do `x?.parent.parent.name`. If `x` is `null` or `undefined`, the `?.` will cause the rest of the chain to be ignored and it will just return `undefined`. Be careful though - if `x` is defined but `x.parent` might not be, you would still need `x?.parent?.parent.name`, and if `x.parent.parent` might be undefined, you would need to use `?.` in all three places.

Finally, although this syntax is a bit weird, you can also use `?.` to safely attempt to call a function or index an array. `x?.[y]` returns `x[y]` if `x` is defined and `undefined` otherwise. `x?.(...)` calls `x` as a function, returning `x(...)` if `x` is defined and `undefined` otherwise. Do beware that if `x` exists but is not a function, `x?.()` will still error.

## Conclusion

In today's tutorial, we went over how to install Node.JS and then some basics with asynchronous programming, JavaScript's value types, and the list of operators. Next time, we will start writing some actual code and looking into defining variables and how to use functions.

:::info
test
:::
