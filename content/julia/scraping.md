---
title: Scraping web pages with Julia and the HTTP and Gumbo packages
date: 2022-05-17 12:35:32+11:00
seoTitle: "Scraping web pages with Julia HTTP & Gumbo: Tutorial"
description: Julia can be used for fast web scraping, not just data analysis.
authors: ["Ron Erdos"]
tableOfContents: true
version: 1.7.2
---

Do you want to crawl and scrape web pages with the Julia language? This tutorial will show you how.

## Ethical and legal web scraping

Before we begin, it goes without saying that you should only scrape web pages where you are not infringing any laws or rules by doing so. See the box below for a few websites you can scrape legally and ethically.

<aside>

### Which websites can I legally practise web scraping on?

You can practise web scraping legally on `example.com` (or any of its clones, `example.net`, `example.org` and `example.edu`). These simple websites are published by the Internet Corporation for Assigned Names and Numbers (ICANN) especially for documentation and testing purposes.

If you want **more complex websites to scrape legally**, try `toscrape.com`, which provides a list of quotes at `quotes.toscrape.com` and a dummy bookstore at `books.toscrape.com`. These websites are published by Scraping Hub for scraping practice.

</aside>

## Introducing three Julia web scraping packages: HTTP.jl, Gumbo.jl and AbstractTrees.jl

We're going to use three packages. Here's what they each do:

### `HTTP.jl`

Scrapes web pages

### `Gumbo.jl`

Parses these web pages after we've scraped them with `HTTP.jl`, so that we can more easily retrieve specific HTML elements (such as an `<h1>` heading).

### `AbstractTrees.jl`

Allows us to extract specific HTML elements (again, such as an `<h1>` heading) by name, rather than position.

Without `AbstractTrees.jl`, as far as I know, you'll only be able to receive specific HTML elements by their position in the source code.

For example, you'd need to know that the `<h1>` heading is, say, the 17th element in the source code. Of course, if you are scraping multiple websites, or even multiple page types from the same websites, this won't always be true, and your scrape won't work.

<aside>

There is another Julia package named `Cascadia.jl` which allows you to scrape HTML elements by CSS class or id, but that's out of scope for this article (for now).

</aside>

### Downloading and installing the packages

First, if you don't have Julia itself installed, here's [how to install Julia on a Mac](../setup/).)

OK, once you have Julia installed, fire up your terminal of choice, and enter the Julia REPL by typing `julia` at your command prompt.

You should see the green Julia command prompt:

`julia>`

Now it's time to add the packages.

My favourite way to do this is to type the `]` (right square bracket, located above the Return / Enter key on your keyboard) into the Julia terminal.

This changes the green prompt we saw above into a purple one that says:

<code class="julia-pkg">pkg></code>

The three letters above stand for "package". We're now in Julia's package manager.

We can now easily add the three web scraping and parsing packages we need, using the `add` command:

`add HTTP Gumbo AbstractTrees`

Note that we separate the package names with spaces, rather than commas.

Hit Enter and let Julia do its thing.

Once the packages have been installed, you can exit out of Julia's package manager by pressing Delete / Backspace. You'll then see the green prompt again:

`julia>`

### "Using" the packages

Before we do any actual web scraping, we need to tell Julia we intend to use all three of our newly installed packages:

`using HTTP, Gumbo, AbstractTrees`

Unlike when we `add`ed the packages, we need to use commas to separate the package names in a `using` command.

## Scraping example.com

Now that we're all set up, let's get to work scraping the homepage of `example.com`.

We'll store it in a new variable we'll create, and we'll call this variable `r` (for "request", as in "HTTP request").

We'll just use the `HTTP` package for now---we'll use the others later.

We enter our command into the Julia REPL:

`r = HTTP.get("https://example.com/")`

<aside>

### We need to use the full protocol in urls

Note that we need to include the full protocol (e.g. `https://`) at the start of our target url, as shown just above this box. If we just do this:

`r = HTTP.get("example.com")`

... we get this error:

`ERROR: DNSError: , unknown node or service (EAI_NONAME)`

</aside>

### The output

Here's the output. We'll walk through it below.

```
HTTP.Messages.Response:
"""
HTTP/1.1 200 OK
Age: 433583
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Sun, 02 Aug 2020 02:37:09 GMT
Etag: "3147526947+ident"
Expires: Sun, 09 Aug 2020 02:37:09 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (sjc/4E8D)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256

<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;

    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
        border-radius: 0.5em;
        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        div {
            margin: 0 auto;
            width: auto;
        }
    }
    </style>
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domai
⋮
1256-byte body
"""
```

### Output walkthrough

Julia truncates the output, but we can see, in descending order:

1) the **protocol** and **status code** (`HTTP/1.1 200 OK`). `HTTP/1.1` is the protocol and `200` is the status code. A code of `200` means the page has loaded correctly, which is why we also see the word `OK`.
2) the **headers**, which in this case consists of eleven key-value pairs from `Age: 433583` through to `Content-Length: 1256`
3) the first 1000 or so characters of the **source code** of the actual HTML document. In this case, we see `<!doctype html>` through to just after `<h1>Example Domain</h1>`. Note that the full web page will be stored in our variable `r`; it's just the output in the Julia REPL that's truncated.

Later on in this tutorial, we'll explore techniques for laser-targeting _just_ the status code or _just_ the headers (see the table of contents at the top for links to these), but for now, let's continue exploring how we can target individual HTML elements in the source code. First though, the next three sections show you how to set cookies and headers when web scraping with Julia and the HTTP.jl package. If you don't need to set cookies or headers, feel free to skip past these.

## How to set cookies when web scraping with Julia and the HTTP.jl package

This week, I needed to scrape our company's staging server. I knew that I would need my scraper set a cookie to make our staged web application behave properly for the scrape.

To set a cookie, I started with the following (vastly simplified) code:

```
using HTTP, Gumbo

url = https://staging.example.com 

r = HTTP.request("GET", url; cookies=Dict("foo"=>123))
```

_Notice that a semicolon separates the first two parameters from the_ `cookies` _dictionary---it'd be easy to misread this as a comma._

This differs from the code earlier in this tutorial in two ways:

1. I used the more verbose `HTTP.request()` (with a `"GET"` argument) rather than `HTTP.get()`. This is because `HTTP.request()` allows you to set cookies, and, as far as I can tell, `HTTP.get()` doesn't.
2. I set a cookie. In the code above, my cookie's name is `foo` and its value is `123`.

Below are some additional pointers on setting cookies when scraping websites with Julia.

### How to set the value of a cookie to `true` with HTTP.jl

If you need to set the value of a cookie to `true`, set it to `"true"` (with the double quotes). For example:

`r = HTTP.request("GET", url; cookies=Dict("foo"=>"true"))`

(Don't worry, it will come through correctly as `true`.)

Otherwise you'll get an error like so:

`LoadError: TypeError: in keyword argument cookies, expected Union{Bool, Dict{<:AbstractString, <:AbstractString}}, got a value of type Dict{String, Bool}`

### How to set multiple cookies with HTTP.jl

Note that if I'd needed to set multiple cookies, I could have done so like this:

`r = HTTP.request("GET", url; cookies=Dict("foo"=>123, "bar"=>"abc"))`

### You don't need to set a cookie expiry with HTTP.jl

I didn't bother setting a cookie expiry since my script sets the cookie each time the code scrapes a url.

So that wraps up the "how to" on setting cookies with Julia web scraping. Before we get back to the tutorial proper, a quick tip on setting headers.

## How to set headers when web scraping with Julia and the HTTP.jl package

Today I need to scrape a different staging server at work. This didn't require cookies to be set, but it did require a custom header. In this case, I needed to set an `x-country` header. 

Here's the relevant line of code which does that in Julia using the HTTP.jl package:

`r = HTTP.request("GET", url; headers=Dict("x-country" => "$country"))`

_For the rest of the code, scroll up to the earlier part of the tutorial where we talk about scraping_ `example.com`.

## How to set both cookies and headers when scraping with Julia and the HTTP.jl package

If you need to set both cookies _and_ headers when scraping with Julia's HTTP.jl package, you can do so like this:

`r = HTTP.request("GET", url; headers=Dict("x-country" => "$country"), cookies=Dict("ALLOW_ZONE_OVERRIDE"=>"true"))`

_For the rest of the code, scroll up to the earlier part of the tutorial where we talk about scraping_ `example.com`.

And now, let's continue our tutorial on scraping `example.com`.

## Enter Gumbo.jl

`Gumbo.jl` is a Julia package that enables us to transform the relatively amorphous blob of HTML we crawled with `HTTP.jl` into a parseable HTML tree. This means we can fairly easily zero in on particular HTML elements, such as the `<h1>` heading. We'll do just that below.

<aside>

### "Using" Gumbo.jl

Earlier in this tutorial, we told Julia that we want to be `using Gumbo` (as part of a multi-package `using HTTP, Gumbo` command), so we don't need to do that again here.

If you skipped entering that command into your Julia REPL earlier, you should tell Julia that you want to be `using Gumbo`.

</aside>

### Turning our blob of HTML into a parseable HTML tree with Gumbo

Earlier, we stored our crawl of the homepage of `example.com` into a variable we called `r` (which stands for "request", as in "HTTP request").

Now we'll make a "Gumbo" version---which will be easily traversable---of that `r` variable, and we'll call it `r_parsed`.

The way to do that is below:

`r_parsed = parsehtml(String(r.body))`

### The output from the Gumbo command

```
HTML Document:
<!DOCTYPE html>
HTMLElement{:HTML}:<HTML>
  <head>
    <title>
      Example Domain
    </title>
    <meta charset="utf-8"/>
    <meta content="text/html; charset=utf-8" http-equiv="Content-type"/>
    <meta content="width=device-width, initial-scale=1" name="viewport"/>
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;

    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
...
```

This output is also truncated, but we're now ready to start digging down into our parsed HTML tree. We'll do that right now.

### Digging down into Gumbo's parsed HTML tree

To dig down into Gumbo's parsed HTML tree, we'll need to append `.root` to `r_parsed` to start working with it. So we'll be using `r_parsed.root`.

Now, let's say we want to see _just_ the `<head>` section of this source code, the source code of `example.com`.

Well, there are two main sections in the source code of any web page: the `<head>` (available at `r_parsed.root[1]`), and the `<body>` (which can be found at `r_parsed.root[2]`). (Remember that Julia uses 1-based indexing.)

Since we want the `<head>` section to start with, we'll use this command:

`head = r_parsed.root[1]`

We get:

```
HTMLElement{:head}:<head>
  <title>
    Example Domain
  </title>
  <meta charset="utf-8"/>
  <meta content="text/html; charset=utf-8" http-equiv="Content-type"/>
  <meta content="width=device-width, initial-scale=1" name="viewport"/>
  <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;

    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
...
```

Now, if we want _just_ the `<body>` section, we'll use _this_ command:

`body = r_parsed.root[2]`

... and we see this:

```
HTMLElement{:body}:<body>
  <div>
    <h1>
      Example Domain
    </h1>
    <p>
      This domain is for use in illustrative examples in documents. You may use this
      domain in literature without prior coordination or asking for permission.
    </p>
    <p>
      <a href="https://www.iana.org/domains/example">
        More information...
      </a>
    </p>
  </div>
</body>
```

### Getting the text of the h1 heading

OK, so how do we get the text of the `<h1>` heading?

Well, immediately inside the `<body>`, we have a `<div>`, and then immediately inside that, we have our `<h1>`. So that's the first element inside the first element inside the `<body>`.

So we'll do this:

`h1 = body[1][1]`

... and we get this:

```
HTMLElement{:h1}:<h1>
  Example Domain
</h1>
```

OK, so how do we get just the text of this `<h1>`?

Well, if we enter this command:

`h1[1].text`

... we get this:

`"Example Domain"`

Booyah!

## Scraping a given HTML element by name in Julia using the AbstractTrees package

Okay, so the code in the previous section is all well and good if you're scraping a particular page type---such as your company's blog posts---where the template is fixed and thus the order of the HTML elements doesn't change.

But what if you want to scrape multiple websites, or even multiple page types on the same website? When the `<h1>` heading is, say, element number 17 on one page, and, say, element number 23 on another page, using its position isn't scalable.

Instead, let's scrape our desired HTML element(s) by name.

And let's scrape the page `<title>` this time, instead of the `<h1>` element.

Below is the complete code to scrape the page `<title>` element from example.com.

```
using HTTP, Gumbo, AbstractTrees

r = HTTP.get("https://example.com/")
r_parsed = parsehtml(String(r.body))
root = r_parsed.root

for elem in PreOrderDFS(root)
    try
        if tag(elem) == :title
            println(AbstractTrees.children(elem)[1])
        end
    catch
        # Nothing needed here
    end
end
```

Running that code, we get:

`Example Domain`

... which, if you look at `example.com`, is the text in the page `<title>`. Boom!

Let's walk through that code so we have it straight:

> `using HTTP, Gumbo, AbstractTrees` We're using the two packages (`HTTP` and `Gumbo`) we covered earlier in this tutorial. We're _also_ using a package we haven't yet used in this tutorial: `AbstractTrees`. It's this package that will let us scrape by the name of the HTML element, rather than its position.

> `r = HTTP.get("https://example.com/")` We used this exact line of code earlier in the tutorial. It uses the `HTTP` package to scrape `https://example.com/`, but we'll need to do more to get it into a usable form. Let's keep going ...

> `r_parsed = parsehtml(String(r.body))` We also used this exact line earlier in the tutorial. Here we're using the `parsehtml()` function from the `Gumbo` package to make our scraped HTML more usable.

> `root = r_parsed.root` We haven't used this line before in this tutorial. Here we're creating a variable, `root`, which contains, both the `<head>` and `<body>` sections from `example.com`.

> `for elem in PreOrderDFS(root)` Here we start a "for" loop. We'll be iterating over each element (`elem`) in our `root` variable. We've wrapped root inside a function from the `AbstractTrees` package named `PreOrderDFS()`, which appears to be necessary to allow us to extract HTML elements by name.

> `try` Here we start a "try / catch" block. We need this because different HTML elements have different structures, and our code only handles one type. If we didn't have the "try / catch" block, our code would eventually throw an exception and fail.

> `if tag(elem) == :title` Here we're telling our code we only want the page `<title>` element. Both the `tag()` function and the `:title` symbol [are from Gumbo and/or AbstractTrees](https://github.com/JuliaWeb/Gumbo.jl).

> `println(AbstractTrees.children(elem)[1])` With the `println()` function, we are telling Julia to print to the terminal the actual text in the page `<title>` of the `example.com` homepage. Of course, you don't have to print the page `<title>` to the terminal, you can write it to a dataframe, a CSV or a text file.

> The rest of the code just closes all the loops; a necessary task.

## Getting just the status code of web pages

Let's do this with an example.

By the way, if you're not sure what a status code is, read the box below.

<aside>

### What is a web page status code?

When you request a web page in your browser, the server will send your browser a three-digit code to tell it how that request went. For example, a code of `200` means the page loaded as requested. You beauty!

A code of `404` means the page couldn't be found.

And a code of `301` means the page has been permanently redirected to another page.

There are many other status codes; these are just three examples.

</aside>

<aside>

### "Using" the HTTP package

If you haven't already done so, you need to tell Julia that you'll be `using` the `HTTP` package (you won't need the `Gumbo` package to get the status codes).

To do so, at the green `julia` prompt, enter the following command:

`using HTTP`

Julia will take a second or two to action this.

</aside>

### Getting the status code of a working page (status code 200)

Let's say we want to get the status code of `https://example.com/`. This is a working page, which therefore has the status code of `200`.

To do this, we'll have Julia visit the url above and assign it to the variable `example_com`:

`example_com = HTTP.head("https://example.com/")`

<aside>

### HTTP.get() vs HTTP.head() in the Julia HTTP.jl package

Note that we aren't using `HTTP.get()` to pull the status code, although we could. We're using `HTTP.head()` because it only pulls the headers (which include the status code). We're saving bandwidth (ours and that of the web server) by not pulling the whole web page when we only need the headers.

</aside>

Now we can trivially get the status code:

`example_com.status`

You should see the following output:

`200`

As explained in the box above, `200` is the status code that indicates the page has loaded correctly.

### Getting the status code of a 404 page

If we want to get the status code of a 404 page, then we need to add a bit more to our code.

Let's say we want the status code of `https://example.com/404`, which has the status code of `404`, which means the page could not be found.

If we just try this:

`example_com_404 = HTTP.head("https://example.com/404")`

... then we get this error:

`ERROR: HTTP.ExceptionRequest.StatusError`

The way to avoid this error is by adding the argument `status_exception=false` to our command:

`example_com_404 = HTTP.head("https://example.com/404", status_exception=false)`

Now we can get the status code:

`example_com_404.status`

... and we see in the first few lines of our terminal output, that the status code is "404 Not Found":

```
HTTP.Messages.Response:
"""
HTTP/1.1 404 Not Found
```

### Getting the status code of a 500 page

When pages cannot be loaded due to a server error, there's a good chance it has the status code of `500`.

For example, this page has a (deliberate) `500` error:

`https://getstatuscode.com/500`

Similarly to the `404` example above, we need to use the `status_exception=false` argument in our code:

`get_status_code_500 = HTTP.head("https://getstatuscode.com/500", status_exception=false)`

... so that we can ask for the status code:

`get_status_code_500.status`

... and get the right answer in the first few lines of our terminal output:

```
HTTP.Messages.Response:
"""
HTTP/1.1 500 Internal Server Error
```

By contrast, if we leave out the `status_exception=false` argument:

`get_status_code_500 = HTTP.head("https://getstatuscode.com/500")`

... then we get the same sort of error we saw earlier in our 404 example:

`ERROR: HTTP.ExceptionRequest.StatusError`

### Getting the status code of a 301 redirect

If you want to know which pages on a website have been 301 redirected, we'll need to use `HTTP.get()` rather than `HTTP.head()`. The latter doesn't work with 301 (or 302) redirects.

For example, let's say you want the status code for `http://balloon.com`, which 301 redirects to `https://LABalloons.com`, an LA-based balloon supply company. 

<aside>

In this example, we'll be using the non-secure (`http`) version of balloon.com, as the secure (`https`) version does not resolve.

</aside>

If we simply run:

`balloon_com = HTTP.head("http://balloon.com")`

... we get an error, which reads, in part:

```
ERROR: HTTP.ExceptionRequest.StatusError(405, "HEAD", "/", HTTP.Messages.Response:
"""
HTTP/1.1 405 Method Not Allowed
```

We get the same 405 status error even if we run the command with an additional argument preventing redirects:

`balloon_com = HTTP.head("http://balloon.com", redirect=false)`

Instead, we need to use a full `HTTP.get()` request instead of `HTTP.head()`.

We'll also need to disallow redirects with that `redirect=false` argument:

`balloon_com = HTTP.get("http://balloon.com", redirect=false)`

We get, in part:

```
HTTP.Messages.Response:
"""
HTTP/1.1 301 Moved Permanently
Date: Fri, 12 Nov 2021 12:20:46 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 57
Connection: keep-alive
Location: https://laballoons.com
```

We can now see that our original request for `http://balloon.com` was 301 redirected to `https://laballoons.com` (the new URL is in the last line of the truncated output above).

<aside>

If we had left out the `redirect=false` argument, we would have been silently redirected to `LAballoons.com`.

Let's play this out. Here's the command without the `redirect=false` argument:

`balloon_com = HTTP.get("http://balloon.com")`

And here's the first part of the terminal output:

```
HTTP.Messages.Response:
"""
HTTP/1.1 200 OK
```

We wouldn't know about the 301 redirect that happened.

That's why, when running a multi-url web scrape, I always include the `redirect=false` argument. I can then have my script get the destination urls (e.g. `LAballoons.com`) in the event of a 301 redirect.

`HTTP.jl` has changed since I last did a big web scrape, but it appears I'll have to use the less-efficient `HTTP.get` rather than `HTTP.head` in order to cater for 301 and 302 redirects.

</aside>

### Getting the status code of a 302 redirect

The process is very similar for a 302 redirect (a temporary redirect).

Let's take a look at an example.

Did you know that Jeff Bezos owns `relentless.com` and 302 redirects it to Amazon?

So if we have Julia crawl it:

`relentless_com = HTTP.get("http://relentless.com/", redirect=false)`

We get, in part:

```
HTTP.Messages.Response:
"""
HTTP/1.1 302 Moved Temporarily
Date: Fri, 12 Nov 2021 12:32:33 GMT
Server: Server
Location: http://www.amazon.com
```

We can see that a 302 redirect (a temporary redirect) has occurred, and we have ended up on `http://www.amazon.com`. As an aside, of course Amazon is going to then redirect that non-secure Amazon.com url to the secure `https://www.amazon.com`. We can have our script follow these rabbit trails to make sure we are getting the final url---in this case, the secure `https://www.amazon.com`.

## Crawling just the headers

If you're not sure what headers are, check out the box below.

<aside>

### What are HTTP headers?

HTTP headers are a form of "meta information" sent between the server and the browser (and vice versa). Examples of HTTP headers sent by a server to a browser are the date the web page was last modified, and the length of time for which the page may be cached. By contrast, headers sent the other way---from browser to server---may include cookies or the referring page.

</aside>

### Worked example

Let's fetch the headers from `example.com`. As mentioned earlier, because we aren't fetching the whole page, we don't need to use `HTTP.get()` (which fetches the headers _and_ all the HTML); we can use `HTTP.head()` (which only fetches the headers) instead. We're saving bandwidth (ours and that of the web server) this way.

First, we visit the page:

`example_com = HTTP.head("https://example.com/")`

... and then we request the headers:

`headers = example_com.headers`

This is the output I got:

```
12-element Array{Pair{SubString{String},SubString{String}},1}:
  "Accept-Ranges" => "bytes"
            "Age" => "390198"
  "Cache-Control" => "max-age=604800"
   "Content-Type" => "text/html; charset=UTF-8"
           "Date" => "Sat, 01 Aug 2020 03:25:23 GMT"
           "Etag" => "\"3147526947\""
        "Expires" => "Sat, 08 Aug 2020 03:25:23 GMT"
  "Last-Modified" => "Thu, 17 Oct 2019 07:18:26 GMT"
         "Server" => "ECS (sjc/4E74)"
           "Vary" => "Accept-Encoding"
        "X-Cache" => "HIT"
 "Content-Length" => "1256"
 ```

Note that some or all of the dates in your output may be different.

### Extracting the relevant headers from the Julia array by index

You can then pull the relevant key-value pair(s) from this array as needed. For instance, if you want the `Last-Modified` header (which is the eighth element in the `headers` array we created), you could do this:

`last_modified = headers[8]`

... and you'd get this:

`"Last-Modified" => "Thu, 17 Oct 2019 07:18:26 GMT"`

Or if you want just the _value_, you could do this:

`last_modified = headers[8][2]`

... and you'd get this:

`"Thu, 17 Oct 2019 07:18:26 GMT"`

NB: The `[2]` at the end of the last command above signifies that we want only the _second_ item in the key-value pair, namely, the value, which in this case is the actual date and time the page was last modified. (Remember that Julia uses 1-based indexing, not zero-based like many other programming languages. In plain English, Julia starts counting at one, not zero---just the way I like it.) If we'd used `[1]` instead of `[2]`, we'd get just the key, which in this case is simply the words `"Last-Modified"`.

### Extracting the relevant headers from the Julia array by key name

It's probably a better idea to extract the value of a given header by its _name_ (e.g. `"Last-Modified"` rather than its position in the array.

For example, if you're scraping multiple web pages, some might have 11-element header arrays, and some might have 9, 10, 12, and so on.

Here's how to get the value of a given header (we'll work with `"Last-Modified"`) by name.

Using our same `headers` array we created above:

```
for header in headers
  if header[1] == "Last-Modified"
    println(header[2])
  end
end
```

Above, we iterate over each header in the `headers` array (`for header in headers`).

Then we say that if the header key (name) is "Last-Modified" (`if header[1] == "Last-Modified"`), then print the value of that header (`println(header[2])`). Of course, you could save it to another array or a dataframe; I've just provided you with a simple example above.
