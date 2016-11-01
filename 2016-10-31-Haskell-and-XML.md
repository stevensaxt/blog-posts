---
layout: page
subheadline: "Gwen Lofman"
title: "Haskell and XML"
teaser: "A Technical Reflection"
date: 2016-10-31
categories:
  - technical reflection
author: Gwen Lofman
tags:
  - Haskell
  - XML
header: no
image:
  header: https://www.haskell.org/static/img/haskell-logo.svg?etag=ukf3Fg7-
  thumb: https://www.haskell.org/static/img/haskell-logo.svg?etag=ukf3Fg7-
  homepage: https://www.haskell.org/static/img/haskell-logo.svg?etag=ukf3Fg7-
  caption: Date of issue?
  link: http://link-to-page-containing-text
---
# Haskell & XML

I got pretty bored performing the same search-and-replace actions for a variety of different place names.  As any programmer knows, performing the same actions over and over again signal perfect opportunities to use code to script out those actions.

I spent time this weekend investigating the possiblity of using Haskell to write a script to automatically TEI index certain words with tags and reference data.

The program (which would be a command-line utility at first) needs to perform a few basic functions:

1. Add tags to text inside any xml nodes
2. Update attributes of tags whith specified contents
3. Take in input file that describes a set of commands

This would potentially allow me to update every xml file in the entire repository simultaneously, inserting the proper tags for a list of names, dates, and locations.  Something like:

```yaml
placename:
  - (Tokyo, ref, some-link-to-wikidata)
  - (London, ref, some-link-to-wikidata)
  - (St. Petersburg, ref, some-link-to-wikidata)
  - (Port Said, ref, some-link-to-wikidata)
persname:
  - (Lord Cromer, ref, some-link-to-person-data)
```

This would allow me to just keep an up-to-date set of location names, attribute names, and links that I could then very easily update or apply to any issue in the entire year, and perhaps even set it to run every time I changed a file, so I would never have to worry about manually changing this information.

> ## A quick note on XML
> XML can be visualized as a "rose tree" made of nodes.  This basically means that every part of the XML document is some sort of node, which has other nodes inside of it.  The nodes can be any type of tag, or any plain text, and any number of both in any order.
> Here's what some XML in your issue might look like as a Tree
```
div -*- dateline -*- date - Text
     |            *- Text
     |            *- placename - Text
     |
     *- p         - Text
     *- byline    - Text
```
> And here it is as XML
```XML
<div>
    <dateline><date>2nd October 1905</date>, <placename>Port Said</placename></dateline>
    <p>Here is some text, detailing an amazing story about something.</p>
    <byline>(Reuter.)</byline>
</div>
```

## Functional Programming

![Haskell Logo](https://www.haskell.org/static/img/haskell-logo.svg?etag=ukf3Fg7-)

I program in Haskell mostly.  Haskell is a pure functional language, which just means it's not the same as the traditional programming languages you see day-to-day, and it's not something FSU teaches currently.

It looks something like this:

```Haskell

and :: Bool -> Bool -> Bool
and c t = if c then t else False

filter :: (a -> Bool) -> [a] -> [a]
filter p xs = [x | x <- xs, p x]

map :: (a -> b) -> [a] -> [b]
map _ []   = []
map f x:xs = f x : map xs

```

It's pretty rad, it looks quite unlike most langauges I've seen in my day.  Haskell has some really interesting properties, though.  It's really easy to parse documents as a result, but it's difficult to transform them easily.  Many libraries exist to extract data easily and reliably from xml documents, but few to transform or modify them in a programatic way.

I did find one eventually: the [Haskell XML Toolbox](https://wiki.haskell.org/HXT).  It lets you do some pretty interesting things.  It uses something called arrows (a _very_ trippy functional programming concept), to allow the programmer to compose filters together to navigate the XML tree.

The [documentation](https://wiki.haskell.org/HXT#The_concept_of_filters) has this example:

```Haskell

isA  :: (a -> Bool) -> (a -> [a])
isA p x | p x       = [x]
        | otherwise = []

```

It takes a function (called a predicate) that returns a True/False value, and creates a new function that can process, or filter, values.  Essentially, these arrows let us process parts of a tree into no results, one result, or many results.  By using different types of filters, and by putting these filters together, with some relatively simple structure we can create some _extremely_ complex instructions to parse and transform the document.


Again, an example from the documentation:

```Haskell
getGrandChildren :: XmlFilter
getGrandChildren = getChildren >>> getChildren
```

The `(>>>)` operator makes it extremely trivial to chain filters together to make new fiters.  The ability to compose functions like this is the hallmark of functional programming, and works extremely weel in this case.


By defining a few more combination operatos, the HXT library quickly builds up sufficient capability to tackle some really complicated parsing, as in:

```Haskell

getTextChildren2 :: XmlFilter
getTextChildren2 = getChildren >>> ( isXText <+> ( getChildren >>> isXText ) )

```

which only gets the text children for of the current tag, and the text of any tags inside the current one.

Using this extremely flexible library, I hope to find a practical way to automate much of the task of TEI indexing the Egyptian Gazette!
