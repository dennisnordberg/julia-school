---
title: About
seoTitle: About
description: About this site
layout: page-with-subscribe-form
---

Hey, I'm Ron Erdos.

Part and parcel of being an SEO (search engine optimiser) at leading Australian tech companies means I need to wrangle fairly big datasets.

Sometimes I need to pivot data, but I dislike doing this in Excel as the pivot table it gives you is not very customisable. (I always end up copying the values to a new sheet, which breaks the link to the original data) [Pivoting data in Julia](../julia/dataframes/#how-to-use-pivot-tables-in-julia-dataframes) is more extensible, and just more fun.

Sometimes I need data that doesn't exist in our analytics suites. To get this data, I'll scrape my employer's websites (with permission). Again, I use [Julia for web scraping](../julia/scraping/) as I find it more elegant than Python (my former scraping tool of choice).

I also love the fact that Julia is 1-indexed (it starts counting from 1, not 0), you know, like humans do.

Not only that, in Julia, slices don't require me to rack my brains each time for the syntax.

Look at this unintuitive slicing in Python:

```
# PYTHON
nums = [1,2,3,4,5]
nums[2:3]
[3]
```

Now look at the same example in Julia:

```
# JULIA
nums = [1,2,3,4,5]
nums[2:3]
2-element Array{Int64,1}:
 2
 3
```

Beautiful. I mean, that makes so much more sense, right?

If you want to read more on this, I wrote a piece on [why Julia is a great programming language for data analysis](../julia/why/).

I created this website because I'd love to see the Julia ecosystem grow even further ([it's already being used at top companies you've heard of](../julia/why/#who-uses-julia)).

Good luck, and please feel free to reach out with any questions, tutorial requests, or just to say hi. I'm on [Twitter](https://twitter.com/RonErdos), [LinkedIn](https://www.linkedin.com/in/ronerdos/) and on email: [my firstname] at [my lastname] dot com dot au.

Cheers,

Ron Erdos
