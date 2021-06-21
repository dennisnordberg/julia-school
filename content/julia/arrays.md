---
title: How to use Julia arrays
date: 2021-06-21 16:56:08+11:00
seoTitle: "Julia arrays: How to add, delete, and replace items"
description: This tutorial will show you how to add, delete, and replace items in arrays in Julia.
authors: ["Ron Erdos"]
tableOfContents: true
version: 1.6.1
---

## What is an array in the Julia language?

Like in many programming languages, in Julia, an array is an ordered collection of items. You create arrays in Julia with square brackets, with each item separated with a comma:

`planets = ["Mercury", "Venus", "Mars", "Earth", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

## How to create a Julia array with the numbers in a given range

If you want a Julia array with all the numbers from 1 to 5:

`collect(1:5)`

We get:

```
5-element Array{Int64,1}:
  1
  2
  3
  4
  5
 ```

If we want to count by anything other than 1, we can put the increment size between our start and end numbers.

For example, if we want an array with the numbers from 1 to 5, counting by 2, we do this:

`collect(1:2:5)`

We get:

```
3-element Vector{Int64}:
 1
 3
 5
 ```

## How to remove the last item from a Julia array

Since Pluto is now officially a "dwarf planet" rather than a regular planet, perhaps it shouldn't be in the `planets` array above.

To remove the last item from an array, we can use `pop!()`:

`pop!(planets)`

Now when we have Julia show us the updated array with:

`planets`

... we get:

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

 Bye Pluto.

## How to remove the first item from a Julia array

 Let's say the Sun has grown in size and swallowed Mercury. Now we need to delete Mercury (the first element in our array) from our `planets` array. We can do this using a function called `popfirst()`:

` popfirst!(planets)`

We get the following result:

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

Now Mercury has been removed from our array too. Pluto is missing too, because we deleted it earlier.

## How to remove an item from a Julia array by index number

Let's create a new array:

`astronauts = ["Buzz Aldrin", "Neil Armstrong", "Michael Collins", "Yuri Gagarin"]`

 What if we now want to delete, say, the third astronaut in our array?
 
 We can do this with the `deleteat!` function. (It looks like the function name is "delete eat" but it's actually "delete at" 😂).

 `deleteat!(astronauts, 3)`

 We get:

 ```
 3-element Vector{String}:
 "Buzz Aldrin"
 "Neil Armstrong"
 "Yuri Gagarin"
 ```

## How to delete elements from an array by name in Julia

There isn't a simple way to delete an element by name in Julia. However, there are a few slightly complex approaches. Of these, the one I like is below.

Say we have the following array:

`rockets = ["Apollo", "Saturn", "Falcon Heavy"]`

`deleteat!(rockets, findall(x->x=="Saturn",rockets))`

We get:

```
2-element Vector{String}:
 "Apollo"
 "Falcon Heavy"
 ```

## How to add an item to an array in Julia

Let's start with our original planets array (including Pluto) again:

`planets = ["Mercury", "Venus", "Mars", "Earth", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

And let's pretend NASA (with a little help from SpaceX and Blue Origin) have just discovered a new planet, Mythos. To add it to our `planets` array, we use `push!()`:

`push!(planets, "Mythos")`

We get:

```
10-element Vector{String}:
 "Mercury"
 "Venus"
 "Mars"
 "Earth"
 "Jupiter"
 "Saturn"
 "Uranus"
 "Neptune"
 "Pluto"
 "Mythos"
 ```

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

### Code walkthrough

Let's go through the example above bit by bit:

> `array_1 =` Here we are re-assigning the array (named `array_1`) to the output of our code. This means it is a "destructive" outcome.

> `replace.` This is the "find and replace" function in Julia. We use the `.` to signify that we want to apply our find and replace to each item in the array. In Julia, this is called "broadcasting" 🎙.

> `(` Here, we open the function brackets, a necessary task.

> `array_1` Now we tell Julia which array to perform our "find and replace". In this case, it's our array named `array_1`.

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
planets_a = ["Mars", "Earth", "Neptune"]
planets_b = ["Pluto", "Earth", "Jupiter"]
```

 Now let's say we want to see all the elements in both arrays, deduped (note that `"Earth"` appears in both arrays).

 We can do that like this:

 `array_union = union(planets_a, planets_b)`

 We get:

 ```
 5-element Array{String,1}:
 "Mars"
 "Earth"
 "Neptune"
 "Pluto"
 "Jupiter"
 ```

 Notice that the result is deduped---we only get `"Earth"` once, even though it appeared in both arrays.

## How to get the intersection of two arrays in Julia

 We can also get the _intersection_ of two arrays in Julia.

 This means we want to see _only_ the element(s) that appear in both arrays.

 We do that like this:

 `array_intersect = intersect(planets_a, planets_b)`

 The result:

 ```
 1-element Array{String,1}:
 "Earth"
 ```

 `"Earth"` was the only element that appeared in both arrays, so it is the only element that appears in the intersection.

 ## How to get the opposite of the intersection on two arrays in Julia (symmetric difference)

 If you want to get the opposite of the intersection of two arrays in Julia---items that appear in one array or the other but not both, then you need to use `symdiff()`.
 
 <aside>
 
 In mathematics, the set of non-overlapping items between two sets is called the "symmetric difference" (or the "disjunctive union"), hence Julia's function name `symdiff()`.

 </aside>

 Here's an example. Let's say we have the following two arrays. You'll notice the only common item is `Apollo`:

```
 rockets = ["Apollo", "Saturn", "Falcon Heavy"]
 greek_gods = ["Apollo", "Zeus", "Hera"]
 ```

 We run the symdiff function to get all the non-common items:

 `symdiff(rockets, greek_gods)`

 The result:

 ```
 4-element Vector{String}:
 "Saturn"
 "Falcon Heavy"
 "Zeus"
 "Hera"
 ```

 `Apollo` is the only item in both arrays, and thus it is the only item not to appear in our result.

## How to get the items that exist in one Julia array but not the other

Here are our two planet arrays again:

```
planets_a = ["Mars", "Earth", "Neptune"]
planets_b = ["Pluto", "Earth", "Jupiter"]
```

Let's say we want the items that exist in `planets_a`, but don't appear in `planets_b`.

We do that like this:

`setdiff(planets_a, planets_b)`

... and we get:

```
2-element Array{String,1}:
 "Mars"
 "Neptune"
 ```

 The order matters here. If we reverse the order of the arrays inside the `setdiff` function, like this:

 `setdiff(planets_b, planets_a)`

 ... we get:

 ```
 2-element Array{String,1}:
 "Pluto"
 "Jupiter"
 ```

## How to check if a value exists in an array

Let's begin with the O.G. nine planets again:

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

## How to deduplicate a Julia array

This one's pretty simple. Let's say you have an array with duplicates:

`space_companies = ["SpaceX", "Blue Origin", "Boeing", "Sierra Nevada Corporation", "Boeing"]`

(We have Boeing twice.)

Here's how you dedupe it in Julia:

`space_companies_deduped = unique(space_companies)`

This gives you the deduped array:

```
4-element Vector{String}:
 "SpaceX"
 "Blue Origin"
 "Boeing"
 "Sierra Nevada Corporation"
```

Or if you wanted to overwrite the original array with the deduped list, you could do it like this:

`unique!(space_companies)`

Now when you call up `space_companies`, you get this:

```
4-element Vector{String}:
 "SpaceX"
 "Blue Origin"
 "Boeing"
 "Sierra Nevada Corporation"
 ```

## How to write a Julia array to a CSV file

 Let's say you want to write the contents of a Julia array to a CSV file.

We'll be using the `array_union` file from earlier in this tutorial. If you've just joined and want to follow along, you can create it like this:

`array_union = ["Mars", "Earth", "Neptune", "Pluto", "Jupiter"]`

Okay, now let's specify where the file should be saved, its name and filetype. To keep things simple, let's store it in a new variable we'll create called `outfile`:

 `outfile = "/Users/ron/Desktop/outfile.csv"`

 So the file will be called `outfile.csv` and it will be created on my Mac desktop.

 Obviously, you'll need to change the `ron` part in the path above. And that's assuming you're on Mac; if you're on Windows, you'll need to alter the path entirely.

 Next, we're going to use a module that's built-in to Julia, called `DelimitedFiles`. Because it comes with Julia out of the box, you don't need to install it. You do, however, need to tell Julia that you want to use it:

 `using DelimitedFiles`

 Now we can use `DelimitedFiles` to write our CSV:

 `writedlm(outfile, array_union)`

 Walking through that code above, we're using a built-in function called `writedlm` to write our `array_union` of planets to our `outfile` (which we defined earlier).

You'll end up with an `outfile.csv` on your desktop, the contents of which reads:

```
Mars
Earth
Neptune
Pluto
Jupiter
```
