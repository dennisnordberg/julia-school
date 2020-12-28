---
title: Why Julia? It's fast and easy
date: 2020-02-16 11:00:00+11:00
seotitle: Why Julia?
description: If you're going to learn a coding language for data science, Julia is the one. Here's why.
img: ""
authors: ["Ron Erdos"]
contentformat: article
---

## We don't have to pick between fast and easy anymore: Julia is both

Here's a [cracking a quote from Viral Shah, a co-creator of Julia, in Quartz magazine](https://qz.com/1360318/is-julia-a-good-alternative-to-r-and-python-for-programmers/):

>“If you are a mathematician, scientist, or engineer, you have historically had the choice to pick a language that was fast, like C++ or Java, or a language was easy to learn, like Matlab, R, or Python,” says Viral Shah, one of the creators of Julia. “In Julia, we created a language that was simultaneously fast and easy.”

## Why is Julia faster than Python or R?

Another great quote from the same Quartz article:

> Julia is faster than Python and R because it is specifically designed to quickly implement the basic mathematics that underlies most data science, like matrix expressions and linear algebra.

## You don't have to avoid loops, or write C functions in R or Python any more

[Daniel Moura writes at Data Science Central](https://www.datasciencecentral.com/profiles/blogs/6448529:BlogPost:871831):

> While I was happy coding in R, it involved having a set of strategies for avoiding loops and recursion and many times the effort was being directed to “how do I avoid the pitfalls of an interpreted language?”. I got to a point where I was coding C functions to tackle bottlenecks on my R scripts and, while performance clearly improved, the advantages of using R were getting lost in the way. That was when I started looking for alternatives and I found Julia.

Later on, Moura writes:

> Julia stands out by delivering C-like performance out of the box.

## "His jaw dropped"

[Steven G. Johnson, an MIT professor of applied mathematics and physics writes](https://discourse.julialang.org/t/julia-vs-r-vs-python/4997/5):

> I remember showing a colleague’s group this [benchmark graph](https://julialang.org/benchmarks/), and after a few seconds his jaw dropped and he said … “Wait, that’s a log scale?!?”

Johnson also writes, in that same thread:

> I often find that scientists’ mental model of a CPU is a box that does floating-point arithmetic at a fixed rate, and so they don’t understand how code could run much faster without reducing the number of floating-point operations.

One more quote from Johnson, same thread:

> [T]he semantics of [Python and Matlab] make them intrinsically hard to compile to fast code, and even harder to compile _reliably_ ... so it will be hard for them to ever fully catch up to Julia.

## Speed without vectorization or C extensions

While we're quoting MIT academics, here's [Professor Mark Vogelsberger, a theoretical astrophysicist at MIT](https://juliacomputing.com/):

> ... Julia code is ... more than 100 times faster than the equivalent Python code. Multiple dispatch with function calls gives Julia extremely efficient code that is practically superior to any high-level language. Faster code in Julia can be achieved without any tricks like vectorization or outsourcing to C extensions. By contrast, such tricks are often necessary to speed up Python or R code.

## Python slices drive me nuts

Look at this Python code:

```
nums = [1,2,3,4,5,6,7,8,9,10]
subset_nums = nums[2:7]
subset_nums
[3,4,5,6,7]
```

Who has the headspace to remember that a slice of `2:7` is actually the items 3 to 7? Not me.

By contrast, look at the mental ease of doing this in Julia:

```
nums = [1,2,3,4,5,6,7,8,9,10]
subset_nums = nums[3:7]
[3,4,5,6,7]
```

I grew up counting from the number one, so sue me if I can't be bothered to remember that "2" is actually "3" (sounds a bit Orwellian, just kidding).

In sum:

Julia: One is one, two is two, and so on. Simple, right? A kindergarten kid could get it.

Python: One is zero, two is one, and seven is actually seven if it's the end of the slice. Oh. My. God. Sounds more complicated than the rules of rugby union.

## Who uses Julia?

From the [official Julia website](https://julialang.org/):

> Julia has been downloaded over 13 million times and the Julia community has registered over 2,800 Julia packages for community use.

Now, some of those 13 million downloads will be people kicking the tyres and then never using it again. Fair enough.

However, a lot of those downloads are in use at major league companies, including:

- Apple
- Amazon
- Facebook
- Google (Alphabet)
- Microsoft
- IBM
- Uber
- KPMG
- NASA
- The New York Federal Reserve
- Disney
- Ford
- Blackrock, the world's largest asset manager
- Tencent
- and [wherever I work](https://www.linkedin.com/in/ronerdos/)!

Pretty impressive list, right?