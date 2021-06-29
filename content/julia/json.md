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
    "launchers": [
        {
            "name": "Falcon 1",
            "max_payload": 180
        },
        {
            "name": "Falcon 9",
            "max_payload": 22800
        },
        {
            "name": "Falcon Heavy",
            "max_payload": 63800
        }
    ]
}
```

... and you want to use Julia to convert it to a CSV like this:

| id     | org    | launcher   | max_payload |
|--------|--------|------------|-------------|
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
    \"launchers\": [
        {
            \"name\": \"Falcon 1\",
            \"max_payload\": 180
        },
        {
            \"name\": \"Falcon 9\",
            \"max_payload\": 22800
        },
        {
            \"name\": \"Falcon Heavy\",
            \"max_payload\": 63800
        }
    ]
}
```

Let's create this as variable in Julia:

```
spacex = "{
    \"id\": 1234,
    \"org\": \"SpaceX\",
    \"launchers\": [
        {
            \"name\": \"Falcon 1\",
            \"max_payload\": 180
        },
        {
            \"name\": \"Falcon 9\",
            \"max_payload\": 22800
        },
        {
            \"name\": \"Falcon Heavy\",
            \"max_payload\": 63800
        }
    ]
}"
```

All we've done here is create the variable name (`spacex =`) and then wrap all that escaped JSON inside double quotes.

If we check the type of our `spacex` variable:

`typeof(spacex)`

We get:

`String`

Now let's turn this `String` into structured, usable data.

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

Above, we're creating a new variable, `spacex_dict`, by parsing the `spacex` data with the `JSON` package.

We get:

```
Dict{String, Any} with 3 entries:
  "id"        => 1234
  "org"       => "SpaceX"
  "launchers" => Any[Dict{String, Any}("name"=>"Falcon 1", "max_payload"=>180), Dict{String, Any}("name"=>"Falcon 9", "max_payload"=>22800), Dict{String, Any}("name"=>"Falcon He…
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

So we have a dictionary (`Dict`). That's great---it's more structured than the simple `String` we had before.

## Creating a dataframe from our structured data variable

Okay, let's go ahead and turn this into a dataframe. We're going to use the DataFrames package.

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
 Row │ id     launchers                          org
     │ Int64  Any                                String
─────┼──────────────────────────────────────────────────
   1 │  1234  Dict{String, Any}("name"=>"Falco…  SpaceX
   2 │  1234  Dict{String, Any}("name"=>"Falco…  SpaceX
   3 │  1234  Dict{String, Any}("name"=>"Falco…  SpaceX
```

Again, note the output has been truncated---this is how it shows up for me in my terminal. Again, all the data is there in memory, though.

The above dataframe is _sorta_ what we want: the non-nested JSON (`id`, `org`) is appearing properly in distinct columns.

However, the nested JSON---the `name` of the launch vehicle and its `max_payload` for low earth orbit---is appearing all as one blob of a dictionary in the `launchers` column.

Our next step will be to turn these dictionaries into clean columns in our dataframe.

## Cleaning up the dataframe

Let's take a closer look at the payloads column:

`launchers_col = df[!, :launchers]`

I've written a tutorial on [Julia dataframes](../dataframes/) if you're not familiar with them, but let's walk through this code briefly:

> `launchers_col =` Here we're creating a new variable, `launchers_col` to use later.

> `df[!, :launchers]` From the dataframe named "df" (`df[]`), get all rows (`!`) from the launchers column (`:launchers`).

We get:

```
3-element Vector{Any}:
 Dict{String, Any}("name" => "Falcon 1", "max_payload" => 180)
 Dict{String, Any}("name" => "Falcon 9", "max_payload" => 22800)
 Dict{String, Any}("name" => "Falcon Heavy", "max_payload" => 63800)
```

`payloads_length = length(payloads)`

We get:

`3`
Now let's create two empty arrays to store our data:

`max_payloads = Int64[]`
`launchers = String[]`

```
for i in 1:payloads_length
    for max_payload in payloads[i]["max_payload"]
        push!(max_payloads, max_payload)
    end
    for launcher in [payloads[i]["launcher"]]
        push!(launchers, launcher)
    end
end
```

Let's take a look at our new `max_payloads` array:

```
3-element Vector{Int64}:
   180
 22800
 63800
 ```

 And our new `launchers` array:

 ```
 3-element Vector{String}:
 "Falcon 1"
 "Falcon 9"
 "Falcon Heavy"
 ```

 Now we can add them to our dataframe:

 `df[!, :launchers] = launchers`

 `df[!, :max_payloads] = max_payloads`

 Let's take a look so far

 `df`

 We get:

 ```
 3×5 DataFrame
 Row │ id      org     payloads                           launchers     max_payloads
     │ Int64   String  Any                                String        Int64
─────┼───────────────────────────────────────────────────────────────────────────────
   1 │ 1234  SpaceX  Dict{String, Any}("max_payload"=…  Falcon 1               180
   2 │ 1234  SpaceX  Dict{String, Any}("max_payload"=…  Falcon 9             22800
   3 │ 1234  SpaceX  Dict{String, Any}("max_payload"=…  Falcon Heavy         63800
```

(Remember you can scroll horizontally on this site inside the code boxes to see the full output.)

There's just one thing left to do---we no longer need the unstructured `payloads` column. We delete it like this:

`select!(df, Not(:payloads))`

We get our finished, fully-structured dataframe:

```
3×4 DataFrame
 Row │ id      org     launchers     max_payloads
     │ Int64   String  String        Int64
─────┼────────────────────────────────────────────
   1 │ 1234  SpaceX  Falcon 1               180
   2 │ 1234  SpaceX  Falcon 9             22800
   3 │ 1234  SpaceX  Falcon Heavy         63800
```
