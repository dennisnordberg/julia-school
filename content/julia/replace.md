---
title: How to find and replace in Julia
date: 2022-02-28 06:28:00+11:00
seoTitle: How to find and replace in strings, arrays in Julia
description: Find and replace requires slightly different approaches when dealing with strings vs arrays in Julia.
authors: ["Ron Erdos"]
tableOfContents: true
draft: true
---

## How to find and replace on strings in Julia

Let's start with a string, such as `"Neptune"`.

Let's assign it to a variable `x`, like so:

`x = "Neptune"`

Now let's perform a "find and replace" on this string. Let's say we want to emphasise the musical nature of the planet 🤷🏽 and call it NepTUNE.

We could do this:

`replace(x, "tune" => "TUNE")`

We get:

`"NepTUNE"`

So far so good.

However, the change is not permanent. When we input our variable again:

`x`

we get:

`"Neptune"`

which was the undesired original value.

To alter `x` so that the change persists, we need to perform the replacement, and assign the result back to `x`, like this:

`x = replace(x, "tune" => "TUNE")`

When we check by inputting `x`:

`x`

We get:

`"NepTUNE"`

Great! And by the way, this works for multiple matches. Let's create a new variable, `y`, with the value `"Neptune Neptune"`:

`y = "Neptune Neptune"`

Now let's reassign `y` using the same code syntax as we did previously:

`y = replace(y, "tune" => "TUNE")`

When we input `y` into our terminal, we get:

`"NepTUNE NepTUNE"`

<aside>

## We can't use replace!() on strings in Julia

In Julia --- and other programming languages --- we have the concept of a "bang function".

This is where we take a function, such as `replace()` and add an exclamation mark, like this: `replace!()`.

Bang functions change data in a way that persists.

For example, if you have an array like this:

`array_1 = ["a", "b", "c"]`

then you can use the `pop!()` bang function to remove the last item (`"c"`) in the array in a way that persists:

`pop!(array_1)`

Now if we enter `array_1` into our terminal, we get our desired result:

```
2-element Vector{String}:
 "a"
 "b"
 ```

However, due to Julia design decisions I haven't delved into, we can't use `replace!()` to turn the string `"Neptune"` into `"NepTUNE"`.

If we do this:

`replace!(x, "tune" => "TUNE")`

we get a `MethodError`. 

So the way to do a find and replace on a string in Julia is as described above this box, i.e.

`x = replace(x, "tune" => "TUNE")`

</aside>

## How to use regex to find and replace on strings in Julia

Say you had some text, and you wanted to change all colour names to `"grey"`. You could do this with regex.

Here's our string:

`z = "The rocket will be painted purple. The fins will be painted gold."`

And here's how we change both purple and gold to grey:

`z = replace(z, r"(purple|gold)" => "grey")`

When we input `z` into our terminal, we now get:

`"The rocket will be painted grey. The fins will be painted grey."`

Let's walk through that code, bit by bit:

> `z =` Here we are reassigning `z` to the result of our forthcoming find-and-replace. This will make the change persist.

> `replace(` Here we call the built-in Julia function `replace`, and open the parentheses so we can add our arguments.

> `z` We tell Julia where to perform the find-and-replace --- the proverbial haystack. In this case, it's the string `z`.

> `r"(purple|gold)"` We now tell Julia what we're looking for --- the proverbial needle.
>
> In this case, it's a regex (denoted by the letter `r` immediately before an opening double quote mark).
>
> Inside the double quotes, we have parentheses with two options, `purple` or `gold`, separated by a pipe symbol `|`, sometimes called a "vertical line". In regex, the pipe symbol means "or" --- purple _or_ gold.

> `=>` The "fat arrow" or "stabby arrow" means replace the "needle" with whatever comes next. In this case, it's ...

> `"grey"` --- as noted before, we want to turn `"purple"` _or_ `"gold"` into `"grey"`.

> `)` Here we close the parentheses for the `replace` function.

<aside>

As in the simple (non-regex) example earlier on this page, we can't use the bang function `replace!()` on strings here either.

</aside>

## How to "find and replace" on arrays in Julia

Let's input the following array into our Julia terminal:

`array_1 = ["Houston", "Washington", "orlando"]`

Notice that the first two items are in title case, while `"orlando"` is in lowercase. Let's make it consistent using a find and replace.

We can use the approach we took earlier in this tutorial:

`array_1 = replace(array_1, "orlando" => "Orlando")`

Now when we input `array_1` into our Julia terminal, we get:

```
3-element Vector{String}:
 "Houston"
 "Washington"
 "Orlando"
 ```

 Voila! However, because we are working with an array, we _can_ use the bang function `replace!()`, which will make our code more concise.

 Let's use another array:

 `array_2 = ["Gagarin", "Armstrong", "aldrin"]`

 This time, let's use the bang function to make the last item in title case:

 `replace!(array_2, "aldrin" => "Aldrin")`

 When we input `array_2` into our Julia terminal, we get:

 ```
 3-element Vector{String}:
 "Gagarin"
 "Armstrong"
 "Aldrin"
 ```

Brilliant!

## How to use regex with "find and replace" on arrays in Julia

Using regex to find and replace within arrays in Julia requires a slightly different approach. Let's work with an example.

Let's input a third array into our terminal:

`array_3 = ["purple", "gold"]`

Now let's turn both colours into `"grey"`.

First, we'll try the "reassign" approach:

`array_3 = replace(array_3, r"(purple|gold)" => "grey")`

Now we input `array_3` into the REPL, but nothing has changed:

```
2-element Vector{String}:
 "purple"
 "gold"
```

I'm not sure why this doesn't work, but it doesn't.

Okay, what about the bang function approach? Let's try that:

`replace!(array_3, r"(purple|gold)" => "grey")`

When we input `array_3`, we still get the same unchanged result:

```
2-element Vector{String}:
 "purple"
 "gold"
```

Hmm. It turns out the way to make this work is to use the "reassign" approach, _but_ with what's called a "broadcast" function. This is where we use a dot `.` after the function name --- so, `replace.()` instead of `replace()` --- to have Julia operate on each item in the array. Here's the code to input:

`array_3 = replace.(array_3, r"(purple|gold)" => "grey")`

When we input `array_3`, _now_ we get the desired result:

```
2-element Vector{String}:
 "grey"
 "grey"
```
