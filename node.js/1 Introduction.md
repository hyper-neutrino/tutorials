# Introduction

Welcome to my node.js tutorial. In this series, I will go over the fundamentals of node.js and guide you through three projects - a number guessing game, a simple calculator app, and a 2048 game.

In this lesson, I will just be going over what programming is in the first place and why you might want to learn it, what JavaScript is and why or why not to learn it, then an installation guide for node.js, and finally showing you a hello world program just as an example.

## What is programming?

At its core, programming is just creating a sequence of instructions for a machine to read. This isn't necessarily a regular home use PC - so many things these days have computers, from cars to airplanes to medical devices. In all of these cases, you obviously do not want any chance of malfunction and want the machine to behave exactly how you want with the highest possible precision, and that is what programming is all about - strict and precise instructions delivered to a machine with zero ambiguity.

As such, instead of giving human-readable instructions, programming ultimately needs to be very rigorously defined so the computer can interpret it without any guesswork. Most programming languages are fairly human readable as it would be very difficult to program without that, but ultimately they all get translated into what computers can understand.

Why learn programming? These days, a lot of career fields have some overlap with programming, and having it as a skill can be very benficial. Besides just for a career, being able to code is a huge convenience; for example, if I need some niche utility tool or want to automate some task, I can do it because I know how to program.

## Why or why not JavaScript?

JavaScript is a solid starting language because it is relatively simple and abstracts a lot of difficulties away. Additionally, its syntax is quite resemblant of a lot of other languages like C/C++ and Java (do note that Java and JavaScript are not really related at all), and so while the experience is not transferable, things will at least be familiar if you later move to other languages.

However, it also has some setbacks in terms of being a starter language. It has a lot of weird quirks that can be quite difficult to understand as a beginner, and because it likes to silently discard errors or just return blank values, it can be difficult to debug and you can find yourself stuck on unexpected behavior for a while if you don't know what to look for.

Ultimately, I would overall still say that it is a fairly good beginner language. Additionally, since it is one of the mainstream languages for websites and has support from essentially all browsers, it is excellent if you want to do web development, and you can also run things like web servers or Discord bots using node.js so it is not restricted to websites and has a lot of potential use cases. Finally, it is also used in a lot of professional environments. Companies like Microsoft, IBM, PayPal, AWS, and many more use node.js, on top of it being extremely prevalent in all web development areas, so it is not just a beginner language, experience with it can be valuable in professional fields, and I have used it at work before.

## Installing node.js

Before you program in node.js, you will obviously need to install it. node.js is an runtime environment for JavaScript, meaning it is not really its own language, but rather lets you run JavaScript on the server-side (as opposed to on a webpage, which runs it in the browser which would be client-side). It is open-source and cross-platform and can run on Windows, Linux, and macOS.

To install, go to https://nodejs.org/en/download/. The installation should be very straightforward. You can also install it through a package manager: https://nodejs.dev/en/download/package-manager/. For more information, check the starter guide on the official website at https://nodejs.dev/en/learn/how-to-install-nodejs/.

To confirm that you've installed it correctly, open up your terminal and run `node`. You should see a REPL similar to the following appear:

```
$ node
Welcome to Node.js v19.0.1.
Type ".help" for more information.
>
```

Don't worry if the version is different. Type `1 + 2` and hit <kbd>Enter</kbd> and you should see `3` appear below it. Now, exit the node REPL. You can either press <kbd>Ctrl</kbd> + <kbd>D</kbd> or type `process.exit(0)` and hit <kbd>Enter</kbd>, which would be the programmatic way to exit node.js.

## Hello, World!

One of the first snippets of code for learning any language is the Hello World program, which is to just print out that text literally. You could do this in the REPL, but let's write our code down in a file and run it like that. For this, you will want an editor of some sort; I use VSCode but you should use whatever you're familiar with. If you aren't familiar with any editors, Visual Studio Code (**not** Visual Studio) is my recommendation.

Create a new project folder and a new file in it called `main.js`. Now, in this file, type `console.log("Hello, World!");` and save it. Let's go over what this means:

```
console.log("Hello, World!");
^------                         this is the console object, which is used for interfacing with the terminal
       ^---                     this gets the "log" property of the console, which is a function that outputs the given value(s) to the screen
           ^               -    these brackets call the "log" function, which executes it and the contents between the brackets are the arguments
            ^--------------     in this case, there is only one argument, which is the literal string "Hello, World!" which is a piece of text
                            ^   this semicolon means that we are at the end of the statement
```

Now, open your folder in the terminal. On Windows, you can right click in File Explorer and click "Open in Terminal" (if you don't see it, try doing shift + right click). Other operating systems probably have something similar. Now, run `node main.js`. Instead of opening a REPL, this will run the code in the file `main.js`. You should now see `Hello, World!` appear on your screen and it should go back to the terminal instead of opening the node.js REPL.

---

Congratulations! You have now installed node.js and written your first JavaScript program. Next time, we will go over some general fundamentals of programming and an introduction to some basic concepts such as representing data and functions (`console.log` is a function, but we will go more in-depth on what functions are and how to use them).