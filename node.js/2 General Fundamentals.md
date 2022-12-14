# General Fundamentals

This lesson covers some core concepts and fundamentals in programming, and how they apply in JS. We will go over what values are and how to store them, what functions are and more about using them, and the concept of STDIO, which, although not actually as relevant in JS as some other languages, is still an important concept and a lot of knowledge about it carries over.

## Values

At its core, a value is just any data. A number, some text, a user's account - these are all values. Even your code itself is data. In JavaScript, there are 8 types of values:

-   `number` - encompasses the number primitive (note that there can be non-primitive numbers)
-   `bigint` - infinite precision integers
-   `string` - text primitives (note that there can be non-primitive strings)
-   `boolean` - `true` or `false`
-   `undefined` - a missing value
-   `symbol` - we won't go over this
-   `function` - functions are technically objects, which we will discuss more later
-   `object` - anything else

Let's go over some examples of all of them.

`3`, `3.5`, and `-4` are all examples of primitive numbers. Note that these are all floating-point numbers (formally, `double`s, but it is fine if you don't understand what that means), and have limited precision. For example, try `0.1 + 0.2` in your REPL. Funnily enough, `NaN`, the value meaning "not a number", is of type `number`.

`10n` is a BigInt. These only represent integers but have infinite precision by using a different storage method. The advantage is that you can have numbers of theoretically unbounded size. You cannot mix these with regular numbers during computation; you will either need to convert the other number to a bigint using `BigInt(x)`, which won't really work if it's not an integer, or converting the bigint back to a number primitive, which may lose precision if it's large.

`"Hello, World!"` from last time is a string. These represent text and are denoted either `"like so"` or `'like so'`. I generally prefer the former unless the contents of the string contain double-quotes.

`true` and `false` are the only two booleans. However, every other object can be considered "truthy" or "falsy" - `0`, `null`, `undefined`, `""`, `NaN`, and `false` are considered falsy and everything else is truthy.

`undefined` is a special value. If you try to access a non-existent property of an object, you will get `undefined`. Likewise, if you get the result of a function that didn't return any values, you get `undefined`.

`symbol` is a type with some more advanced use cases. They are beyond this tutorial and I have never used them personally. Check out [this page](https://javascript.info/symbol) if you would like to learn more; however, most of it will probably not make sense yet.

`console.log` is an example of a `function`. We'll go over what it entails for functions to be considered objects later, but for now just remember that it is also a type.

Finally, everything else is an `object`, including things like arrays, which are ordered collections of other values.

### More about strings

`""` and `''` strings cannot span multiple lines. How would you insert a newline in the string then? Using `\n`:

-   `\n` gives a newline, also known as a _line feed_.
-   `\r` is a _carriage return_. It means "move the cursor to the front of the line".
-   `\f` is a _form feed_. It means "move to a new page". This doesn't often appear.
-   `\v` is a _vertical tab_. It means "move the cursor down".

There are some caveats. `\n` on its own moves to the start of a new line, meaning you can end your lines with just a `\n`. This is known as "LF" or Unix-style endings. You can also do `\r\n`, which is what Windows does, also known as "CRLF" for "Carriage Return, Line Feed". `\f` is basically never used but if you were to do `console.log("hello\fworld")` you would notice that `world` is on the next line but not the start of it. The same goes for `\v`; internally, `\f` and `\v` do represent different things, but they show up the same in most consoles.

-   `\'` gives the literal quote character. This is optional if using `"..."` but required for `'...'`.
-   `\"` gives the literal double quote character. This is optional if using `'...'` but required for `"..."`.
-   <code>\`</code> gives the literal backtick character. This is optional if using both `"..."` and `'...'`; however, it is needed for _template strings_, which will come in just a bit.

-   `\t` gives a literal tab character.
-   `\b` gives a backspace character. This moves the cursor back one place, so if you do `console.log("abc\bxyz");`, you will likely see `abxyz` on your screen since the cursor moves behind the `c` and overwrites it. If you see something else, don't worry too much; different consoles behave differently.

Finally, if you want to actually insert a backslash character itself, do `\\`.

### Arrays

Arrays are a built-in type (classified under `object`) that allow you to easily represent an ordered collection of other objects (including other arrays). You can create them using `[]` like so - `[1, 2, 3]`. We'll go over what you can do with these once we discuss variables and operators.

### Template Strings

These are actually not very complicated. Instead of `"..."` or `'...'`, we do <code>\`...\`</code>. These _can_ span multiple lines so you don't need `\n` and can just insert newlines directly, but most importantly, you can interleave code segments in using `${...}`. For example, <code>\`Hello ${1 + 2}\`</code> would evaluate to `"Hello 3"`. The part within the `${...}` is evaluated at runtime and so this lets you set up a _template_ that formats the values into a string. An application of this might be something like <code>console.log(\`Welcome back, ${username}!\`);</code>.

We'll end up using these more in the future, so if it's a bit confusing right now, don't worry. All you need to know is that you can inject the result of a snippet of code into a string, and to use <code>\`</code> and `\$` in template strings to insert literal backticks or dollar signs.

## Functions

A function, to put it simply, is a sequence of steps packed into an input-output machine that can take inputs, do some task or run some calculations, and then optionally return a result. We've already looked at two functions - `process.exit` and `console.log` - but there are countlessly many more, and you can (and should) define your own as well (though we will go over this much later, as there are a lot of basics we still need to cover).

`console.log` is an example of a function that does not produce a result - it does output to the console, but the result of the function is absent. This is why if you run `console.log("Hello, World!");` in your REPL, you will see `undefined` below the text. In the node.js REPL, the result of your expression will be printed each time, and the `console.log` function does not return any value.

`Math.max`, on the other hand, is an example of a function that _does_ produce a result - it evaluates to the largest of its inputs, or `-Infinity` if there are none. Thus, you can do `console.log(Math.max(3, 4))` and it will print out `4` - this is because `Math.max(3, 4)` calls the function with inputs `3` and `4`, and so the function responds with the value `4`.

`Math.max` also happens to not have any side effects, also known as a _pure_ function. `console.log` is not, as it produces output to STDOUT and is therefore not pure. Note that returning a result and being impure are not exclusive; you can have side effects and return a result, and also you can be pure and return nothing, although that means your function doesn't actually do anything.

### More about calling functions

If you have something _iterable_ (like an array or a string - something that is a collection of values that you can go through one-by-one), then you can use `...` to expand it out within the function. For example, try `console.log(1, 2, 3)` and `console.log([1, 2, 3])`. Notice the difference - the first outputs three values (automatically separating by spaces) and the second outputs an array. Now do `console.log(...[1, 2, 3])`. Finally, try `console.log(..."hello");`.

### Functions are first-class

In JavaScript, functions are _first-class_, meaning they are treated the same way as other objects. This means that you can also pass them as values to other functions. Try `console.log(process.exit)`. Note that `process.exit` is a function, but we can still use it as a function input because it is just a regular value too.

A common application of this is for _meta-functions_ or functions that specifically are intended to take other functions. `.forEach` is a good example of this - given an array, calling its `.forEach` function using another function as input will run that function on each value. As an example, try `[1, 2, 3].forEach(console.log)`.

```js
1 0 [ 1, 2, 3 ]
2 1 [ 1, 2, 3 ]
3 2 [ 1, 2, 3 ]
```

So, why is it outputting like that and not just printing `1`, `2`, and then `3`? Well, it is a feature of `forEach` works. `forEach` will actually pass three arguments to the function within - the first is the value, the second is the index, and the third is the entire array. Since `console.log` can take any number of arguments, it accepts all three and prints them all, space-separated.

This is another important feature of JavaScript. If a function requests two inputs and you only give it one, the second just becomes `undefined` and there is no error. If a function requests one input and you give it two, it will just ignore the second. This is unlike most other languages that will throw errors, so this is a place in which you need to be very careful because JavaScript might hide your mistakes and make it hard to detect them.

## STDIO

Finally, let's talk about STDIO (Standard I/O - Input/Output). There are three special data streams known as STDIN (Standard In), STDOUT (Standard Out), and STDERR (Standard Error). These are universal and they are very similar to files in that you can open the stream and either read data from it or write data to it (however, unlike files, you can only do one or the other depending on which stream it is).

STDIN is the console input. It is the built-in input stream that all processes have. It is definitely not the _only_ input source - there are many, such as your microphone, your mouse movement and clicks, etc. By default, reading STDIN gives you what the user types into the console after they hit <kbd>Enter</kbd>, but you can do `node main.js < in.txt` to redirect a file into STDIN.

STDOUT is the console output for regular data. All processes have it too, and `console.log` prints to it. By default, this appears on your screen, but you can redirect it elsewhere. Try writing `console.log("Hello, World!")` in your main file and then running `node main.js > test.txt`. You should see no output on your console but now have a file called `test.txt` that contains `Hello, World!` in it.

STDERR is the console output for error messages. `console.error` is a function that acts just like `console.log` but writes to STDERR instead. By default, this also appears on your screen, but you can redirect it using `2>`. Try changing the `console.log` to `console.error` and running `node main.js > test.txt`. Notice how you now see the message on-screen and `test.txt` is now blank. Now run `node main.js 2> test.txt` instead. It is written this way because STDERR has file descriptor `2`. You should now see the same thing as when we ran `> test.txt` with STDOUT.

STDIN is not used so much in JavaScript, and in fact it's a bit annoying to even read from it (in Python you can just do `input`). This is because JavaScript was originally made for websites where you don't really ever use STDIN if it even exists, and rather listen to events such as the user clicking a button. In node.js, while you technically can, you usually don't either, since generally you run webservers where your input is people visiting your domain, or other similar tasks. However, to avoid the overhead of learning things like web development infrastructure, the apps we'll be making will use STDIN and we will just have to use the slightly clunky methods of taking input in node.js.

---

Congratulations! You have now learned about values and their different types in JavaScript, what functions are and how to use them, and finally what STDIO are and what the streams do, as well as a bonus lesson in I/O redirection. Next time, we will go into some more JavaScript basics including an important lesson on _scopes_ and what variables are and how to use them.
