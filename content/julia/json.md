---
title: How to convert nested JSON to a dataframe or CSV in Julia
date: 2021-07-06 22:55:59+11:00
seoTitle: "Nested JSON in Julia: How to convert to dataframe"
description: You can't do it with one function, but you can do it!
authors: ["Ron Erdos"]
tableOfContents: true
---

## Inputs and outputs

Let's say you have the following nested JSON:

```
{
    "org": "SpaceX",
    "id": 123,
    "equipment": [
        {
            "launcher": "Falcon 1",
            "max_payload": 180
        },
        {
            "launcher": "Falcon 9",
            "max_payload": 22800
        },
        {
            "launcher": "Falcon Heavy",
            "max_payload": 63800
        }
    ]
}
```

... and you want to use Julia to convert it to a CSV like this:

| org    | id  | launcher     | max_payload |
|--------|-----|--------------|-------------|
| SpaceX | 123 | Falcon 1     | 180         |
| SpaceX | 123 | Falcon 9     | 22800       |
| SpaceX | 123 | Falcon Heavy | 63800       |

This tutorial will show you how to do that using Julia. Let's get started!

## Converting the JSON to a string variable

First, we need to escape the double quotes with a backslash, so that we can create this JSON as a string in Julia.

I did this with a simple find and replace in VS Code, yielding:

```
{
    \"org\": \"SpaceX\",
    \"id\": 123,
    \"equipment\": [
        {
            \"launcher\": \"Falcon 1\",
            \"max_payload\": 180
        },
        {
            \"launcher\": \"Falcon 9\",
            \"max_payload\": 22800
        },
        {
            \"launcher\": \"Falcon Heavy\",
            \"max_payload\": 63800
        }
    ]
}
```

Now let's create this as a Julia variable, `spacex`, by wrapping the escaped JSON in double quotes:

```
spacex = "{
    \"org\": \"SpaceX\",
    \"id\": 123,
    \"equipment\": [
        {
            \"launcher\": \"Falcon 1\",
            \"max_payload\": 180
        },
        {
            \"launcher\": \"Falcon 9\",
            \"max_payload\": 22800
        },
        {
            \"launcher\": \"Falcon Heavy\",
            \"max_payload\": 63800
        }
    ]
}"
```

So what are we working with here? What sort of variable is `spacex`?

Let's check:

`typeof(spacex)`

We get:

`String`

Okay, `spacex` is a `String`. Now we need to turn it into structured data so we can use it.

Enter the `JSON.jl` package.

## Converting our string variable into structured data

Let's go ahead and install the `JSON.jl` package. Instructions are in the box below.

<aside>

### Installing the JSON.jl package in Julia

At the Julia prompt:

`julia>`

... input a closing square bracket:

`]`

Now the prompt will look like this:

<code class="julia-pkg">pkg></code>

We have entered Julia's package manager area.

Now we can easily add the `JSON` package:

`add JSON`

Once the package has been installed, we are shown the package manager prompt again:

<code class="julia-pkg">pkg></code>

Hit the delete key to return to the regular prompt:

`julia>`

Now we can get to work.

</aside>

First, let's tell Julia we want to _use_ the package:

`using JSON`

Now we can go ahead and actually use it. We can do so like this:

`spacex_dict = JSON.parse(spacex)`

In the line of code above, we're creating a new variable, `spacex_dict`, by parsing the `spacex` data with the `JSON` package.

The value of our `spacex_dict` variable is:

```
Dict{String, Any} with 3 entries:
  "id"        => 123
  "org"       => "SpaceX"
  "equipment" => Any[Dict{String, Any}("max_payload"=>180, "launcher"=>"Falcon 1"), Dict{String, Any}("max_payload"=>22800, "launcher"=>"Falcon 9"), Dict{String, Any}("max_paylo…
```

<aside>

### Truncated terminal output in Julia

You can scroll horizontally inside the code block above.

However, please note that the terminal output above has been truncated---even when you scroll to the end---since that's how it showed up for me in my terminal on my 13" laptop. Nevertheless, all our data is there in memory, which is what counts.

</aside>

Let's go ahead and check the type of our new `spacex_dict` variable:

`typeof(spacex_dict)`

We get:

`Dict{String, Any}`

So now we have a dictionary---our `spacex_dict` variable---to work with.

That's great---it's way more structured than our original variable, `spacex`, which was just a simple `String`.

Now it's time to make a table---specifically, a `DataFrame`---from our `spacex_dict` variable.

Enter the `DataFrames.jl` package.

## Creating a dataframe from our structured data variable

Let's install the `DataFrames.jl` package. The instructions are in the box below.

<aside>

### How to install the Dataframes package in Julia

At the Julia prompt, enter to the package manager area by inputting `]`.

The prompt will change to this:

<code class="julia-pkg">pkg></code>

Next, input `add DataFrames`.

Once it's done, you can return to the Julia prompt by pressing the delete key.

</aside>

We now need to tell Julia we want to use the DataFrames package:

`using DataFrames`

Now we can actually create a dataframe from our data. I'm going to use the conventional `df` as our variable name, but you can choose any variable name you like. Here's my code:

`df = DataFrame(spacex_dict)`

This code says "make a variable named `df` which contains a dataframe of the `spacex_dict` dictionary.

<aside>

### Singular vs plural: "DataFrame" vs "DataFrames"

Note that `DataFrame` is singular in the code above---because we're only creating one dataframe.

However, we use the plural `DataFrames` when we add it in the package manager and when we tell Julia we'll be `using` it.

</aside>

We get:

```
3×3 DataFrame
 Row │ equipment                          id     org
     │ Any                                Int64  String
─────┼──────────────────────────────────────────────────
   1 │ Dict{String, Any}("max_payload"=…    123  SpaceX
   2 │ Dict{String, Any}("max_payload"=…    123  SpaceX
   3 │ Dict{String, Any}("max_payload"=…    123  SpaceX
```

<aside>

### Truncation in the terminal

Again, note the terminal output---in this case, the `equipment` column---has been truncated. That's because this is how it shows up for me in my terminal on my 13" laptop. Again, all the data is there in memory, which is what counts.

</aside>

The above dataframe is _sort of_ what we want. The good news is that the non-nested JSON (the `id` and `org` columns) is appearing properly in distinct columns.

However, the nested JSON---the `launcher` names and their `max_payload`s for low earth orbit---have not been separated into distinct columns, so the `equipment` column is a bit of a mess.

We'll fix that next.

## Cleaning up the dataframe

Let's take a closer look at the `equipment` column. Let's make a variable called `equipment_col` so we can do just that:

`equipment_col = df[!, :equipment]`

I've written a tutorial on [Julia dataframes](../dataframes/) if you're not familiar with them, but let's walk through this code briefly:

> `equipment_col =` Here we're creating a new variable, `equipment_col` to use later.

> `df[!, :equipment]` From the dataframe named "df" (`df[]`), get all rows (signified with an exclamation mark `!`) in the `equipment` column (signified with a colon and then the column name: `:equipment`).

So the value of our newly-created `equipment_col` variable is:

```
3-element Vector{Any}:
 Dict{String, Any}("max_payload" => 180, "launcher" => "Falcon 1")
 Dict{String, Any}("max_payload" => 22800, "launcher" => "Falcon 9")
 Dict{String, Any}("max_payload" => 63800, "launcher" => "Falcon Heavy")
```

(Remember you can scroll horizontally in the code block above.)

Before we go on, let's recall our desired end result. We want a dataframe or CSV that looks like this:

| org    | id  | launcher     | max_payload |
|--------|-----|--------------|-------------|
| SpaceX | 123 | Falcon 1     | 180         |
| SpaceX | 123 | Falcon 9     | 22800       |
| SpaceX | 123 | Falcon Heavy | 63800       |

However, our current dataframe looks like this:

```
3×3 DataFrame
 Row │ equipment                          id     org
     │ Any                                Int64  String
─────┼──────────────────────────────────────────────────
   1 │ Dict{String, Any}("max_payload"=…    123  SpaceX
   2 │ Dict{String, Any}("max_payload"=…    123  SpaceX
   3 │ Dict{String, Any}("max_payload"=…    123  SpaceX
```

So we have the `id` and `org` columns already, but we don't have the `launcher` and `max_payload` columns. We _do_ have an `equipment` column, but it's a mess, so we'll delete it once we've extracted its data.

To that end, let's create empty variables for our desired new columns:

```
launchers_array = String[]
max_payloads_array = Int64[]
```

These will become our desired columns later.

Now for probably the hardest bit of the whole tutorial. Hang in there! We're going to iterate over the `equipment` column, grab each `launcher`'s name (`Falcon 1`, `Falcon 9` etc) and its `max_payload`, and put them in the empty arrays we just created earlier.

<aside>

Before we go on, let's cover how to drill down to get specific data out of a Julia dictionary.

In our `equipment` column, we have a dictionary in each row.

In the first row, we have:

`Dict{String, Any}("max_payload" => 180, "launcher" => "Falcon 1")`

If you want to get the name of that `launcher` (i.e. `"Falcon 1"`), you can do it like this:

`equipment_col[1]["launcher"]`

We get:

`"Falcon 1"`

Walking through this code, if you recall, earlier we created the variable `equipment_col` and filled it with the `equipment` column of our dataframe.

Next to that, we write `[1]` to tell Julia we want the first row.

And finally, we write `["launcher"]` to tell Julia we want the **value** of the `launcher` key/value pair, which in this case is `"Falcon 1"`.

We're going to use this approach to get the values of *all* the launcher names, as well as their maximum payloads.

</aside>

The code looks like this:

```
for i in 1:length(equipment_col)
    push!(launchers_array, equipment_col[i]["launcher"])
    push!(max_payloads_array, equipment_col[i]["max_payload"])
end
```

Let's walk through that code line by line:

> `for i in 1:length(equipment_col)` Here we are iterating over the number of items in our equipment column. In our SpaceX example, we have three rows (one each for Falcon 1, Falcon 9, and Falcon Heavy). So in our iterator, `i` will equal `1`, then `2`, and finally `3`. We'll use this changing value of `i` below to get our launcher names and their max payloads.

> `push!(launchers_array, equipment_col[i]["launcher"])` Now we are adding (`push!`) the name of each launcher ("Falcon 1", "Falcon 9" etc) into our `launchers_array`. When `i` is equal to `1`, we are adding `equipment_col[1]["launcher"]`, which is equal to `"Falcon 1"`. When `i` is equal to `2`, we are adding `equipment_col[2]["launcher"]`, which equals `"Falcon 9"`. And finally, when `i` equals `3`, we're adding the launcher name (`"Falcon Heavy"`) from the third element in our `equipment_col`. If this is at all unclear, let me know and I'll do a tutorial just on this.

> `push!(max_payloads_array, equipment_col[i]["max_payload"])` Here we're doing the same thing as the line above, but instead of adding launcher names into the launchers array, we're adding the maximum payload for each launcher into our `max_payloads_array`.

> `end` Here we close the `for` loop we started a few lines earlier.

Let's take a look at our new `launchers_array`:

```
3-element Vector{Int64}:
   180
 22800
 63800
 ```

 And our new `max_payloads_array`:

 ```
 3-element Vector{String}:
 "Falcon 1"
 "Falcon 9"
 "Falcon Heavy"
 ```
 
 Great, everything's in order.

 Now we can add them to our dataframe:

 `df[!, :launcher] = launchers_array`

 `df[!, :max_payload] = max_payloads_array`

 The two lines of code above add the `launchers_array` and the `max_payloads_array` respectively as columns to our dataframe.

 Let's take a look so far:

 `df`

 We get:

 ```
3×5 DataFrame
 Row │ equipment                          id     org     launcher      max_payload
     │ Any                                Int64  String  String        Int64
─────┼─────────────────────────────────────────────────────────────────────────────
   1 │ Dict{String, Any}("max_payload"=…    123  SpaceX  Falcon 1              180
   2 │ Dict{String, Any}("max_payload"=…    123  SpaceX  Falcon 9            22800
   3 │ Dict{String, Any}("max_payload"=…    123  SpaceX  Falcon Heavy        63800
```

(Remember you can scroll horizontally inside the code box above to see the full output.)

At this point, we no longer need the unstructured `equipment` column---we have replaced it fully with our new, structured columns `launcher` and `max_payload`. We delete the `equipment` column like this:

`select!(df, Not(:equipment))`

The above code says "select all columns from our `df` dataframe but `Not` the column named `equipment`". Or in plain English: delete the column named `equipment` from our dataframe.

We get our fully-structured dataframe:

```
3×4 DataFrame
 Row │ id     org     launcher      max_payload
     │ Int64  String  String        Int64
─────┼──────────────────────────────────────────
   1 │   123  SpaceX  Falcon 1              180
   2 │   123  SpaceX  Falcon 9            22800
   3 │   123  SpaceX  Falcon Heavy        63800
```

If you want to reorder the columns so that `org` comes before `id`:

`select!(df, [:org, :id, :launcher, :max_payload])`

Now everything is in order and complete:

```
3×4 DataFrame
 Row │ org     id     launcher      max_payload
     │ String  Int64  String        Int64
─────┼──────────────────────────────────────────
   1 │ SpaceX    123  Falcon 1              180
   2 │ SpaceX    123  Falcon 9            22800
   3 │ SpaceX    123  Falcon Heavy        63800
```

## Writing our finished dataframe to a CSV

We can now trivially write our finished dataframe to a CSV.

First, you'll need to install the `CSV.jl` package if you haven't already. Instructions are in the box below.

<aside>

### Installing the Julia CSV package

If you haven't already installed the `CSV.jl` package, you can do that by typing `]` at the Julia prompt. You'll then see the blue package manager prompt:

<code class="julia-pkg">pkg></code>

Here you input:

`add CSV`

Once the CSV package is installed, hit the delete key to return to the regular Julia prompt.

</aside>

Now we need to tell Julia we want to use the CSV package:

`using CSV`

Finally, we can write our CSV file with the contents of our `df` dataframe:

`CSV.write("df.csv", df)`

The code above says "write a CSV named `df.csv` (you can name it anything you like) with the contents of the `df` dataframe".

Using the code above, the CSV will be written to the same folder as your Julia script. If you're coding this directly into your terminal, then the CSV will be written to your home directory. On my Mac, this is at `/Users/ron/`.

If you want the CSV to end up in a specific place, you can do it like this:

`CSV.write("/Users/ron/Desktop/df.csv", df)`

The above code will write the CSV to the desktop of my Mac. Remember to change the `/ron/` part if you run this code for yourself!
