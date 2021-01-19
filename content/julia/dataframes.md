---
title: How to use dataframes in Julia
date: 2020-03-01 11:00:00+11:00
seoTitle: Julia dataframes tutorial
description: "Julia dataframes let you do anything you want: pivot tables, data cleaning, table joins, filtering, and more, all with a nice clean syntax."
authors: ["Ron Erdos"]
tableOfContents: true
version: 1.3.1
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

It will have three columns: `item`, `quantity`, and `description`.

## If you want to play along

If you want to play along (as opposed to just reading through this tutorial), you'll need to [download, install, and run Julia](../setup/) if you haven't already.

Once you have Julia up and running, you'll see this prompt:

`julia`

This prompt will be green in color. Yeah, like in _The Matrix_.

Onwards!

## Installing the DataFrames package

The first dataframes-specific thing we need to is to install the DataFrames package. This is a one-time task.

The easiest way to do this is to type the `]` (right square bracket, located above the return/Enter key on your keyboard) into the Julia terminal.

This changes the `julia` prompt we saw above into one that says:

<code class="julia-pkg">pkg></code>

This prompt will be blue in colour.

This means we are now in Julia's package manager (hence the abbreviation <code class="julia-pkg">pkg</code>
). We can install packages with a lot less typing in here (compared to at the green `julia` prompt).

Go ahead and add the DataFrames package by typing in the line below and hitting return/Enter:

`add DataFrames`

Julia's package manager will do its thing and install the DataFrames package for you.

You'll be taken back to the package manager prompt:

<code class="julia-pkg">pkg></code>

To exit the package manager area, and to go back to the main Julia area, simply hit the delete key. How cool is that?

You'll now see the Julia prompt again:

`julia`

Now we can actually create our NASA inventory dataframe.

## How to create a Julia dataframe in the terminal

First, we need to tell Julia we want to use the DataFrames package we just installed.

We do that by entering the line below---make sure to press return/Enter at the end:

`using DataFrames`

Next, we can enter the lines below (again, pressing return/Enter at the end). This will create our NASA inventory dataframe.

### Code for playing along

```
inventory = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"%Lunar Rover",
		"30% Sun Filter"
	],
	quantity = [
		3,
		1,
		12,
		100
	],
	description = [
		"mars buggy",
		"venus spaceship",
		"Neil's used car",
		"Sun filter"
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

### Julia output

You'll end up with this output in your terminal:

```
4×3 DataFrame
│ Row │ item           │ quantity │ description     │
│     │ String         │ Int64    │ String          │
├─────┼────────────────┼──────────┼─────────────────┤
│ 1   │ Mars Rover     │ 3        │ mars buggy      │
│ 2   │ Venus Explorer │ 1        │ venus spaceship │
│ 3   │ %Lunar Rover   │ 12       │ Neil's used car │
│ 4   │ 30% Sun Filter │ 100      │ Sun filter      │
```

Let's quickly talk through what we see above.

### Quick tour of a Julia dataframe

The first words of the output above, `4x3 DataFrame` says that we have a 4-row, 3-column dataframe. Simple enough.

Next, we have the column headers, `Row`, `item`, `quantity`, and `description`.

The first of these, `Row`, is not something we asked for, though it is very useful. It's a column of row numbers that Julia generates automatically. But don't worry, they're not part of the dataframe proper, nor will they show up in CSV exports and so forth.

The next column headers, `item`, `quantity`, and `description` are those we supplied to Julia in our code above.

<aside>

### Julia variable naming conventions

When it comes to naming variables in Julia, I follow convention and keep them all lowercase. For variable names with multiple words (e.g. `my_new_column`), I use "snake case": all lowercase, with underscores separating the words.

</aside>

Next up, we have the **type** of each column. Let's go over what that means now.

We see `String`, `Int64`, and another `String`.

The `Row` column doesn't have a type because, as mentioned above, it's not part of the dataframe proper. It's just there to help you keep track.

Our first "real" column, `item`, has the **type** `String`.

This means that anything that goes in this column will be treated as text.

The next column, `quantity`, has the type `Int64`. Although this might look weird, it just means "integer" (a whole number, not a decimal). The `64` just refers to 64 bit, which means huge numbers can be stored here. "Huge numbers" is my technical term for the range of integers between `-2^63` and `2^63 - 1`.

Finally, the last column, `description`, is another `String`-type column, the same as the `item` column.

And finally, we have the actual data.

## How to "find and replace" in a Julia dataframe

As mentioned above, I've deliberately included some typos in the `inventory` dataframe, so we can practise "find and replace".

In the `description` column, `mars` should be `Mars`, and `venus` should be `Venus`.

Let's clean that up.

Here's how you do that:

### Code for playing along

```
for i in eachrow(inventory)
	i[:description] = replace(i[:description], "mars"=>"Mars")
	i[:description] = replace(i[:description], "venus"=>"Venus")
end
```

Talking through the code above, it says "for each row in the `inventory` dataframe, within the `description` column, replace `mars` with `Mars` (because grammar) and `venus` with `Venus` (also because grammar).

Alright, so you've typed or copied and pasted the code above into Julia and hit the return/Enter key.

But now all we see is the Julia prompt again:

`julia`

What gives? Well, if you type in `inventory` and hit the return/Enter key, you'll see that the planets in our `description` column now start with a capital letter.

### Julia output

```
4×3 DataFrame
│ Row │ item           │ quantity │ description     │
│     │ String         │ Int64    │ String          │
├─────┼────────────────┼──────────┼─────────────────┤
│ 1   │ Mars Rover     │ 3        │ Mars buggy      │
│ 2   │ Venus Explorer │ 1        │ Venus spaceship │
│ 3   │ %Lunar Rover   │ 12       │ Neil's used car │
│ 4   │ 30% Sun Filter │ 100      │ Sun filter      │
```

So Julia _has_ done the requested find and replace, you just have to ask it to show you the output (which we did by typing `inventory` and hitting return/Enter).

OK, moving on.

There's one more typo in the `inventory` dataframe.

In the `item` column, `%Lunar Rover` should be `Lunar Rover`.

Let's clean that up too. But we'll need to be careful---we don't want to remove the percentage sign from `30% Sun Filter`. After all, it's a thirty per cent sun filter, and that's how it should stay.

Read on to see how to do this. We're going to use regex.

## How to "find and replace" with regex in a Julia dataframe

We're going to use regex (regular expressions) to do our find and replace.

It looks like this:

### Code for playing along

```
for i in eachrow(inventory)
	i[:item] = replace(i[:item], r"^%"=>"")
end
```

In the code above, you can see in the second line that we use `r` at the beginning of the text to find, i.e. `r"^%"`.

The `r` simply means we are using regex.

Then we have double quote marks surrounding our regular expression.

The regular expression itself consists of a caret i.e. `^` and the character we want to match, the `%` sign.

If you know regex, you'll know that the caret (in this context) means "the start of the string".

So `^%` means "a percentage sign at the start of a string". Any percentage signs _not_ at the start of the string will not be matched. So our regex won't match `30% Sun Filter`---just as we intended.

Moving on, we have a "fat arrow" i.e. `=>`. This signifies that whatever comes next is the text to replace our match(es) with.

In this particular case, our replace text is blank i.e. `""`, because we want to replace the percentage sign at the start of `%Lunar Rover` with nothing, i.e. delete it.

Thus, we end up with:

### Julia output

```
4×3 DataFrame
│ Row │ item           │ quantity │ description     │
│     │ String         │ Int64    │ String          │
├─────┼────────────────┼──────────┼─────────────────┤
│ 1   │ Mars Rover     │ 3        │ Mars buggy      │
│ 2   │ Venus Explorer │ 1        │ Venus spaceship │
│ 3   │ Lunar Rover    │ 12       │ Neil's used car │
│ 4   │ 30% Sun Filter │ 100      │ Sun filter      │
```

There we are---all cleaned up for the ball!

## How to filter a dataframe in Julia

Let's say, for the dataframe above, we only want the rows which mention "Rover".

If you're just joining us now, here's the code to create that dataframe:

### Code for playing along

```
inventory = DataFrame(
	item = [
		"Mars Rover",
		"Venus Explorer",
		"Lunar Rover",
		"30% Sun Filter"
	],
	quantity = [
		3,
		1,
		12,
		100
	],
	description = [
		"Mars buggy",
		"Venus spaceship",
		"Neil's used car",
		"Sun filter"
	]
)
```

To get just the rows with "Rover" in the item column, use this code:

### Code for playing along

```
rovers = filter(
	x -> any(occursin.(["Rover"], x.item)),
	inventory
)
```

### Julia output

```
2×3 DataFrame
│ Row │ item        │ quantity │ description              │
│     │ String      │ Int64    │ String                   │
├─────┼─────────────┼──────────┼──────────────────────────┤
│ 1   │ Mars Rover  │ 3        │ Mars buggy               │
│ 2   │ Lunar Rover │ 12       │ Neil's used car          │
```

### Code walkthrough

As usual on this site, I've broken up the code above for easier reading.

First, we create a variable, `rovers`, which we'll use to store the filtered, Rover-only version of the master dataframe, `inventory`. No Venus Explorers welcome here!

On the other side of the equals sign, we're using the `filter` function, which takes two arguments.

The first argument is the change we want to make, which in our case will be to identify all rows in the `item` column containing the word "Rover".

The second argument is the dataframe we want to change. In our case, that's `inventory`.

Let's go back to the first argument---the identification of each "Rover" in the `item` column.

It looks like this:

`x -> any(occursin.(["Rover"], x.item))`

If you look closely, that's actually a function. It's not a named function like `print("Hello world")`, which is sensibly named "print".

Rather, it's an "anonymous" function, because it doesn't have a name. Specifically, it's a "stabby lambda" anonymous function. "Stabby" because it has a stabby arrow: `->`.

And the reason we're using an anonymous rather than named function is because we're only using it once, and it's more concise this way. Whereas our `print` function we might use every second day.

So what does this "stabby lambda" anonymous function actually do?

Right, now we're getting down to it. Bear with me. It says for a given `x`, are there `any` occasions where `"Rover"` occurs in the item column (`x.item`).

And because this stabby lambda function is inside another function, the `filter()` function, we're doing some filtering. As above, the second argument inside `filter()` is simply the dataframe we want Julia to filter: `inventory`. So that's how we end up with a dataframe named `rovers` which is Rover-only subset of our `inventory` dataframe.

## How to join two dataframes in Julia

Did you know Julia can join two tables just like SQL?

### Creating our second dataframe

Let's say we have another table which lists each `item` and its corresponding SKU (stock keeping unit).

If you want to play along, here's the Julia code to copy and paste into your terminal:

### Code for playing along

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

And here's the Julia dataframe output:

### Julia output

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

OK, so we have a 4-row, 2-column dataframe. It's called `sku`, because that's what we named it in the Julia code above.

Moving on, each `item` is stored as plain text (i.e. a `String`), and each `sku` is an integer (specifically, an `Int64` integer).

### Doing the actual join

Now it's time to combine (join) this new dataframe with the dataframe from earlier, `inventory`.

It's actually really easy to do this in Julia. It's only one line of code:

### Code for playing along

`inventory_sku = join(inventory, sku, on = :item)`

Let's walk through this code. First, we create a new variable, `inventory_sku` that we'll use to store the joined table.

Next, `join()` is the function, the instruction to Julia that we want to join two dataframes together.

The first two items (known as "arguments") inside the `join()` function are the two dataframes we want to join, `inventory` and `sku`. Whichever dataframe you mention first inside the `join()` function will go on the left of our joined dataframe. The unique columns from the other dataframe will go on the right.

The third "argument" inside the `join()` function is this code: `on = :item`.

This simply means that the `:item` column is the one we're using to match the correct rows from each dataframe. In SQL and other database languages, this is called a "primary key".

And here's our output:

### Julia output

```
4×4 DataFrame
│ Row │ item           │ quantity │ description     │ sku   │
│     │ String         │ Int64    │ String          │ Int64 │
├─────┼────────────────┼──────────┼─────────────────┼───────┤
│ 1   │ Mars Rover     │ 3        │ Mars buggy      │ 34566 │
│ 2   │ Venus Explorer │ 1        │ Venus spaceship │ 78945 │
│ 3   │ Lunar Rover    │ 12       │ Neil's used car │ 15179 │
│ 4   │ 30% Sun Filter │ 100      │ Sun filter      │ 77254 │
```

Pretty cool, right?

### More complex dataframe joins in Julia

Now, this was an easy join. We had four items in `inventory` and the same four items in `sku`. There were no missing rows, in other words.

In addition, this join was super easy because the `item` column was identical in both dataframes.

If it had been named `item` in one dataframe, and `item_name` in the other, we would have had to rename one column.

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

### Code for playing along

`using Dates`

Now let's create our Hubble telescope pings dataframe:

### Code for playing along

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

And here's the resultant dataframe. Remember to type `hubble_pings` and hit return/Enter to see it.

### Julia output

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

The actual pivot table code is very simple; it's just one line.

### Code for playing along

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

Take a look at our pivot table below---if you're playing along, type `hubble_pings_weekly` into your terminal and hit return/Enter:

### Julia output

```
2×2 DataFrame
│ Row │ iso_week │ pings_sum │
│     │ Int64    │ Int64     │
├─────┼──────────┼───────────┤
│ 1   │ 12       │ 4850      │
│ 2   │ 13       │ 5199      │
```

<aside>

### Why pivot tables are better in Julia compared to Excel or Google Sheets

The reason I love Julia pivot tables is they are a first-class citizen. You can take this `hubble_pings_weekly` dataframe, run transformations on it, and it will act just like any other Julia dataframe.

By contrast, pivot table worksheets in Excel or Google Sheets are decidedly _not_ first-class citizens.

There are so many times I can remember where I had to manually copy and paste an Excel or Google Sheets pivot table into a new worksheet, just so I could run further transformations and analysis on it.

</aside>

OK, let's say you wanted to rename that `pings_sum` column to the cleaner, shorter `pings`. The next section will show you how to do that.

## How to rename columns in a Julia dataframe

OK, so we want to rename the `pings_sum` column from the dataframe above so it becomes `pings`.

### Code for playing along

It's actually just one line of code:

`rename!(hubble_pings_weekly, :pings_sum => :pings)`

And here's the result---if you're playing along, you'll need to enter `hubble_pings_weekly` followed by the return/Enter key into your terminal to see it.

### Julia output

```
2×2 DataFrame
│ Row │ iso_week │ pings     │
│     │ Int64    │ Int64     │
├─────┼──────────┼───────────┤
│ 1   │ 12       │ 4850      │
│ 2   │ 13       │ 5199      │
```
