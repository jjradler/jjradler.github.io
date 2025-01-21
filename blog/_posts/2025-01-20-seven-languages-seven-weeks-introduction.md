---
layout: post
title: Introduction to My <u>Seven Languages in Seven Weeks</u> Journey
Category: Projects
tags: [ Programming, Ruby, Erlang, Prolog, Clojure, Introduction, Personal, Projects, Learning]
description: >
    Introduction to my personal notes and “digital laboratory notebook” from my time with the book <u>“Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages”</u> by Bruce A. Tate
---

**My motivations:**

First, a disclaimer. *I do not own or have the rights to the materials in this book. I have purchased the book with my own money, and I have typed every word in these posts without copy-pasting, including the code snippets as a form of programming exercise. I do not intend to use any of this material for my own personal profit, and neither should you!*

Typically when I learn a new tool or programming language, I work through it alone using the tutorials on the project sites. I will sit and read, maybe with my buddy Fuzz (pictured in his typically bulbous, fuzzy form) laying across my feet, certainly while sipping a cup of coffee and furrowing my brow. ![](../../assets/img/fuzz-lozenge.png) 

Then more brow-furrowing occurs while I am tap-tap-tapping away at the console, or the REPL, or Vim, or at the IDE – depending on which is best suited for the language. 

However, I have done most of my professional work and personal software carpentry using `bash` scripts and various versions of `Python`. I use `C` and a touch of `C++` for my embedded programming projects for work, but also my hobbies (e.g., Arduino doo-hickeys, blinkenlights and fan control on Raspberry Pis, and the odd, fun noisemaking box.)  

I know a *tiny* bit about how [Ruby-on-Rails](https://rubyonrails.org) and [`Bundler`](https://bundler.io/guides/rationale.html) work from my time nibbling at their edges, reading blog posts [like this very helpful one by Bozhidar Batsov](https://batsov.com/articles/2025/01/12/running-jekyll-on-ruby-3-4/) (thank you, sir!), then subsequently patching this website when updated versions to one of its packages breaks the build process. In fact, that is what I was doing just before I wrote this paragraph!

Still, there are so many languages out there, and being naturally curious I wanted to know more about the different methods for handling problems. Then I discovered this really exciting book called [<u>Seven Languages in Seven Weeks</u>](https://pragprog.com/titles/btlang/seven-languages-in-seven-weeks/) by [Bruce A. Tate](https://redrapids.medium.com). When I cracked it open, I was shocked to find that this book intends to teach the structures and some of the theory behind seven rather exotic languages -- at least exotic to me, and with the exception of maybe Ruby which seems to be everywhere!

The languages in question include: 

1.	[`Ruby`](https://www.ruby-lang.org/en/):   A purely Object-Oriented language and apparently the basis for a great deal of commercial and open-source applications programming. Something I should probably learn! It might also give me another view on Object-Oriented programming, which is frankly something I struggled with for a long time in `Python` -- instead preferring to stick to a more Procedural and a crude Functional style of programming. 
2.	[`Scala`](https://www.scala-lang.org):  A language I dabbled in many years ago in a Coursera course. It is not terribly popular for some reason, but it runs on the Java Virtual Machine (JVM) and seems quite powerful as a hybrid Functional and Object-Oriented language with applications in scientific computing.
3.	[`GNU-Prolog`](http://www.gprolog.org):  Probably the most exotic and quirkiest on this list in my humble opinion -- as a *logic programming language* it appears to have a fundamentally different method for interacting with the computer based around logical Facts and Rules. I am really excited to play with it in a pseudo-structured way in the book! 
4.	[`Erlang`](https://www.erlang.org):  I keep hearing about this language on YouTube and how it hybridizes a number of seemingly disparate programming paradigms and contains some very powerful features. It is almost a complete unknown for me at this point, so I am excited to work with it! 
5.	[`Clojure`](https://clojure.org):  A predominantly functional language in the `LISP` family. I have been dancing around learning this one in earnest because it seems so strange, but it also seems like a really interesting way to approach programming problems using a Functional Paradigm. The syntax is bizarre to a long-time `C` or `Python` programmer like me, but it runs on the JVM and seems to have some really powerful tools for applications programming and "scripting". To my mind right now as a complete neophyte it reads like *"alternate reality Python in a functional style where it's all just generators and list comprehensions."*, but I have a feeling that might be overly reductive.
6.	[`Io`](https://iolanguage.org):  I know approximately *nothing* about `Io` *a priori*, so I am very excited to compile my own thoughts about the design of the language. What I do know is that it's a "prototyping" language where everything is an object. In that sense, maybe it is more similar to `Ruby`? Time will tell!
7.	[`Haskell`](https://www.haskell.org):  The big baddie in the Functional Programming space. It seems both polarizing and exciting and I can already imagine it will affect the way I approach problem solving in a functional fashion in my Python scientific computing code. 

These seven languages seem to run the gamut of type systems and programming paradigms implemented in technology today, so working with them will probably solidify my understanding of *strong vs. weak typing* and *dynamic vs static* typing through hands-on practice. 

I also thought the *best* way to deepen my knowledge of the different programming paradigms, design choices for languages, evaluation models, and typing models was to work my way through a smattering of languages in a somewhat structured way. 

Why have I waited until so far into my career to post a digital “laboratory notebook” about programming? 

Honestly, it’s anxiety and the classic “Developer’s Impostor Syndrome.” 

However, I was inspired by the author of <u>“Seven Languages”</u>. In his introduction, he tells us he is *not* an expert in many of these languages, but he wanted to learn them and understand them as deeply as he could, as quickly as possible. His motivations were stated to include improving skills and learning new problem solving methods in the other languages he is more “fluent” in.  

I felt inspired by this because you *do not have to have all the answers* to bring your voice to the table and help someone else with their craft. We all have different strengths and approaches. 

Accordingly, I have a few reasons to make the following digest posts of the book I am working through with a bit of my own commentary sprinkled in: 

1.  I want to learn these languages for my own self-edification and posting will give me some accountability.
2.  Maybe my perspective could unlock some ideas in your own mind, dear Anonymous Reader, and even though I am not an expert in these languages you can find something useful in what I have to say.
3.  It is a good way to show my own growth and motivation to better myself in this craft. 
4.  It will help me understand these concepts better and apprehend them more rapidly. Writing is truly one of my favorite ways to organize my thoughts and crystallize them into an understanding. 
5.  Online laboratory notebooks are a good way to keep track of what I have learned. Sometimes one needs to be reminded of what they can do (especially when one is having an episode of the dreaded Impostor Syndrome!)



I intend to publish code snippets from my REPL or text editors as I keyed them in and as they appear on my personal system (right now it’s an Apple Silicon Mac running MacOS). The exercises will be done by me and if I make any modifications to the listings from the book that I display, I will make a note of it. 

So far I am really enjoying his writing and I highly recommend you work thorugh his book yourself! I don’t know Bruce A. Tate, but if I ever meet him, I hope to buy him the beverage of his choice for inspiring me with his work. 

This ought to be fun!

---
