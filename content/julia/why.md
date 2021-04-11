---
title: Why Julia? It's fast and easy
date: 2021-03-31 11:00:00+11:00
seoTitle: Why Julia?
description: If you're going to learn a coding language for data science, Julia is the one. Here's why.
authors: ["Ron Erdos"]
tableOfContents: false
---

## We don't have to pick between fast and easy anymore: Julia is both

Here's a [cracking a quote from Viral Shah, a co-creator of Julia, in Quartz magazine](https://qz.com/1360318/is-julia-a-good-alternative-to-r-and-python-for-programmers/):

>“If you are a mathematician, scientist, or engineer, you have historically had the choice to pick a language that was fast, like C++ or Java, or a language was easy to learn, like Matlab, R, or Python,” says Viral Shah, one of the creators of Julia. “In Julia, we created a language that was simultaneously fast and easy.”

## Why is Julia faster than Python or R?

Another great quote from the same Quartz article:

> Julia is faster than Python and R because it is specifically designed to quickly implement the basic mathematics that underlies most data science, like matrix expressions and linear algebra.

## You don't have to avoid loops, or write C functions in R or Python any more

[Daniel Moura writes at Data Science Central](https://www.datasciencecentral.com/profiles/blogs/6448529:BlogPost:871831):

> While I was happy coding in R, it involved having a set of strategies for avoiding loops and recursion and many times the effort was being directed to “how do I avoid the pitfalls of an interpreted language?”
>
>I got to a point where I was coding C functions to tackle bottlenecks on my R scripts and, while performance clearly improved, the advantages of using R were getting lost in the way. That was when I started looking for alternatives and I found Julia.

More from Moura:

> Julia stands out by delivering C-like performance out of the box.

## "His jaw dropped"

Steven G. Johnson, an MIT professor of applied mathematics and physics, [writes](https://discourse.julialang.org/t/julia-vs-r-vs-python/4997/5):

> I remember showing a colleague’s group this [benchmark graph](https://julialang.org/benchmarks/), and after a few seconds his jaw dropped and he said … “Wait, that’s a log scale?!?”

Johnson continues:

> I often find that scientists’ mental model of a CPU is a box that does floating-point arithmetic at a fixed rate, and so they don’t understand how code could run much faster without reducing the number of floating-point operations.

One more quote from Johnson:

> [T]he semantics of [Python and Matlab] make them intrinsically hard to compile to fast code, and even harder to compile _reliably_ ... so it will be hard for them to ever fully catch up to Julia.

## 100x speed without vectorization or outsourcing to C extensions

While we're quoting MIT academics, here's [Professor Mark Vogelsberger, a theoretical astrophysicist at MIT](https://juliacomputing.com/):

> Julia code is ... more than 100 times faster than the equivalent Python code. Multiple dispatch with function calls gives Julia extremely efficient code that is practically superior to any high-level language. Faster code in Julia can be achieved without any tricks like vectorization or outsourcing to C extensions. By contrast, such tricks are often necessary to speed up Python or R code.

## The machine learning climate model that wouldn't exist without Julia

MIT oceanographer Dr. Ferrari (yes, that's his real name, and yes I am jealous of his name, even though my maths teachers used to ask me if I was related to Paul Erdos) on [building a "disruptive" machine learning climate model backed by former Google CEO Eric Schmidt](https://www.csmonitor.com/Environment/2021/0122/Meet-the-team-shaking-up-climate-models):

> There was no way we could have done it with another language ... after three or four months, we realized there was no way we could go back.

## First-time Julia user clocks 30x speed increases over heavily-optimised Python

A new Julia user [reports](https://discourse.julialang.org/t/julias-applicable-context-is-getting-narrower-over-time/55042/5):

> In a few days of work, using Julia for the first time, I was able to produce a prototype that was 14 times faster than [our] heavily optimized Python code.

He continues:

> A few weeks later that code is in production, and has seen another ~2x speedup for a total of ~30x. As far as I can tell, the improvements I made wouldn’t be possible to replicate in Python right now.
>
> The fact that we spent 200+ expert hours on the Python version vs. 10 hours of a beginner’s time on the Julia also tells you a lot about the developer experience.


## Python slices drive me nuts

Look at this Python code:

```
nums = [1,2,3,4,5]
subset_nums = nums[2:4]
subset_nums
[3,4]
```

Who has the headspace to remember that a Python slice of `2:4` starts at the _third_ item (okay, that makes sense if you _must_ use a language with zero-based indexing), but continues until the _fourth_ item? (I thought we were doing the whole `n - 1` thing when counting in Python?)

By contrast, look at the mental ease of doing this in Julia:

```
nums = [1,2,3,4,5]
subset_nums = nums[2:4]
3-element Vector{Int64}:
 2
 3
 4
```

In sum:

Julia: One is one, two is two, and so on. Simple, right? A kindergarten kid could get it.

Python: Zero is one, two is three, but four is actually four if it's at the end of a slice. Oh. My. God. Sounds more complicated than the rules of rugby union.

## Who uses Julia?

From the [official Julia website](https://julialang.org/):

> Julia has been downloaded over 25 million times and the Julia community has registered over 5,000 Julia packages for community use.

Now, some of those 25 million downloads will be people kicking the tyres and then never using it again. Fair enough.

However, Julia is used at major league companies and organisations, including:

- Apple
- Amazon
- Facebook
- Google (Alphabet)
- Microsoft
- IBM
- Uber
- KPMG
- NASA
- The Brazilian space program
- The New York Federal Reserve
- Disney
- Ford
- Blackrock, the world's largest asset manager
- Tencent

Pretty impressive list, right?
