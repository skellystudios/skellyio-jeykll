---
layout: post
title:  "StoryTime Lang – A Steganographic Language"
date:   2016-02-12 07:39:51 +0000
categories: technology
sidebar-image: https://images.unsplash.com/photo-1447069387593-a5de0862481e?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&s=0dab06e522cf4cd96ee75e9801e73c8a
photo-credit: "Joanna Kosinska - www.joannakosinska.com"
draft: true
---


At the weekend I attended [Banterhack 2016](http://banterhack.com/), an amazing hackathon run by my friend and esteemed colleague Manoj. Banterhack is, as far as I can tell, a reaction to the swathes of hackathons that have big name corporate sponsors who judge entries and give our appealing prizes and even more appealing chances to work for them. What that means is that people chase the "dream" of winning a 3D printed Oculus Leap Nest with the BarclaySachsWaterhouseCooper Prize for Innovation, and don't make cool shit any more. In contrast, I was one of the sponsors of Banterhack, where we gave  [these](http://www.amazon.co.uk/gp/product/B00KARD0D2) terrible China clone iWatches as top prize. My judging criteria was maximum ratio of high cool to low commercial viability.

Keeping within the spirit of Banterhack, I wanted to create something fairly cool, but with basically zero practical application. Thus, StoryTime (name pending potential rethink) was born.

---

# StoryTime

     A republican hears the democrat say nothing and the democrat hears the republican say wise things. Campaigning is where someone says promises they can't keep and the democrat hears them and they listen to the republican and the republican hears the democrat. I always campaign.

>
> Prints the first 20 fibonacci numbers

StoryTime is a language whose aim is to be easily writable by a human programmer, but also easily mistook for valid semantic English. The idea came from the concept of [steganography](https://en.wikipedia.org/wiki/Steganography) (security by obscurity, often hiding a secret inside something innocuous, not to be confused with [stenography] (https://en.wikipedia.org/wiki/Stenography), which is the art of being able to write something so quickly it's instantly unreadable).

I actually had three criteria in mind while making this:

- It should be possible to construct programs by hand
- The program should look and read like valid English
- The program should surprise and delight (shock and awe) even those who know it's a program, by being non-obvious how the program's function came from its structure.

That last rule was pretty useful in making decisions about how to structure the language, because it ruled out doing things in the way other natural language-esque esolangs do, e.g. [~English](https://esolangs.org/wiki/~English).


*Storytime* is currently an interpreted language built in Haskell with Parsec for parsing.  You can see the source on Github [here](https://github.com/skellystudios/storytime-lang)

# Syntax

### Basics

A storylang program is a series of sentences. Sentences are separated by full-stops `.` (translation for America English: periods). All 'statements' (liguistically equivalent to clauses) are expressions: every statement returns a value.


### Variables

One thing I really wanted was arbitrary nouns and verbs to be used as variables. I also wanted proper nouns, because that really makes things a lot more fun. Anything preceded with `a` or `the` is a variable. Anything with a capital letter is also treated as a variable, apart from "A", "The" and verbs at the beginning of method declarations.

When variables are instantiated with a number equal to their length, for example:

    > A dog.  
    [ returns 3 ]
    > The mouse.
    [ returns 5 ]
    > Gandalf.
    [ returns 7 ]


Variables can hold either string or integer values at a given time.

### Printing

The verb `said` represents printing to the console. Other conjugations of the verb are valid, and they don't need to make grammatical sense in the sentence (although that's the fun of it),

A variable followed by a `said` and then a quoted string will print the value of the string, as well as returning that as the expression's value.

    > A cow said "moo".
    moo
    [ returns moo ]

A variable followed by `said` and then anything else will print the current value of the variable.

    > The judge said his verdict.
    [ returns 5 ]


### Assignment

Conjugations of the verb `to hear` followed by an expression represent assignment, and return the value after the assignment.

    > The man heard the woman say "hello".
    hello
    [ returns "hello"; man's value is "hello" ]


### Method declarations

Method declaration is of the form:

    > Fooing is where someone says their name.

And is used as it would be in valid english:

   > I foo. You foo. The horse foos.
   1
   3
   5

Methods currently take a single argument, which must be a variable, which binds to "someone", as well as "they", "their" and "them". Other variables are resolved to whatever exists in the environment at the time (i.e. not bound into a closure).

Actually, now I think about it, I don't really know what method declaration returns. Maybe I should look into that.

### Other builtins

`X listens to Y` is equivalent to `X = X + Y`

`X always METHOD` run `METHOD` 20 times (hella arbitrary, I know. It was for the demo).

# Runtime

The thing is interpreted, but is flakey as hell. Syntax errors, access errors, etc. will all basically just cause a runtime error. Needless to say, you shouldn't run this anywhere near production, because that would be like letting a kid with a toy stethoscope perform heart surgery.


# Future Development

### Modifiers

I'd like to use adjectives for some purpose slightly separate to verbs (i.e. methods).

I was thinking one obvious thing would be to make them modify the instantiation of the variables, so that it's easier to get bigger numbers in the program. For example `a big dog` could equal 30, with `big` being a x 10 modifier.

### Time travel

It would be nice if you could use methods etc. before they're defined, or even better, use future "facts" about

### Attributes

Something like `Sally's age` or `Obama's birth certificate` could actually refer to valued properties. This would make it fairly object oriented – my worry with this is that it might move the semantics too close to just describing the world, rather than highly obfuscated program.