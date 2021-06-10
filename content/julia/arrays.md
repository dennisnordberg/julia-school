---
title: How to use Julia arrays
date: 2021-06-10 20:28:09+11:00
seoTitle: "Julia arrays: How to add, delete, and replace items"
description: This tutorial will show you how to add, delete, replace items in arrays in Julia.
authors: ["Ron Erdos"]
tableOfContents: true
version: 1.6.1
---

## What is an array in the Julia language?

Like in many programming languages, in Julia, an array is an ordered collection of items. You create arrays in Julia with square brackets, with each item separated with a comma:

`planets = ["Mercury", "Venus", "Mars", "Earth", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

## How to create a Julia array with the numbers in a given range?

If you want a Julia array with all the numbers from 1 to 10:

`collect(1:10)`

We get:

```
10-element Array{Int64,1}:
  1
  2
  3
  4
  5
  6
  7
  8
  9
 10
 ```

 (Yes, I specially designed this website so the above would look like _The Matrix_.)

If we want to count by anything other than 1, we can put the increment size between our start and end numbers. For example, if we want an array with the numbers from 5 to 15, counting by 2, we do this:

`collect(5:2:15)`

We get:

```
6-element Array{Int64,1}:
  5
  7
  9
 11
 13
 15
 ```

## How to remove an item from an array in Julia

Since Pluto is now officially a "dwarf planet" rather than a regular planet, perhaps it shouldn't be in the `planets` array above.

We can remove elements from Julia arrays in a few different ways.

### How to use the "pop!" function to remove the last item from a Julia array

To remove the last item from an array, we can use `pop!()`:

`pop!(planets)`

Now when we have Julia show us the `planets` array again, we get:

```
8-element Array{String,1}:
 "Mercury"
 "Venus"
 "Mars"
 "Earth"
 "Jupiter"
 "Saturn"
 "Uranus"
 "Neptune"
 ```

 Bye Pluto!

### How to use the "popfirst!" function to remove the first item from a Julia array

 Let's say the Sun has grown in size and swallowed Mercury. Yikes, likely to be quite hot here on Earth! Anyway, let's delete Mercury (the first element in our array) from `planets`:

` popfirst!(planets)`

 Apart from it getting rather hot here on Earth, here's how our `planets` array looks now:

 ```
 7-element Array{String,1}:
 "Venus"
 "Mars"
 "Earth"
 "Jupiter"
 "Saturn"
 "Uranus"
 "Neptune"
 ```

 Bye Mercury! Note that Pluto is also missing because we deleted it earlier in this tutorial.

And then there were seven.

### How to use the "deleteat!" function to remove a given item from a Julia array

 What if we don't want to delete the first or last item in a Julia array, but rather, a given item in the middle? If we know the index number of the item we want to remove, we can do this with the `deleteat!` function. (It looks like the function name is "delete eat" but it's actually "delete at" 😂).

 In our array of what is now seven planets, we can delete Jupiter, the fourth item, like this:

 `deleteat!(planets, 4)`

 We get:

 ```
 6-element Array{String,1}:
 "Venus"
 "Mars"
 "Earth"
 "Saturn"
 "Uranus"
 "Neptune"
 ```

 Bye Jupiter! Geez, only six planets left!

### How to delete elements from an array by name in Julia

There isn't a simple way to delete an element by name in Julia. However, there are a few complex approaches. Of these, the one I like is:

`deleteat!(planets, findall(x->x=="Saturn",planets))`

We get:

```
5-element Array{String,1}:
 "Venus"
 "Mars"
 "Earth"
 "Uranus"
 "Neptune"
 ```

Bye Saturn! And then there were five.

## How to add items to an array in Julia

NASA (with a little help from SpaceX and Blue Origin) have just discovered a new planet, Mythos*. To add it to our `planets` array, we use `push!()`:

`push!(planets, "Mythos")`

We get:

```
6-element Array{String,1}:
 "Venus"
 "Mars"
 "Earth"
 "Uranus"
 "Neptune"
 "Mythos"
 ```

* _Not really._

## How to find and replace strings in Julia arrays

OK, time for a new array:

`array_1 = ["Houston", "Astrodome", "apollo"]`

Let's say you want to fix that lowercase `apollo`, so that it becomes `Apollo`.

Here's how to do that in Julia:

`array_1 = replace.(array_1, "apollo"=>"Apollo")`

Now `array_1` looks like this:

```
array_1
3-element Array{String,1}:
 "Houston"
 "Astrodome"
 "Apollo"
 ```

 Great!

### Code walkthrough

Let's go through the example above bit by bit:

> `array_1 =` Here we are re-assigning the array (named `array_1`) to the output of our code. This means it is a "destructive" outcome.

> `replace.` This is the "find and replace" function in Julia. We use the `.` to signify that we want to apply our find and replace to each item in the array. In Julia, this is called "broadcasting" 🎙.

> `(` Here, we open the function brackets, a necessary task.

> `array_1` Now we tell Julia which array to perform our "find and replace". In this case, it's an array named `array_1`.

> `"apollo"` Here's the "needle" we're looking for in the proverbial haystack. We're looking for all instances of `apollo`.

> `=>` The "fat arrow" in Julia means, in this context, "transform". So we are transforming `"apollo"` into whatever comes after the fat arrow (which in this case, is `"Apollo"`)

> ``"Apollo"`` Here's what we want our "find and replace" matches to become.

> `)` Here we close the function brackets, a necessary task.

## How to find and replace strings using regex in Julia arrays

Okay, new example. Let's say we have the following array:

`array_2 = ["Lunar Rover 1", "%Lunar Rover 2", "50% Sun Filter"]`

We can see there's an extraneous `%` at the start of the second item in this array, `%Lunar Rover 2`.

However, the `%` in `"50% Sun Filter"` is __meant__ to be there.

So how can we delete the `%` from `%Lunar Rover 2` without also deleting the `%` from `50% Sun Filter`?

The answer is: with regex.

Here's how to use regex to do a find and replace in a Julia array:

`array_2 = replace.(array_2, r"^%"=>"")`

We get our desired result:

```
3-element Array{String,1}:
 "Lunar Rover 1"
 "Lunar Rover 2"
 "50% Sun Filter"
 ```

### Code walkthrough

The `replace.()` code is virtually the same as in the previous, non-regex example; the only difference here is that we've added regex.

Let's walk through the code bit by bit:

> `array_2 =` Here we are re-assigning the array (named `array_2`) to the output of our code. This means it is a "destructive" outcome.

> `replace.` Here's our Julia "find and replace" function again. We're again using our trusty `.` to signify that we want to apply our find and replace to each item in the array.

> `(` Here, we open the function brackets, a necessary task.

>`array_2`  Now we tell Julia which array to perform our "find and replace". In this case, it's an array named `array_2`.

> `r"^%"` Here's the "needle" we're looking for in the proverbial haystack. This is what we want Julia to match. The `r` before the first double quote tells Julia we're using regex. The double quotes contain the "needle"; the match itself. The caret `^`, when used this way in regex, means "the start of the string". And finally, we have the percentage sign itself. (Remember, we're trying to remove leading percentage signs from elements in our array). Putting that all together, we're looking for a percentage sign at the start of a string.

> `=>` The "fat arrow" in Julia means, in this context, "transform". So we are transforming our regex match into whatever comes after the fat arrow (see below).

> `""` The empty double quotes are, in this context, how you replace something with nothing, which is what we're doing here; replacing a leading `%` with nothing.

> `)` Here we close the function brackets.

## How to get the union of two arrays in Julia

 Let's say we have two arrays:

 ```
 array_3 = ["Mars", "Earth", "Neptune"]
 array_4 = ["Pluto", "Earth", "Jupiter"]
 ```

 Now let's say we want to see all the elements in both arrays, deduped (note that `"Earth"` appears in both arrays).

 We can do that like this:

 `array_union = union(array_3, array_4)`

 We get:

 ```
 5-element Array{String,1}:
 "Mars"
 "Earth"
 "Neptune"
 "Pluto"
 "Jupiter"
 ```

 Notice that we only get `"Earth"` once, even though it appeared in both arrays.

## How to get the intersection of two arrays in Julia

 We can also get the _intersection_ of two arrays in Julia.

 This means we want to see _only_ the element(s) that appear in both arrays.

 We do that like this:

 `array_intersect = intersect(array_3, array_4)`

 The result:

 ```
 1-element Array{String,1}:
 "Earth"
 ```

 `"Earth"` was the only element that appeared in both arrays, so it is the only element that appears in the intersection.

## How to get the items that exist in one Julia array but not the other

Let's say we want the items that exist in `array_3`, but don't appear in `array_4`.

We do that like this:

`setdiff(array_3, array_4)`

... and we get:

```
2-element Array{String,1}:
 "Mars"
 "Neptune"
 ```

 The order matters here. If we reverse the order of the arrays inside the `setdiff` function, like this:

 `setdiff(array_4, array_3)`

 ... we get:

 ```
 2-element Array{String,1}:
 "Pluto"
 "Jupiter"
 ```

## How to check if a value exists in an array

Let's begin with the O.G. planets again:

`planets = ["Mercury", "Venus", "Mars", "Earth", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

If you wanted to check if Pluto was included in our `planets` array (hey, you never know these days), you can do that like this:

`"Pluto" in planets`

We get a result of `true`, which tells us that Pluto does in fact exist in our array.

Conversely:

`"Krypton" in planets`

leads to a result of `false`.

## How to handle uppercase and lowercase when checking if a value exists in an array

Using the example above, if we had instead checked if `"pluto"` (lowercase) existed in the `planets` array:

`"pluto" in planets`

then we'd get a result of `false`, because `"pluto"` is not the same thing as the `"Pluto"`.

However, you can handle lowercasing elegantly like this:

`"pluto" in lowercase.(planets)`

This gives us a result of `true`, because we told Julia to lowercase the `planets` array before checking for `"pluto"`.

In the code above, we're using the `lowercase()` function, which turns strings to lowercase in Julia, _but_ we've added a dot immediately after `lowercase`. This dot tells Julia to convert each item in the array to lowercase, rather than to the array itself (which isn't possible and leads to a `MethodError`). This trailing dot is known as a "broadcast function", and it's built in to Julia---no package required.

You can similarly handle uppercase inconsistencies:

`"PLUTO" in uppercase.(planets)`

gives us a result of `true`.

## How to write a Julia array to a file

 Let's say you want to write the contents of a Julia array to a `.txt` file.

We'll be using the `array_union` file from earlier in this tutorial. If you've just joined us and want to follow along, you can create it like this:

`array_union = ["Mars", "Earth", "Neptune", "Pluto", "Jupiter"]`

Okay, now let's specify where the file should be saved, its name and filetype. To keep things simple, let's store it in a new variable we'll create called `outfile`:

 `outfile = "/Users/ron/Desktop/outfile.txt"`

 So the file will be called `outfile.txt` and it will be created on my Mac desktop.

 Obviously, you'll need to change the `ron` part in the path above. And that's assuming you're on Mac; if you're on Windows, you'll need to alter the path entirely.

 Right, now we need to iterate over the `array_union` array and write each element to its own line in our `outfile`. We do that like this:

```
open(outfile, "w") do f
    for i in array_union
        println(f,i)
    end
end
```

You'll end up with an `outfile.txt` on your desktop, the contents of which reads:

```
Mars
Earth
Neptune
Pluto
Jupiter
```

NB: The code above will overwrite any existing file content. If you want to append, rather than overwrite the file content, then change the `"w"` in the first line to `"a"`, which is short for "append".
