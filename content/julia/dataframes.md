---
title: How to use dataframes in Julia
date: 2021-07-21 00:57:22+11:00
seoTitle: Julia dataframes tutorial
description: "Julia dataframes let you do anything you want: pivot tables, data cleaning, table joins, filtering, and more, all with a nice clean syntax."
authors: ["Ron Erdos"]
tableOfContents: true
version: 1.6.2
---

## What are dataframes?

If you're not familiar with dataframes, read on. If you are, feel free to skip ahead to the <a href="#introducing-our-example-nasa-inventory">next section</a>.

Dataframes are basically a table that you can manipulate in Julia. You can:

- Do a "find and replace" on a given column, or multiple columns. This is useful for correcting typos, fixing capitalisation, and many more tasks.
- Perform mathematical operations, such as add ten per cent to every number in a given column. This would be useful, for, say, calculating sales tax on retail prices.
- Make a pivot table, just like in Excel, only better! Excel has a row limit of 1,048,576. Julia doesn't. In addition, pivot tables in Julia are first-class citizens, ready for more transformations, joins and so on. This is decidedly _not_ the case in Google Sheets or Excel---you often have to copy and paste the pivot table into a new sheet in order to keep working on it.
- Merge two or more tables (dataframes) together according to rules you set. This is conceptually the same as "joins" in SQL and other database languages.

So why would you use a Julia dataframe and not just Excel? I'll write a post on that later and link to it from here.

## Introducing our example: NASA inventory

In this tutorial, we're going to be using a small dataframe of NASA inventory. The data is fictional; it's just for this tutorial.

We'll call the dataframe `inventory`.

It will have three columns: `item`, `id`, and `kind`.

## Installing Julia

If you want to play along (as opposed to just reading through this tutorial), you'll need to [download, install, and run Julia](../setup/) if you haven't already.

Once you have Julia up and running, you'll see this prompt:

`julia>`

This prompt will be green in colour, probably on a black background. Yeah, like in _The Matrix_.

Onwards!

## Installing the DataFrames package

The first dataframes-specific thing we need to is to install the DataFrames package. This is a one-time task.

The easiest way to do this is to input `]` (right square bracket, located above the return/Enter key on your keyboard) into the Julia terminal.

This changes the `julia>` prompt we saw above into one that says:

<code class="julia-pkg">pkg></code>

This prompt will be blue / purple in colour.

This means we are now in Julia's package manager (hence the abbreviation <code class="julia-pkg">pkg</code>
). We can install packages with a lot less typing in here (compared to at the green `julia>` prompt).

Go ahead and add the DataFrames package by inputting the line below:

`add DataFrames`

Julia's package manager will do its thing and install the DataFrames package for you.

You'll be taken back to the package manager prompt:

<code class="julia-pkg">pkg></code>

To exit the package manager area, and to go back to the main Julia area, simply hit the delete key. How cool is that?

You'll now see the Julia prompt again:

`julia>`

Now we can actually create our NASA inventory dataframe.

## How to create a Julia dataframe in the terminal

First, we need to tell Julia we want to use the DataFrames package we just installed.

We do that by inputting the line below:

`using DataFrames`

Next, we input the lines below. This will create our NASA inventory dataframe:

```
inventory = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"%Lunar Rover",
		"30% Sun Filter"
	],
	id = [
		100,
		101,
		102,
		103
	],
	kind = [
		"rover",
		"spaceship",
		"rover",
		"Sun Filter"
	]
)
```

<aside>

### Julia and line break formatting

You don't need to "prettify" your Julia code with all those line breaks as I've done here.

The reason I prettify Julia code on this site is so that you won't have to scroll horizontally (as much) on mobile devices.

Also, the code is more "digestible".

However, my production Julia code is not prettified in this way.

</aside>

We get:

```
4×3 DataFrame
 Row │ item            id     kind
     │ String          Int64  String
─────┼───────────────────────────────────
   1 │ Mars Rover        100  rover
   2 │ Venus Explorer    101  spaceship
   3 │ %Lunar Rover      102  rover
   4 │ 30% Sun Filter    103  Sun Filter
```

Let's quickly talk through what we see above.

### Quick tour of a Julia dataframe

The first words of the output above, `4x3 DataFrame` says that we have a 4-row, 3-column dataframe. Simple enough.

Next, we have the column headers, `Row`, `item`, `id`, and `kind`.

The first of these, `Row`, is not something we asked for, though it is very useful. It's a column of row numbers that Julia generates automatically. But don't worry, they're not part of the dataframe proper, nor will they show up in CSV exports and so forth.

The next column headers, `item`, `id`, and `kind` are those we supplied to Julia in our code above.

<aside>

### Julia variable naming conventions

When it comes to naming variables in Julia, I follow convention and keep them all lowercase. For variable names with multiple words (e.g. `my_new_column`), I use "snake case": all lowercase, with underscores separating the words.

</aside>

Next up, we have the **type** of each column. Let's go over what that means now.

We see `String`, `Int64`, and another `String`.

The `Row` column doesn't have a type because, as mentioned above, it's not part of the dataframe proper. It's just there to help you keep track.

Our first "real" column, `item`, has the **type** `String`.

This means that anything that goes in this column will be treated as text.

The next column, `id`, has the type `Int64`. Although this might look weird, it just means "integer" (a whole number, not a decimal). The `64` just refers to 64 bit, which means huge numbers can be stored here. "Huge numbers" is my technical term for the range of integers between `-2^63` and `2^63 - 1`.

Finally, the last column, `kind`, is another `String`-type column, the same as the `item` column.

And finally, we have the actual data within the rows and columns of our dataframe.

## How to "find and replace" in a Julia dataframe

As mentioned above, I've deliberately included some typos in the `inventory` dataframe, so we can practise "find and replace".

In the `kind` column, `rover` should be `Rover`, and `spaceship` should be `Spaceship`.

Let's clean that up.

Here's how you do that:

```
for i in eachrow(inventory)
	i[:kind] = replace(i[:kind], "rover"=>"Rover")
	i[:kind] = replace(i[:kind], "spaceship"=>"Spaceship")
end
```

To see the results, we call up our dataframe again:

`inventory`

We get:

```
4×3 DataFrame
 Row │ item            id     kind
     │ String          Int64  String
─────┼───────────────────────────────────
   1 │ Mars Rover        100  Rover
   2 │ Venus Explorer    101  Spaceship
   3 │ %Lunar Rover      102  Rover
   4 │ 30% Sun Filter    103  Sun Filter
```

Let's walk through that code line-by-line:

> `for i in eachrow(inventory)`
>
> Here we open up a "for loop", where we'll iterate over each row in the `inventory` dataframe.

> `i[:kind] = replace(i[:kind], "rover"=>"Rover")`
>
> Here we replace any instance of `rover` in the `description` column, and replace it with the correctly-cased `Rover`.

> `i[:kind] = replace(i[:kind], "spaceship"=>"Spaceship")`
>
> Here we do a similar replacement, but this time we're swapping out "spaceship" for "Spaceship".

> `end`
>
> Here we close the "for loop" we started in the first line.

OK, moving on.

There's one more typo in the `inventory` dataframe.

In the `item` column, `%Lunar Rover` should be `Lunar Rover`.

Let's clean that up too. But we'll need to be careful---we don't want to remove the percentage sign from `30% Sun Filter`.

Read on to see how to do this.

## How to "find and replace" with regex in a Julia dataframe

We're going to use regex (regular expressions) to do our find and replace.

The code looks like this:


```
for i in eachrow(inventory)
	i[:item] = replace(i[:item], r"^%"=>"")
end
```

After inputting that code, we call up our dataframe again:

`inventory`

We get:

```
4×3 DataFrame
 Row │ item            id     kind
     │ String          Int64  String
─────┼───────────────────────────────────
   1 │ Mars Rover        100  Rover
   2 │ Venus Explorer    101  Spaceship
   3 │ Lunar Rover       102  Rover
   4 │ 30% Sun Filter    103  Sun Filter
```

Let's walk through that code line-by-line.

> `for i in eachrow(inventory)`
>
> Here we start a "for loop" over the `inventory` dataframe. We use `eachrow()`, a function that ships with Julia's `DataFrames.jl` package, to iterate over the rows.

> `i[:item] = replace(i[:item], r"^%"=>"")`
>
> This line instructs Julia to conduct a find-and-replace on each row of the `item` column, using Julia's `replace()` function. The actual "find and replace" part comes in the form of the second argument, `r"^%"=>""`. It starts with the letter `r`, to signify that we are using regex. Next, we have double quotes surrounding our regex: `"^%"`. The caret, `^` is a regex character which means "match the beginning of the string". The percentage sign, `%`, is a literal match for a percentage sign. Then we have a fat arrow, `=>` which tells Julia what the replacement should be. And finally, we have empty double quote marks, `""`, which tells Julia to replace our regex match with nothing, i.e. delete it. We want to replace the percentage sign at the start of `%Lunar Rover` with nothing---that is, delete it.

> `end`
>
> Finally, we close the "for loop" we started on the first line.

## How to filter a dataframe in Julia

Here's our spruced-up `inventory` dataframe again:

```
4×3 DataFrame
 Row │ item            id     kind
     │ String          Int64  String
─────┼───────────────────────────────────
   1 │ Mars Rover        100  Rover
   2 │ Venus Explorer    101  Spaceship
   3 │ Lunar Rover       102  Rover
   4 │ 30% Sun Filter    103  Sun Filter
```

Let's say we want only the rows which mention "Rover".

If you're just joining us now, you can create the dataframe by inputting the following at the Julia prompt:

```
inventory = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"Lunar Rover",
		"30% Sun Filter"
	],
	id = [
		100,
		101,
		102,
		103
	],
	kind = [
		"Rover",
		"Spaceship",
		"Rover",
		"Sun Filter"
	]
)
```

Now, to make a new dataframe named `rovers` containing just the rows with "Rover" in the `item` column, use this code:

```
rovers = filter(
	x -> any(occursin.(["Rover"], x.item)),
	inventory
)
```

We get:

```
2×3 DataFrame
 Row │ item         id     kind
     │ String       Int64  String
─────┼────────────────────────────
   1 │ Mars Rover     100  Rover
   2 │ Lunar Rover    102  Rover
```

Let's walk through that code line by line:

> `rovers = filter(` 
>
> Here we create a variable named `rovers`, which we'll use to store the filtered, dataframe we're about to create. We'll use Julia's built-in `filter` function to do so. This function takes two "arguments", or inputs, inside its brackets---these are covered below.

> `x -> any(occursin.(["Rover"], x.item)),`
>
> The first "argument" is the rule for the filtering. In our case, we want to include only the rows which contain the word "Rover" in the `item` column.
>
> If you look closely, our rule is actually a function. It's not a named function like `print()`.
>
> Rather, it's an "anonymous" function, because it doesn't have a name. Specifically, it's a "stabby lambda" anonymous function. "Stabby" because it has a stabby arrow, `->`.
>
> And the reason we're using an anonymous rather than named function is because we're only using it once, and it's more concise this way. Whereas we might use our `print` function every second day, so it makes sense for it to have a name.
>
> So what does this "stabby lambda" anonymous function actually do? It says for a given `x`, are there `any` occasions where `"Rover"` occurs in the item column (`x.item`). And because we're using it inside our `filter` function, we are filtering by this rule, this anonymous function.

> `inventory`
>
> The second "argument" is the dataframe we want to reference. In our case, that's the `inventory` dataframe.

> `)`
>
> Here we close the `filter` function we began using in the first line.

## How to join two dataframes in Julia

Did you know Julia can join two tables just like SQL?

### Creating our second dataframe

Let's say we have another dataframe named `sku` which lists each `item` and its corresponding `sku` (stock keeping unit).

Here's the Julia code to create the `sku` dataframe:

```
sku = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"Lunar Rover",
		"30% Sun Filter"
	],
	sku = [
		34566,
		78945,
		15179,
		77254
		]
)
```

We get:

```
4×2 DataFrame
│ Row │ item           │ sku   │
│     │ String         │ Int64 │
├─────┼────────────────┼───────┤
│ 1   │ Mars Rover     │ 34566 │
│ 2   │ Venus Explorer │ 78945 │
│ 3   │ Lunar Rover    │ 15179 │
│ 4   │ 30% Sun Filter │ 77254 │
```

And here's the code to create our original `inventory` dataframe:

```
inventory = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"Lunar Rover",
		"30% Sun Filter"
	],
	id = [
		100,
		101,
		102,
		103
	],
	kind = [
		"Rover",
		"Spaceship",
		"Rover",
		"Sun Filter"
	]
)
```

We get:

```
4×3 DataFrame
 Row │ item            id     kind
     │ String          Int64  String
─────┼───────────────────────────────────
   1 │ Mars Rover        100  Rover
   2 │ Venus Explorer    101  Spaceship
   3 │ Lunar Rover       102  Rover
   4 │ 30% Sun Filter    103  Sun Filter
```

Let's join those two dataframes in a new dataframe named `inventory_sku`. It's only one line of code:

`inventory_sku = outerjoin(inventory, sku, on = :item)`

We get:

```
4×4 DataFrame
 Row │ item            id     kind        sku
     │ String          Int64  String      Int64
─────┼──────────────────────────────────────────
   1 │ Mars Rover        100  Rover       34566
   2 │ Venus Explorer    101  Spaceship   78945
   3 │ Lunar Rover       102  Rover       15179
   4 │ 30% Sun Filter    103  Sun Filter  77254
```

Let's walk through this code. Since it's only one line of code, we'll break it up into parts to discuss.

First, we create a new variable, `inventory_sku` that we'll use to store the joined table.

Next, `outerjoin()` is the function, the instruction to Julia that we want to join two dataframes together. The types of join you can do are `innerjoin`, `leftjoin`, `rightjoin`, `outerjoin`, `semijoin`, `antijoin`, or `crossjoin`. I chose `outerjoin`, but since our two dataframes are a perfect match, we could have used `innerjoin`, `leftjoin`, or `rightjoin` and received the same result.

The first two items (known as "arguments") inside the `join()` function are the two dataframes we want to join, `inventory` and `sku`. Whichever dataframe you mention first inside the `join()` function will go on the left of our joined dataframe. The unique columns from the other dataframe will go on the right.

The third "argument" inside the `join()` function is `on = :item`.

This simply means that the `:item` column is the one we're using to match the correct rows from each dataframe. In SQL and other database languages, this is called a "primary key".

### More complex dataframe joins in Julia

Now, this was an easy join. We had four items in `inventory` and the same four items in `sku`. There were no missing rows, in other words.

In addition, this join was super easy because the `item` column had the same name in both dataframes.

If it had been named `item` in one dataframe, and `item_name` in the other, we would have had to rename one column. I'll cover this a bit later on. First up though: pivot tables.

## How to use pivot tables in Julia dataframes

Let's say we have a dataframe which stored the (fictional) number of pings the Hubble telescope received on each day over a two week period.

In this section, we're going to use a pivot table to roll up the daily Hubble telescope ping data into weeks.

First, we're going to use Julia's inbuilt `Dates` package.

Unlike the `DataFrames` package that we installed around the time we began this tutorial, we don't have to install the `Dates` package---it's built into the Julia application.

However, we do have to tell Julia that we want to use it. The box below describes how:

<aside>

### How to use dates in Julia

If you want to be able to add days to a date, to search for rows with dates earlier than a given date, or perform any other date-based operations, we first need to tell Julia that we're using dates.

You do that like this:

`using Dates`

Then, whenever you create a date in Julia, you do it like this:

`alan_turing_birthday = Date(1912,06,23)`

In the code above, we created a variable named `alan_turing_birthday` and set it to equal June 23, 1912.

As you may have gathered, the format inside `Date()` is yyyy,mm,dd -- we use commas to separate the year, month, and date (in that order).

</aside>

Onwards. So, if you haven't already, enter the code below into your Julia terminal:

`using Dates`

Now let's create our Hubble telescope pings dataframe with the code below. I've shown it in elongated form so you don't have to scroll horizontally to see it all.

```
hubble_pings = DataFrame(
	date = [
		Date(2020,03,16),
		Date(2020,03,17),
		Date(2020,03,18),
		Date(2020,03,19),
		Date(2020,03,20),
		Date(2020,03,21),
		Date(2020,03,22),
		Date(2020,03,23),
		Date(2020,03,24),
		Date(2020,03,25),
		Date(2020,03,26),
		Date(2020,03,27),
		Date(2020,03,28),
		Date(2020,03,29)
	],
	iso_week = [
		12,
		12,
		12,
		12,
		12,
		12,
		12,
		13,
		13,
		13,
		13,
		13,
		13,
		13
	],
	pings = [
		774,
		565,
		801,
		739,
		671,
		702,
		598,
		740,
		598,
		761,
		758,
		733,
		803,
		806
	]
)
```

We get:

```
14×3 DataFrame
│ Row │ date       │ iso_week │ pings │
│     │ Date       │ Int64    │ Int64 │
├─────┼────────────┼──────────┼───────┤
│ 1   │ 2020-03-16 │ 12       │ 774   │
│ 2   │ 2020-03-17 │ 12       │ 565   │
│ 3   │ 2020-03-18 │ 12       │ 801   │
│ 4   │ 2020-03-19 │ 12       │ 739   │
│ 5   │ 2020-03-20 │ 12       │ 671   │
│ 6   │ 2020-03-21 │ 12       │ 702   │
│ 7   │ 2020-03-22 │ 12       │ 598   │
│ 8   │ 2020-03-23 │ 13       │ 740   │
│ 9   │ 2020-03-24 │ 13       │ 598   │
│ 10  │ 2020-03-25 │ 13       │ 761   │
│ 11  │ 2020-03-26 │ 13       │ 758   │
│ 12  │ 2020-03-27 │ 13       │ 733   │
│ 13  │ 2020-03-28 │ 13       │ 803   │
│ 14  │ 2020-03-29 │ 13       │ 806   │
```

All that work we did earlier with setting up our dates correctly has paid off---notice how the type of the `date` column is `Date`, not `String`.

Now, you might be wondering what the `iso_week` column is. That's just the International Organization for Standardization week number for the given date.

In our dataframe, we have seven days in Week 12 (of the year 2020), and another seven in Week 13 (also of the year 2020).

We're going to use that `iso_week` column to pivot our data to get our weekly pings total.

The Julia code to make a pivot table is quite simple:

```
hubble_pings_weekly = by(
	hubble_pings,
	:iso_week,
	:pings => sum
)
```

Let's walk through this code. First, we tell Julia to store the pivot table in a new variable, `hubble_pings_weekly`.

Then we use the inbuilt `by()` function to create the actual pivot table.

Inside `by()`, we use three arguments.

The first of these is `hubble_pings`. This tells Julia the name of the source dataframe we want to pivot.

The second argument is `:iso_week`. This is the column in the source dataframe we want to use for grouping.

The third and final argument in the `by()` function is `:pings => sum`.

This tells Julia to take a given column, `:pings`, in the source dataframe and to `sum` them in our pivot table.

Take a look at our pivot table below---if you're playing along, input `hubble_pings_weekly` into your terminal.

We get:

```
2×2 DataFrame
│ Row │ iso_week │ pings_sum │
│     │ Int64    │ Int64     │
├─────┼──────────┼───────────┤
│ 1   │ 12       │ 4850      │
│ 2   │ 13       │ 5199      │
```

So, from the above dataframe (which we named `hubble_pings_weekly`), we can see there were 4,850 pings from the Hubble telescope in Week 12, and 5,199 pings in Week 13.

<aside>

### Why pivot tables are better in Julia compared to Excel or Google Sheets

The reason I love Julia pivot tables is they are a first-class citizen. You can take this `hubble_pings_weekly` dataframe, run transformations on it, and it works just like any other Julia dataframe.

By contrast, pivot table worksheets in Excel or Google Sheets are decidedly _not_ first-class citizens. Far too often, I had to manually copy and paste an Excel or Google Sheets pivot table into a new worksheet, just so I could run further transformations and analysis on it.

</aside>

OK, let's say you wanted to rename that `pings_sum` column to the cleaner, shorter `pings`. The next section will show you how to do that.


## How to rename columns in a Julia dataframe

OK, so we want to rename the `pings_sum` column from the dataframe above so it becomes `pings`.

We do that like this:

`rename!(hubble_pings_weekly, :pings_sum => :pings)`

We get:

```
2×2 DataFrame
│ Row │ iso_week │ pings     │
│     │ Int64    │ Int64     │
├─────┼──────────┼───────────┤
│ 1   │ 12       │ 4850      │
│ 2   │ 13       │ 5199      │
```

## How to add an array as a column to a dataframe in Julia

Julia makes it trivial to add a new column to your dataframe, especially if you already have the data in array form.

For example, if we have the following dataframe we've been using for some of this tutorial:

```
inventory = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"Lunar Rover",
		"30% Sun Filter"
	],
	id = [
		100,
		101,
		102,
		103
	],
	kind = [
		"Rover",
		"Spaceship",
		"Rover",
		"Sun Filter"
	]
)
```

... it looks like this:

```
4×3 DataFrame
 Row │ item            id     kind
     │ String          Int64  String
─────┼───────────────────────────────────
   1 │ Mars Rover        100  Rover
   2 │ Venus Explorer    101  Spaceship
   3 │ Lunar Rover       102  Rover
   4 │ 30% Sun Filter    103  Sun Filter
```

Let's say we want to add a new column called `condition`, where the values can be `Excellent`, `Fair` or `Poor`.

And let's say you already have an array with the condition of each item:

`condition = ["Fair", "Excellent", "Fair", "Poor"]`

To add this `condition` array as a column to our `inventory` dataframe, you do this:

`inventory[!, :condition] = condition`

When we call up our newly-expanded dataframe:

`inventory`

We get:

```
4×4 DataFrame
 Row │ item            id     kind        condition
     │ String          Int64  String      String
─────┼──────────────────────────────────────────────
   1 │ Mars Rover        100  Rover       Fair
   2 │ Venus Explorer    101  Spaceship   Excellent
   3 │ Lunar Rover       102  Rover       Fair
   4 │ 30% Sun Filter    103  Sun Filter  Poor
```

Let's walk through that code. First, we're telling Julia to add a new column (`:condition`) to each row (`!`) of the `inventory` dataframe, and that it should get this data from the array named `condition` (on the right hand side of the equals sign).

## How to write a CSV from a dataframe in Julia

It's easy to write a CSV from our above `inventory` dataframe.

First, you'll need to install the `CSV.jl` package if you haven't already. (I covered [how to install the `DataFrames.jl` package](#installing-the-dataframes-package) earlier---simply follow that but `add CSV` in the <code class="julia-pkg">pkg></code> area as well.)

Now, assuming you've already told Julia that you're using the DataFrames and CSV packages:

`using DataFrames, CSV`

... then we can write our CSV like this:

`CSV.write("/Users/ron/Desktop/inventory.csv", inventory)`

This takes our `inventory` dataframe and writes a CSV named `inventory.csv` to my Mac's desktop (you can save it wherever you'd like).

## How to import a CSV as a dataframe in Julia

Once we've installed the `CSV.jl` package already (see above section on writing CSV files), then we tell Julia we want to use it:

`using CSV`

Now we can import a CSV as a dataframe into Julia like this:

`df = CSV.File("my-file.csv") |> DataFrame`

In the code above, we're telling Julia that we want to create a new dataframe, and that we want to call it `df`. (You can name it whatever you want, though.)

Next, we tell Julia that we have a CSV file, and where to find it. In the example above, `my-file.csv` is located in the same folder as the Julia script that calls it.

However, if the CSV were in a different folder, you could reference it relative to the Julia script. For example, if the CSV is one folder higher than the Julia script, on a Mac you'd write this:

`df = CSV.File("../my-file.csv") |> DataFrame`

(The two dots `..` signify the parent directory in Unix and Mac systems.)

You can also reference the absolute location of the CSV. For instance, if the CSV were on my Mac desktop, I'd write this:

`df = CSV.File("/Users/ron/Desktop/my-file.csv") |> DataFrame`

Finally, the `|> DataFrame` bit tells Julia to create a dataframe from the CSV.

## How to convert an array into a dataframe in Julia

Let's say you have an array like this:

`space_companies = ["SpaceX", "Blue Origin", "Boeing", "Sierra Nevada Corporation"]`

... and you want to convert this array into a Julia dataframe.

First, tell Julia, if you haven't already, that we will be:

`using DataFrames`

This loads the DataFrames package into your workspace.

Now, assuming we want our new dataframe to be called `df`, we input this:

`df = DataFrame([space_companies], [:space_brands])`

We get:

```
4×1 DataFrame
 Row │ space_brands
     │ String
─────┼───────────────────────────
   1 │ SpaceX
   2 │ Blue Origin
   3 │ Boeing
   4 │ Sierra Nevada Corporation
```

Let's walk through the code above now.

> `df =`
>
> We are creating a new variable, which we have named `df`.

> `DataFrame(`
>
> Here we tell Julia to create a dataframe, and we open the brackets, inside of which we'll be configuring said dataframe.

> `[space_companies]`
>
> Here we're telling Julia which array (`space_companies`) we want to reference for our dataframe's content. Important: The array name must be inside square brackets `[]`. The reason why can be slightly confusing, but it's because Julia is expecting a list (otherwise known as an array) of array names, and just because we happen to only have one array (`space_companies`) in this example, doesn't mean Julia isn't able to handle more---and therefore, it expects an array of array names. If that's confusing then just remember to put square brackets around your array name.

> `,`
>
> We separate our array name from the next bit of config with a comma.

> `[:space_brands]`
>
> Here we tell Julia what we want to name the column in our new dataframe. In this case, I've gone for `space_brands` as the column name, but we have to add some additional characters to the name. We first add a colon `:` to signify that this is a dataframes column name (it's part of the DataFrames standard). Next, we have to wrap the colon and column name in square brackets `[]`. So instead of `:space_brands`, we write `[:space_brands]`. The reason for the square brackets is that Julia needs to be ready for more than one array/column name combo, and therefore expects an array---even if you only have one array and column name.

> `)`
>
> Here we close the brackets we opened earlier.
