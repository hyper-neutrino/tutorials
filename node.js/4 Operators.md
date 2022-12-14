# Operators

This lesson just goes over operators, which are built-in syntactical components that allow you to combine and manipulate values. There are a lot of operators, so make sure you refer back to this page if you ever forget what some do or need to remember which operator does something.

## Arithmetic Operators

The following arithmetic operators should be very familiar.

-   `x + y` adds two numbers
-   `+x` returns the number itself
-   `x - y` subtracts two numbers
-   `-x` returns the negative of the number
-   `x * y` multiplies two numbers
-   `x ** y` takes `x` to the power of `y`
-   `x / y` divides two numbers
-   `x % y` returns the remainder of dividing `x` by `y` (the result has the same sign as `x`)

`%` is known as the "modulo operator".

What if `x` or `y` aren't numbers? Except for addition, it attempts to convert them into numbers, and if that isn't valid, the result is `NaN`, meaning "not a number". However, `x + y` will convert both sides to strings if either side is not a number and join them together. This is also known as the "concatenation operator", so `"Hello, " + "World!"` is `"Hello, World!"`. Be careful that this means that `"123" - 2` is `121` but `"123" + 2` is `"1232"`.

`+x` and `-x` also coerce `x` to numbers. For some reason, `""` is a valid number, so `+""` is `0`. `+" "` is also `0` which makes some sense because `+" 2"` and `+"2 "` both work and give `2`.

These operations also work on `bigint`s. If either `x` or `y` is a `bigint`, the other must be as well, otherwise you will get a `TypeError` as `bigint`s and other types cannot be mixed. You can convert a number to a `bigint` using `BigInt(3)` and vice versa using `Number(3n)`.

## Comparison Operators

The following should also be very familiar.

- `x == y` checks if `x` and `y` are equal
- `x === y` checks if `x == y` and also that they are of the same type
- `x != y` is the opposite of `x == y`
- `x !== y` is the opposite of `x === y`
- `x > y` checks if `x` is greater than `y`
- `x < y` checks if `x` is less than `y`
- `x >= y` is the opposite of `x < y`
- `x <= y` is the opposite of `x > y`

Be careful with using `==`. Because they are both considered "empty" values, `[] == 0` (although `[]` is considered truthy but `0` is falsy). `"0" == 0` as well. Finally, `" " == 0` as well. However, `[] != "0"`, `"0" != " "`, and `" " != []`, meaning equality of values is **not** transitive and can be misleading. Using `===` would give `false` for all of these evaluations, as they would not be of the same type.

Also, `x >= y` is the opposite of `x < y`, **not** the same as `x > y` or `x == y`. For example, `null == 0` and `null > 0` are both false, but `null >= 0`. This is because `null < 0` is also `false`.

You can also compare strings in alphabetical order using these; `"a" < "ab"`, `"aaa" < "aaaa"`, and `"abc" > "aba"`.

Generally, if you only work with sensible values, these will make sense, but do be careful with different types and weird values like `null`.

## Ternary Operator

`x ? y : z` will evaluate to `y` if `x` is truthy and `z` if `x` is falsy. For example, `0 ? 1 : 2` is `2` and `1 ? 1 : 2` is `1`. This operation only evaluates the side that is true, so if `x` is truthy then `z` is not evaluated, and if `x` is falsy then `y` is not evaluated, and any potential errors will not happen. This is useful both for avoiding unnecessary computations and in case `x` is being used as a guard against errors; for example, `x == 0 ? special_case() : normal_case(x)` can be useful if putting `0` into `normal_case` would cause an error.

## Logical Operators

`x && y` is known as **logical AND** and evaluates to a result that is truthy if and only if `x` **and** `y` are both truthy (hence the name). Note that it does not actually always return `true` or `false` itself. Rather, if `x` is falsy, it returns `x`, and otherwise, it returns `y`. So, it is equivalent to `x ? y : x`. The reason for this is because if `x` is falsy, then `x` and `y` are not both truthy, so we know the output should be falsy, and if `x` is truthy, then whether or not `x` and `y` are both truthy depends only on `y`, so we can return it.

`x || y` is known as **logical OR** and evaluates to a result that is truthy if and only if `x` **or** `y` is truthy (and possibly both). Like `&&`, it doesn't return `true` and `false` themselves; rather, if `x` is truthy, it returns `x`, and otherwise, it returns `y`, so it is equivalent to `x ? x : y`. If `x` is truthy, then we know that `x` or `y` is truthy, so we can return it immediately, and if not, then whether or not `x` or `y` is truthy depends only on `y`, so we can return it.

Both of these operators will short circuit, meaning that if `x` is falsy for `&&` and if `x` is truthy for `||`, it will not even evaluate `y`. Therefore, even if evaluating `y` would result in an error, it will not happen as long as it is not a syntax error. This can be useful for when `x` is being used as a guard; for example, a divisibility check could be implemented as `y == 0 ? false : x % y == 0`. In this case, division and modulo by `0` are not actually errors in JavaScript, but this demonstrates a potential usage.

`!x` is known as **logical NOT** and returns `true` if `x` is falsy and `false` if `x` is truthy. Unlike the other two, this one will actually return a boolean value. For example, `!0 === true`, `!"hello" === false`, and `![] === false` (because empty arrays are truthy).

`!!x` is not an operator, but just two logical NOT operators together (`!(!x)`) but can be used to convert `x` into a boolean, as flipping its truthiness twice returns the boolean with the same evaluation as it. So for example, `!!0 === false` telling us that `0` is falsy, and `!![] === true`.

## Type Operators

`typeof x` returns the type of `x` as a string, which is one of `number`, `bigint`, `string`, `boolean`, `undefined`, `symbol`, `function`, or `object`. Note that `typeof 3` is `number` but `typeof new Number(3)` is `object`. We'll go over what `new` does much later, but `new Number(3)` returns a `Number` object rather than the primitive data type. Similarly, `typeof "hello"` is `string` but `typeof new String("hello")` is `object`.

`x instanceof y` returns a boolean indicating whether or not `x` is of the type `y`. `y` here must be a _class_ - we will go over these much later, but examples of classes are `Number` and `String`. Note that `3 instanceof Number` is actually `false` because `3` is a primitive number, not a `Number` object. `new Number(3) instanceof Number` returns `true`.

This is a bit confusing, but you will eventually get used to it. This means that to properly check if something is a number, you need to do `typeof x === "number" || x instanceof Number` and similarly for strings, `typeof x === "string" || x instanceof String`.

## Bitwise Operators

Bitwise operators work on 32-bit numbers. All operands are converted into 32-bit numbers, specifically integers. If the number is outside of range, it will be truncated from the left. Let's first discuss how converting to a 32-bit number works.

You are typically used to seeing a number's base-10 representation. It is a list of digits where the rightmost digit is worth `1`, then the second digit from the right is worth `10`, then `100`, then `1000`, etc. For example, `123` represents `3 * 1 + 2 * 10 + 1 * 100`.

In base-2, the digits are instead worth `1`, `2`, `4`, `8`, `16`, etc. Thus, `0b11010` represents `0 + 1 * 2 + 0 * 4 + 1 * 8 + 1 * 16` which in base 10 is `26`.

JavaScript uses twos' complement, meaning the leftmost bit (digit)'s place value is negated. Consider the following example:

```
11111111 11111111 11111111 11001000
```

Starting from the left, we have `-1*2**31`, then `1*2**30`, then `1*2**29`, ..., and eventually `0*2**5`, `0*2**4`, `1*2**3`, `0*2**2`, `0*2**1`, and `0*2**0`. Therefore, this evaluates to `-56`.

What would be the representation of `-1`? It would be `11111111 11111111 11111111 11111111`.

Generally speaking, to negate a number, we flip all of the bits, and then add `1` to the inverted value. So, since `1` is `00000000 00000000 00000000 00000001`, `-1` would be `11111111 11111111 11111111 11111110 + 1`.

This is not too nice to read, but it will make sense as we go on. The following operators work on these bit representations.

`x & y` takes the **AND** of each bit, digit-by-digit. `13 & 7` is `1101 & 0111`, so bit-by-bit, we get `0101` which is `5` (all bits to the left are `0`). `-13 & 7` is `...11110011 & 0111` so we get `0011` which is `3` (all bits to the left of `7` are `0`). `-13 & -7` is `...11110011 & ...1111001` so we get `...11110001` (all bits to the left are `1`). To interpret that, we can do the "flip and add one" step to get `111` which is `15`. Therefore, `-13 & -7 == -15`.

`x | y` takes the **OR** of each bit, digit-by-digit. `13 | 7` is `15`, `13 | -7` is `-3`, and `-13 | -7` is `-5`. Notice that in general, `x & y` is the same as `-(-x | -y)`, and `x | y` is the same as `-(-x & -y)`.

`~x` takes the **NOT** of each bit; in other words, it flips the bits. `~13` is `~ 00000000 00000000 00000000 00001101` which equals `11111111 11111111 11111111 11110010`. To read this, we again flip the bits and add 1, which gives `00000000 00000000 00000000 00001110` which is `13`. Therefore, `~13 == -14`. This is equal to `-1 - 13`. Of course, flipping bits is a self-inverting operation, so `~-14 == 13`. In general, `~x` is the same thing as `-1 - x`.

`x ^ y` takes the **exclusive or (XOR)** of each bit. This is true if `x` or `y` is true but not both, unlike normal or. `13 ^ 7 == 10` because `1101 ^ 0111` is `1010`. It is sort of like a "not equals" operator as `0 ^ 0 == 0` and `1 ^ 1 == 0`. By definition, `x ^ y == (x | y) & ~(x & y)`; in other words, it is the **AND** of bits are true in either `x` or `y` and the opposite of bits that were true in both.

`x << y` shifts `x` to the left by `y` bits, and anything that goes beyond bit 32 disappears. Bits coming in from the right are `0`. For example, `13 << 1` is `00000000 00000000 00000000 00001101 << 1` which equals `[0] 00000000 00000000 00000000 00011010` or `26`. `-13 << 1` is `11111111 11111111 11111111 11110011 << 1` which equals `[1] 11111111 11111111 11111111 11100110` or `-26`.

Typically, `x << y` is the same as `x * 2 ** y`, but if `x` or `y` are large enough, the sign may be flipped or values may be lost. For example, `2 ** 30` is `1073741824`, but `1073741824 << 1` is not equal to `2147483648`, since it is equal to `01000000 00000000 00000000 00000000`, so left-shifting it by `1` gives `10000000 00000000 00000000 00000000`, and since the `1` moved into the negative bit, `1073741824 << 1 == -2147483648` instead.

`x >> y` shifts `x` to the right by `y` bits and drops anything that gets pushed off the right side. Bits coming in from the left have the same sign as the leftmost bit. For example, `13 >> 1` is `00000000 00000000 00000000 00001101 >> 1` which equals `00000000 00000000 00000000 00000110` which is `6` (the rightmost `1` vanishes). `-13 >> 1` is `11111111 11111111 11111111 11110011 >> 1` which equals `11111111 11111111 11111111 11111001` or `-7`.

`x >> y` is the same as `x / 2 ** y` and then rounded down. Since the incoming bit from the left is the same sign as the original leftmost bit, the sign does not flip.

`x >>> y` shifts `x` to the right by `y` bits, but incoming bits from the left side are instead `0`. For positive numbers, this means that `x >> y === x >>> y`; however, for negative numbers, the result is different. `-13 >>> 1` is `11111111 11111111 11111111 11110011 >>> 1` which is now `01111111 11111111 11111111 11111001` which is `2147483641`.

### What are bitwise operators useful for?

You can use them to combine **bitmasks**. A bitmask is a number whose individual bits have assigned meanings. For example, let's suppose we are storing a user's settings and there are five binary options - whether to display their birthday on their profile, whether to show their email on their profile, light vs. dark mode, if they are above the age of 18, and if they want email notifications. Then, we can store all five into one number, where the `1` bit stores birthday visibility, the `2` bit stores email visiblity, the `4` bit stores theme, the `8` bit stores age, and the `16` bit stores email preferences.

Now, suppose we want to send a newsletter. `x & 16` will be `16` if the user wants emails and `0` otherwise, so we can use a boolean condition to determine whether or not to send an email. Let's suppose we want to send a newsletter with adult content. Then, `x & 24` (where `24 == 0b11000` which combines the `8` and `16` bits) will be `8` if the user is an adult but wants no emails, `16` if they want emails but are a child, `0` if they are neither of age nor want emails, and `24` if they have both bits set, so we can use `x & 24 == 24` as a condition.

Suppose we want to select everyone that either has their birthday visible or their email visible but not both. Then, `x & 1` obtains their birthday visibility and `x & 2` obtains their email visibility. However, we cannot take the XOR as this would `0` for neither, `1` or `2` for one, and `3` for both. We can instead use `(x & 2) >> 1` to shift the email visibility into the `1` bit. Now, `(x & 1) ^ ((x & 2) >> 1)` will be `0` if they have neither or both set, and `1` if they only have one set.

Of course, in the real world, for situations like these it would make more senes to just store these as properties and filter by `user.email_notifications && user.adult`, but this is just a demonstration, and bitmasks do pop up every now and then.

### Bitwise operators on bigints

Bitwise operators work a bit differently on bigints. For bigints, you can imagine that a negative number has infinite `1`s to its left, or that it is an infinite-bit integer. `&`, `|`, and `^` work similarly 

## Property Accessors

`x.y` will get the property named `y` of the value `x`. We've already looked at an example - `console.log` gets the `log` property of the `console` object. We will talk about how to define our own custom objects in the next lesson.

What if we want to access the property with some name where the name is in a variable? For example, `let x = "log";`, then how can we get `console.log`? `console.x` would not work as that would get the property `x` of `console`. Here, we can do `console[x]`. `x[y]` in general will get the property `y` of `x`, where `y` is the _value_ and not the literal text in our code.

This is also how we access values from _arrays_. If `let x = [1, 2, 3];`, then `x[1] == 2`. This is because arrays start at zero in JavaScript. Note that `x["1"]` does the same thing; in fact, keys in JavaScript are always strings, so `x[1]` really just coerces the `1` to a `"1"`.

## Null Operators

What happens if we try to access a property of `undefined`? Try doing `let x = undefined;` and then `x.y`. You will get an error, because you cannot access properties of `undefined`. If we want `x.y` if `x` exists but `undefined` if it doesn't, we could do `x === undefined ? undefined : x.y`. This is conveniently expressed as `x?.y` - if `x` is `undefined`, return `undefined`, and if not, return `x.y`. Note that `y` may not exist as a property, but then it is `undefined` and there is no error.

The above also works with `null`; `null?.x` also returns `undefined`.

We can also do this with `[]` - `x?.[y]` returns `x[y]` if `x` exists and `undefined` otherwise. The syntax is a bit strange, but `x?[y]` would not work as that would be ambiguous with `x ? [y] : z`.

Finally, we can also use this on function calls. `x?.(...)` will call the function `x` if it exists and return `undefined` otherwise. Note that this does not check if `x` is a function or not, so if `let x = 1;` then `x?.()` will still be an error as `1` is not a function.

There is also another null operator, `??`. `x ?? y` is the same as `(x === undefined || x === null) ? y : x`. It is similar to `x || y` in that it short-circuits if `x` is truthy and returns `y` if `x` is falsy, but the difference is that `0 ?? 1` is `0` but `0 || 1` is `1`.

By combining these carefully, you can avoid running into errors trying to access properties of `null` or bugs that pop up from trying to access null values. However, be careful that you do not conceal bugs with this - if you expect `x` to exist, don't do `x?.y`; just do `x.y` so any deviation from expectations will actually raise an error (unless you want to gloss over any mistakes, which might be useful in websites where you don't want bugs to crash or freeze the page).

## Assignment

I've already introduced one operator, the assignment operator `=`. `let x = 10;` declares a variable `x` with value `10`, and the operator can then be used to change the value; `x = 11` will set `x` to `11`. This operator, unlike in some other languages, returns the value, so it is an expression and you can do `console.log(x = 11)` to both set `x` to `11` and output `11` to the console.

Using assignment as an expression in other things can be a bit confusing, so just be careful with it.

Except for comparison, type, and property accessor operators, you can also put a `=` after a binary operator (an operator that takes two values) to make it an augmentation assignment operator. `x #= y` is the same as `x = x # y` where `#` is an operator. For example, `let x = 3; x += 4; console.log(x);` will output `7` as we have mutated `x` to be equal to `x + 4 == 3 + 4 == 7`. This also works with `??`, so `x ??= y` essentially replaces `x` with `y` if it was originally missing.

These will evaluate to the new result of the variable being modified, so `let x = 3; console.log(x += 4);` will set `x` to `7` and also output `7`.

## Increment/Decrement

There are also two special operators that increment (increase by 1) or decrement (decrease by 1) a variable. `x++` increases `x` by `1` and returns the old value, and `++x` increases `x` by `1` and returns the new value. `x--` decreases `x` by `1` and returns the old value, and `--x` decreases `x` by `1` and returns the new value.

To demonstrate:

```js
let x = 0;
console.log(x++); // 0; x is now 1
console.log(++x); // 2; x is now 2
console.log(--x); // 1; x is now 1
console.log(x--); // 1; x is now 0
```

---

This lesson only went over operators, but this is an extremely crucial subject to be familiar with. It's okay if you're a bit confused on how bitwise operators work, but over time you should become more familiar with them, and you should revisit this page as needed or to refresh your knowledge. The next lesson will be shorter, and will just cover custom objects (not classes yet, those are a bit more advanced and we will go over those later).