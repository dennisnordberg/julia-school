---
title: How to use Julia arrays
date: 2022-02-28 21:00:08+11:00
seoTitle: "Julia arrays: How to add, delete, and replace items"
description: This tutorial will show you how to add, delete, and replace items in arrays in Julia.
authors: ["Ron Erdos"]
tableOfContents: true
version: 1.7.2
---

## What is an array in the Julia language?

Like in many programming languages, in Julia, an array is an ordered collection, or list, of items. You create arrays in Julia with square brackets, with each item separated with a comma:

`planets = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

## How to get the first "n" items in an array in Julia?

To get the first item in a Julia array, we simply do this:

`first(planets)`

We get:

`"Mercury"`

To get the first "n" items in a Julia array, we add "n" as an optional second argument to `first()`. If we want the first three planets, for example, we'd input:

`first(planets, 3)`

We get:

```
3-element Vector{String}:
 "Mercury"
 "Venus"
 "Earth"
```

<aside>

The ability to add an integer as a second argument to the `first()` function---to specify how many leading items we want from the array---was added in Julia 1.6.4.

</aside>

## How to get the last "n" items in an array in Julia?

To get the last item in a Julia array, we simply do this:

`last(planets)`

We get:

`"Pluto"`

To get the last "n" items in a Julia array, we add "n" as an optional second argument to `last()`. For instance, if we want the last four planets in our array, we input:

`last(planets, 4)`

We get:

```
4-element Vector{String}:
 "Saturn"
 "Uranus"
 "Neptune"
 "Pluto"
```

<aside>

The ability to add an integer as a second argument to the `last()` function---to specify how many trailing items we want from the array---was added in Julia 1.6.4.

</aside>

## How to remove the first item from a Julia array

Let's say the Sun has grown in size and swallowed Mercury. Now we need to delete Mercury (the first element in our array) from our `planets` array. We can do this using a function called `popfirst!()`:

`popfirst!(planets)`

In the terminal, we see the planet we just deleted:

`"Mercury"`

And if we input `planets`, we get:

```
8-element Vector{String}:
 "Venus"
 "Earth"
 "Mars"
 "Jupiter"
 "Saturn"
 "Uranus"
 "Neptune"
 "Pluto"
```

Note that our `planets` array has been made smaller in a persistent way---the `popfirst!()` function modifies its arguments.

<aside>

In Julia---and in some other programming languages---if a function has an exclamation mark after its name, this means it will alter some data in a way that persists.

For example, the `popfirst!()` function removes---in a non-temporary way---the first item from an array.

Functions with an exclamation mark are sometimes called "bang functions".

</aside>

## How to remove the last item from a Julia array

Since Pluto is now officially a "dwarf planet" rather than a regular planet, perhaps it shouldn't be in our `planets` array either.

To remove the last item from an array, we can use `pop!()`, like this:

`pop!(planets)`

Julia will show us the planet we just deleted:

`"Pluto"`

Now when we have Julia show us the updated array:

`planets`

... we get:

```
7-element Vector{String}:
 "Venus"
 "Earth"
 "Mars"
 "Jupiter"
 "Saturn"
 "Uranus"
 "Neptune"
```

Bye Pluto. We can see that Mercury isn't there either---we permanently deleted it in the section above.

## How to remove an item from a Julia array by index number

Let's create a new array:

`astronauts = ["Buzz Aldrin", "Neil Armstrong", "Michael Collins", "Yuri Gagarin"]`

What if we now want to delete, say, the second astronaut in our array?

We can do this with the `deleteat!` function. (It looks like the function name is "delete eat" but it's actually "delete at" 😂).

`deleteat!(astronauts, 2)`

We get:

```
3-element Vector{String}:
 "Buzz Aldrin"
 "Michael Collins"
 "Yuri Gagarin"
```

You can also delete a range of elements from an array using `deleteat!()` in Julia.

Let's start with our original planets again. By inputting the code below, we'll be overwriting our seven-planet array with our original nine-planet array.

`planets = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

If we want to delete planets three to seven inclusive, we can do this:

`deleteat!(planets, 3:7)`

We see the remaining planets:

```
4-element Vector{String}:
 "Mercury"
 "Venus"
 "Neptune"
 "Pluto"
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

## How to keep only certain elements in a Julia array

Now that Julia 1.7 has been released, we can opt to keep only certain elements in a Julia array with `keepat!()`.

For example, say we're using our original array of rockets:

`rockets = ["Apollo", "Saturn", "Falcon Heavy"]`

We can keep just the second rocket:

`keepat!(rockets, 2)`

We get:

```
1-element Vector{String}:
 "Saturn"
```

If we want to keep more than one element, we can do so by passing in an array instead of an integer. If we start with our original astronauts:

`astronauts = ["Buzz Aldrin", "Neil Armstrong", "Michael Collins", "Yuri Gagarin"]`

We can keep the second and fourth astronaut like this:

`keepat!(astronauts, [2,4])`

We get:

```
2-element Vector{String}:
 "Neil Armstrong"
 "Yuri Gagarin"
```

<aside>

Above, we surrounded `2,4` with square brackets, as when we pass more than one number to `keepat!()`, it needs to be in an array (which we denote with square brackets).

</aside>

You can also pass a range of element positions to `keepat!()`.

Let's start with our original planets array:

`planets = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

If we want to keep only planets three to five (inclusive), we do this:

`keepat!(planets, 3:5)`

We get:

```
3-element Vector{String}:
 "Earth"
 "Mars"
 "Jupiter"
```

## How to add an item to an array in Julia

Let's start with our original planets array (including Pluto) again:

`planets = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

And let's pretend NASA (with a little help from SpaceX and Blue Origin) have just discovered a new planet, Mythos. To add it to our `planets` array, we use `push!()`:

`push!(planets, "Mythos")`

We get:

```
10-element Vector{String}:
 "Mercury"
 "Venus"
 "Earth"
 "Mars"
 "Jupiter"
 "Saturn"
 "Uranus"
 "Neptune"
 "Pluto"
 "Mythos"
```

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

## How to find and replace with Julia arrays

Time for a new array:

`array_1 = ["Houston", "Washington", "orlando"]`

Notice that the first two items are in title case, while `"orlando"` is in lowercase. Let's make it consistent using a find and replace.

We can do this:

`array_1 = replace(array_1, "orlando" => "Orlando")`

Now when we input `array_1` into our Julia terminal, we get:

```
3-element Vector{String}:
 "Houston"
 "Washington"
 "Orlando"
 ```

 Voila!
 
 However, we can also use the bang function `replace!()`, which will make our code more concise. Let's use another array to demonstrate:

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

Just what we wanted.

## How to use regex with "find and replace" on arrays in Julia

Using regex to find and replace within arrays in Julia requires a slightly different approach. Let's work with an example.

Let's input a third array into our terminal:

`array_3 = ["fish", "fishes", "goldfish", "cat"]`

Let's normalise this data so that each of the different types of fish end up as simply `"fish"`.

The way to do this in Julia is as follows:

`array_3 = replace.(array_3, r"(gold)?fish(es)?" => s"fish")`

When we input `array_3` into our Julia terminal, we get our desired result:

```
4-element Vector{String}:
 "fish"
 "fish"
 "fish"
 "cat"
```

Let's walk through the code above --- including the regex --- and then we'll get to some approaches that don't work.

> `array_3 =` Here we are reassigning `array_3` to the result of our forthcoming find-and-replace. This will make the change persist.

> `replace.(` Here we call the built-in Julia function `replace`. The dot `.` after `replace` indicates we're using the "broadcast" version of the function. This means that our operation will apply to each item in the array.
>
> Next, we open the parentheses so we can add our arguments.

> `array_3` We tell Julia where to perform the find-and-replace --- the proverbial haystack.

> `r"(gold)?fish(es)?"` We now tell Julia what we're looking for --- the proverbial needle.
>
> In this case, it's a regex (denoted by the letter `r` immediately before an opening double quote mark).
>
> Inside the double quotes, we have the our string `fish`, surrounded by the optional prefix `gold` (as in `goldfish`) and the optional suffix `es` (as in `fishes`). If you're not familiar with regex, it can be a tricky subject, but the question mark `?` means the preceding text is optional. And because we've wrapped `gold` and `es` in parentheses, we've made each a separate optional "unit".

> `=>` The "fat arrow" or "stabby arrow" means replace the "needle" with whatever comes next. In this case, it's ...

> `s"fish"` --- Here, we're telling Julia our desired replacement. The `s` stands for "substitution".

> `)` Here we close the parentheses for the `replace.` function.

### Approaches that don't work for using regex to find and replace in Julia arrays

There are a couple of approaches that won't work here.

First, let's reset our array:

`array_3 = ["fish", "fishes", "silverfish", "cat"]`

Now let's try the "reassign" approach that worked for non-regex find-and-replace on arrays earlier in this tutorial:

`array_3 = replace(array_3, r"(gold)?fish(es)?" => s"fish")`

This doesn't do anything; if we input `array_3` into our Julia terminal, we get the unwanted original values:

```
4-element Vector{AbstractString}:
 "fish"
 "fishes"
 "goldfish"
 "cat"
```

Okay, what about the "bang function" approach that also worked for non-regex find-and-replace on arrays earlier in this tutorial? Let's try that:

`replace!(array_3, r"(gold)?fish(es)?" => s"fish")`

That doesn't work either; when we input `array_3`, we still get the same unchanged result:

```
4-element Vector{AbstractString}:
 "fish"
 "fishes"
 "goldfish"
 "cat"
```

The takeout here is that when using regex to find and replace in Julia arrays, we need to use the "broadcast" version of the replace function, as described at the top of this section. Even though we can get away without using it for non-regex find-and-replace operations on Julia arrays, we need the broadcast-enabled `replace.()` when using regex.

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

`planets = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto"]`

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
