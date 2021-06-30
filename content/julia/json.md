---
title: How to convert nested JSON to a dataframe or CSV in Julia
date: 2021-06-25 16:44:59+11:00
seoTitle:
description: Do you want to use Julia to turn nested JSON into a table, be it a dataframe or CSV? Here's how.
authors: ["Ron Erdos"]
tableOfContents: true
draft: true
---

## Inputs and outputs

Let's say you have the following nested JSON:

```
{
    "id": 1234,
    "org": "SpaceX",
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

| id   | org    | launcher     | max_payload |
|------|--------|--------------|-------------|
| 1234 | SpaceX | Falcon 1     | 180         |
| 1234 | SpaceX | Falcon 9     | 22800       |
| 1234 | SpaceX | Falcon Heavy | 63800       |

This tutorial will show you how to do that using Julia. Let's get started!

## Converting the JSON to a string variable

First, we need to escape the double quotes with a backslash, so that we can create this JSON as a string in Julia.

I did this with a simple find and replace in VS Code, yielding:

```
{
    \"id\": 1234,
    \"org\": \"SpaceX\",
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

Now let's create this as variable in Julia:

```
spacex = "{
    \"id\": 1234,
    \"org\": \"SpaceX\",
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

All we've done here is create the variable name (`spacex =`) and then wrap double quotes around all that escaped JSON we prepared earlier.

If we check the type of our `spacex` variable:

`typeof(spacex)`

We get:

`String`

All well and good. Now let's turn this `String` into structured, usable data.

## Converting our string variable into structured data

To do this, we need to add the `JSON` package.

<aside>

### Installing the JSON package in Julia

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

Now we can go ahead and actually use it:

`spacex_dict = JSON.parse(spacex)`

In the line of code above, we're creating a new variable, `spacex_dict`, by parsing the `spacex` data with the `JSON` package.

The value of our `spacex_dict` variable is:

```
Dict{String, Any} with 3 entries:
  "id"        => 1234
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

So we have a dictionary (`Dict`). That's great---it's more structured than the simple `String` we had started with.

## Creating a dataframe from our structured data variable

Okay, let's go ahead and turn our `spacex_dict` data into a dataframe. We're going to use the DataFrames package.

<aside>

### Installing the Dataframes package in Julia

If you don't have the Julia dataframes package installed, go to the package manager by inputting `]` and then inputting `add DataFrames`. You can return to the Julia prompt by pressing the delete key once the package has been installed.

</aside>

Now let's tell Julia we want to use the DataFrames package:

`using DataFrames`

Now we can actually create a dataframe from our data. I'm going to use the conventional `df` as our variable name, but you can choose anything you like:

`df = DataFrame(spacex_dict)`

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
   1 │ Dict{String, Any}("max_payload"=…   1234  SpaceX
   2 │ Dict{String, Any}("max_payload"=…   1234  SpaceX
   3 │ Dict{String, Any}("max_payload"=…   1234  SpaceX
```

Again, note the terminal output---in this case, the `equipment` column---has been truncated. That's because this is how it shows up for me in my terminal on my 13" laptop. Again, all the data is there in memory, which is what counts.

The above dataframe is _sorta_ what we want: the non-nested JSON (the `id` and `org` columns) is appearing properly in distinct columns.

However, the nested JSON---the `launcher`'s name and its `max_payload` for low earth orbit---has not been separated into distinct columns, so the `equipment` column is a bit of a mess.

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

Okay, so our newly-created `equipment_col` is a 3-element vector.

We're going to need that number 3 later, so let's go ahead and give it its own variable:

`equipment_col_length = length(equipment_col)`

We get:

`3`

Before we go on, let's recall our desired end result. We want a dataframe or CSV that looks like this:

| id   | org    | launcher     | max_payload |
|------|--------|--------------|-------------|
| 1234 | SpaceX | Falcon 1     | 180         |
| 1234 | SpaceX | Falcon 9     | 22800       |
| 1234 | SpaceX | Falcon Heavy | 63800       |

However, our current dataframe looks like this:

```
3×3 DataFrame
 Row │ equipment                          id     org
     │ Any                                Int64  String
─────┼──────────────────────────────────────────────────
   1 │ Dict{String, Any}("max_payload"=…   1234  SpaceX
   2 │ Dict{String, Any}("max_payload"=…   1234  SpaceX
   3 │ Dict{String, Any}("max_payload"=…   1234  SpaceX
```

So we have the `id` and `org` columns already, but we don't have the `launcher` and `max_payload` columns. We _do_ have an `equipment` column, but it's a mess, so we'll delete it once we've extracted its data.

To that end, let's create empty variables for our desired new columns:

```
launchers_array = String[]
max_payloads_array = Int64[]
```

These will become our desired columns later.

Now for probably the hardest bit of the whole tutorial. Hang in there! We're going to iterate over the `equipment` column, grab each `launcher`'s name (`Falcon 1`, `Falcon 9` etc) and its `max_payload`, and put them in the empty arrays we just created earlier.

The code looks like this:

```
for i in 1:equipment_col_length
    push!(launchers_array, equipment_col[i]["launcher"])
    push!(max_payloads_array, equipment_col[i]["max_payload"])
end
```


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

 `df[!, :launchers] = launchers_array`

 `df[!, :max_payloads] = max_payloads_array`

 Let's take a look so far

 `df`

 We get:

 ```
 3×5 DataFrame
 Row │ equipment                          id     org     launchers     max_payloads
     │ Any                                Int64  String  String        Int64
─────┼──────────────────────────────────────────────────────────────────────────────
   1 │ Dict{String, Any}("max_payload"=…   1234  SpaceX  Falcon 1               180
   2 │ Dict{String, Any}("max_payload"=…   1234  SpaceX  Falcon 9             22800
   3 │ Dict{String, Any}("max_payload"=…   1234  SpaceX  Falcon Heavy         63800
```

(Remember you can scroll horizontally on this site inside the code boxes to see the full output.)

There's just one thing left to do---we no longer need the unstructured `payloads` column. We delete it like this:

`select!(df, Not(:equipment))`

We get our finished, fully-structured dataframe:

```
3×4 DataFrame
 Row │ id     org     launchers     max_payloads
     │ Int64  String  String        Int64
─────┼───────────────────────────────────────────
   1 │  1234  SpaceX  Falcon 1               180
   2 │  1234  SpaceX  Falcon 9             22800
   3 │  1234  SpaceX  Falcon Heavy         63800
```

## Writing our finished dataframe to a CSV

We can now trivially write our finished dataframe to a CSV.

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

Finally, we can write our CSV file with the contents of our SpaceX dataframe, which we named `df`:

`CSV.write("df.csv", df)`
