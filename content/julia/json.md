---
title: How to convert nested JSON to a dataframe or CSV in Julia
date: 2021-06-25 16:44:59+11:00
seoTitle:
description:
authors: ["Ron Erdos"]
tableOfContents: false
draft: true
---

Let's say you have the following nested JSON:

```
{
    "id": 123456,
    "org": "SpaceX",
    "payloads": [
        {
            "name": "Falcon 1",
            "max_payload_low_earth_orbit": 180
        },
        {
            "name": "Falcon 9",
            "max_payload_low_earth_orbit": 22800
        },
        {
            "name": "Falcon Heavy",
            "max_payload_low_earth_orbit": 63800
        }
    ]
}
```

... and you want to convert it to a CSV like this:

`[insert CSV format]`

First, we need to escape the double quotes with a backslash, so that we can create this JSON as a string in Julia. I did this with a simple find and replace in VS Code, yielding this:

```
{
    \"id\": 123456,
    \"org\": \"SpaceX\",
    \"payloads\": [
        {
            \"name\": \"Falcon 1\",
            \"max_payload_low_earth_orbit\": 180
        },
        {
            \"name\": \"Falcon 9\",
            \"max_payload_low_earth_orbit\": 22800
        },
        {
            \"name\": \"Falcon Heavy\",
            \"max_payload_low_earth_orbit\": 63800
        }
    ]
}
```

Let's create this as variable in Julia:

```
spacex_payloads = "{
    \"id\": 123456,
    \"org\": \"SpaceX\",
    \"payloads\": [
        {
            \"name\": \"Falcon 1\",
            \"max_payload_low_earth_orbit\": 180
        },
        {
            \"name\": \"Falcon 9\",
            \"max_payload_low_earth_orbit\": 22800
        },
        {
            \"name\": \"Falcon Heavy\",
            \"max_payload_low_earth_orbit\": 63800
        }
    ]
}
"
```

All we've done here is create the variable name (`spacex_payloads =`) and then wrap all that escaped JSON inside double quotes---we're initially creating a string, but we'll turn it into structured data later.

Now we need to add the relevant packages. To do this, at the julia prompt, input a closing square bracket, like this:

`]`

This will turn the green julia prompt into a blue <code class="julia-pkg">pkg></code> prompt:

<code class="julia-pkg">pkg></code>

Now we can easily add the necessary packages:

`add JSON`

Once the packages have been installed, we are shown the <code class="julia-pkg">pkg></code> prompt again:

<code class="julia-pkg">pkg></code>

Hit the delete key to return to the regular prompt:

`julia>`

Now we can get to work.

First, let's check the type of our `spacex_payloads` variable we created earlier:

`typeof(spacex_payloads)`

We get:

`String`

So it's all just one big glob at the moment to Julia; it's not structured at all. Let's fix that with the JSON package we installed earlier. First, we have to tell Julia we want to _use_ the package:

`using JSON`

Now we can go ahead and actually use it:

`spacex_payloads_dict = JSON.parse(spacex_payloads)`

We get:

```
Dict{String, Any} with 3 entries:
  "id"       => 123456
  "org"      => "SpaceX"
  "payloads" => Any[Dict{String, Any}("name"=>"Falcon 1", "max_payload_low_earth_orbit"=>180), Dict{String, Any}("name"=>"Falcon 9", "max_payload_low_earth_orbit"=>22800), Dict{…
```

Note that the terminal output above has been truncated, since that's how it showed up for me in my terminal---it may well be truncated for you too. Nevertheless, all our data is there in memory.

Let's go ahead and check the type of our new `spacex_payloads_dict` variable:

`typeof(spacex_payloads_dict)`

We get:

`Dict{String, Any}`

Okay, let's go ahead and turn this into a dataframe. We're going to use the DataFrames package. If you don't have it installed, go to the package manager by inputting `]` and then inputting `add DataFrames`. You can return to the Julia prompt by pressing the delete key once the package has been installed.

Now let's tell Julia we want to use the DataFrames package:

`using DataFrames`

Now let's create a dataframe from our data. I'm going to use the conventional `df` as our variable name, but you can choose anything you like:

`df = DataFrame(spacex_payloads_dict)`

Note that `DataFrame` is singular above, even though it's plural (`DataFrames`) when we add it in the package manager and when we tell Julia we'll be `using` it.

We get:

```
3×3 DataFrame
 Row │ id      org     payloads
     │ Int64   String  Any
─────┼───────────────────────────────────────────────────
   1 │ 123456  SpaceX  Dict{String, Any}("name"=>"Falco…
   2 │ 123456  SpaceX  Dict{String, Any}("name"=>"Falco…
   3 │ 123456  SpaceX  Dict{String, Any}("name"=>"Falco…
```

Again, note the output has been truncated---this is how it shows up for me in my terminal. Again, all the data is there in memory, though.

The above dataframe is _sorta_ what we want: the non-nested JSON (`id`, `org`) is appearing properly in distinct columns. However, the nested JSON---the name of the launch vehicle and its max payload for low earth orbit---is appearing all as one blob of a dictionary in the `payloads` column.

Our next step will be to turn this dictionary into clean columns in our dataframe.


