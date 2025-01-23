---
layout: post
title: Seven Languages - Day 2 - Ruby
Category: Programming
tags: [ Ruby, Seven Languages, Programming, Learning ]
description: >
    Notebook from my second day of learning Ruby from <u>Seven Languages in Seven Weeks</u> by Bruce A. Tate.
---

**Language 1:  Ruby**

**Day 2**

Here we go again! 

At the end of [Day 1](https://jjradler.github.io/blog/2025-01-21-seven-languages-seven-weeks-ruby-day1/) it felt very empowering to feel like I could build a program that does something — even if that thing is silly. 

Now I think things are about to get more serious in terms of probing the more powerful Object-Oriented (OO) features of Ruby.

**Functions:**

A function can be written right on the `irb` console (much like in Python for my fellow Pythonistas out there) as follows: 

``` ruby
irb(main):001* def tell_the_truth
irb(main):002*   true
irb(main):003> end
=> :tell_the_truth

```

Just like with Python, the `def` keyword is used to start the function, however unlike many of the other languages I’ve used, no parameters or parentheses are required to define the function. The `end` keyword is *required* to complete the function definition. 

As a quick aside, I think the colon (`:`) in front of the function name `:tell_the_truth`  is probably going to be important later.

To use the function, just invoke it in the `irb` REPL like so: 

``` ruby
irb(main):004> tell_the_truth
=> true
```

*Every function returns something.* If an explicit `return` is not given, the value returned will be the last expression evaluated before exiting the function scope. 

Functions are also objects, just like everything else in Ruby! 

There’s no doubt *much, much more* to learn about functions, passing parameters, passing functions *as parameters*, etc. but I will be patient… for now! 

---

**Arrays:**

Apparently `arrays` are a big deal among the ordered data collections in Ruby, alongside `hashes` (more on these later). I’ve already used some arrays in my Day 1 code exercises because we got a little teaser of arrays among the code snippets from the Day 1 lesson. However, it should be noted that there are additional features and tasty bits of *syntactic sugar* associated with arrays in Ruby. 

Examples of arrays are: 

```ruby
irb(main):005> animals = ['lions', 'tigers', 'bears']
=> ["lions", "tigers", "bears"]
irb(main):006> puts animals
lions
tigers
bears
=> nil
```

Array indexing works similarly to pretty much every other language, which is comforting and familiar: 

``` ruby
irb(main):007> animals[1]
=> "tigers"
irb(main):008> animals[0]
=> "lions"
irb(main):009> animals[-2]
=> "tigers"
irb(main):010> animals[-1]
=> "bears"

```

Note also that the *negative indexes* are, in similar fashion to Python again, will yield the values from the *end* of the array. This feature is a good example of the previously mentioned *syntactic sugar*.

What is *not* an example of syntactic sugar, however, is a `Range` object in Ruby. A `Range` is similar to the concept of an array slice in other languages at first glance. 

``` ruby
# now for some Ranges behaving like array slices.
irb(main):011> animals[0..1]
=> ["lions", "tigers"]
irb(main):012> animals[1..2]
=> ["tigers", "bears"]
# note that (1..2) has a different class.
irb(main):014> (1..2).class
=> Range
```

A `Range`, `(X..Y)` means all values from the first value `X` to the second value `Y` <u>inclusive</u>. 

Arrays can hold all sorts of datatypes, including other `array` objects, producing multi-dimensional arrays. There are some gotchas to be aware of with arrays, however. 

Arrays, being objects, will have methods of course! As with the number `4` we saw previously, the `*.class` method is available for an array. Trying it on array `a`:

``` ruby
irb(main):001> a.class
(irb):1:in '<main>': undefined local variable or method 'a' for main (NameError)
	from <internal:kernel>:168:in 'Kernel#loop'
	from /usr/local/Cellar/ruby/3.4.1/lib/ruby/gems/3.4.0/gems/irb-1.14.3/exe/irb:9:in '<top (required)>'
	from /usr/local/opt/ruby/bin/irb:25:in 'Kernel#load'
	from /usr/local/opt/ruby/bin/irb:25:in '<main>'
```

… produces an error since I haven’t told `irb` that I’m declaring it as an array. What about implying that it’s an array by declaring the first element to be `0`? 

``` ruby
irb(main):003> a[0] = 0
(irb):3:in '<main>': undefined local variable or method 'a' for main (NameError)
	from <internal:kernel>:168:in 'Kernel#loop'
	from /usr/local/Cellar/ruby/3.4.1/lib/ruby/gems/3.4.0/gems/irb-1.14.3/exe/irb:9:in '<top (required)>'
	from /usr/local/opt/ruby/bin/irb:25:in 'Kernel#load'
	from /usr/local/opt/ruby/bin/irb:25:in '<main>'
```

Nope! It will evidently need initialization as an empty array first before we can explore what is inside. However, initializing it is super easy: 

``` ruby 
# plonk that element down in those square brackets and you got yourself an array!
irb(main):006> a = [0]
=> [0]
# see? 
irb(main):007> a.class
=> Array
# you could also just use an empty pair o' brackets to define an empty array. Which is good, because these are indeed useful!
irb(main):008> a = []
=> []
irb(main):009> a.class
=> Array
irb(main):010> [].class
=> Array
```

It seems `[]` and `[]=` are methods of the `array` class, as indicated in `irb` with

``` ruby
irb(main):014> [1].methods.include?(:[])
=> true
irb(main):015> [1].methods.include?(:[]=)
=> true
```

These also qualify as syntactic sugar allowing access to the array structure. However, to use that sweet, sweet sugar, the array needs to be initialized first.

``` ruby
irb(main):025> a = []
=> []
irb(main):026> a.class
=> Array
irb(main):027> a.methods.include?(:[])
=> true
```

Another useful feature of arrays in Ruby is that they do not need to be homogeneous (i.e., all the same datatype, or even the same shape.)

``` ruby
irb(main):016> a[0] = 'zero'
=> "zero"
irb(main):017> a[1] = 1
=> 1
irb(main):018> a[2] = ['two', 'things']
=> ["two", "things"]
# viewing the array structure now that it's populated: 
irb(main):020> a
=> ["zero", 1, ["two", "things"]]
```

Furthermore, multiple-dimensional arrays can be created by embedding arrays within another array and the i-jth element ($a_{ij}$) can be accessed using the `a[i][j]` syntax, like in many other languages.. 

``` ruby
irb(main):021> a = [[1, 2, 3], [10, 20, 30], [40, 50, 60]]
=> [[1, 2, 3], [10, 20, 30], [40, 50, 60]]
irb(main):022> a[0][0]
=> 1
irb(main):023> a[2][2]
=> 60
```

OK, so what happens if we attempt to access an element outside the defined array? 

``` ruby
irb(main):024> a[3][3]
(irb):24:in '<main>': undefined method '[]' for nil (NoMethodError)
	from <internal:kernel>:168:in 'Kernel#loop'
	from /usr/local/Cellar/ruby/3.4.1/lib/ruby/gems/3.4.0/gems/irb-1.14.3/exe/irb:9:in '<top (required)>'
	from /usr/local/opt/ruby/bin/irb:25:in 'Kernel#load'
	from /usr/local/opt/ruby/bin/irb:25:in '<main>'
```

Ah, that’s comforting. It means you can’t run over the bounds of the array you defined or the interpreter will complain. 

How about slicing a multidimensional array?  I need to do this a lot in my Python code: 

``` ruby
a = [[1, 2, 3], [10, 20, 30], [40, 50, 60]]
=> [[1, 2, 3], [10, 20, 30], [40, 50, 60]]

# this is a slice to return a subset of the array.
a[1,2]
=> [[10, 20, 30], [40, 50, 60]]

# and this is an element-wise index access. 
a[1][2]
=> 30

```



It also turns out there are handy features in the `array` collection API that enable the construction of queues, linked lists, stacks, or sets. Here’s an example of a stack structure:

``` ruby
# define a stack as an empty array
irb(main):038> stack = []
=> []
# the *.push(value) method pushes value to the stack.
irb(main):039> stack.push(1)
=> [1]
# do it again.
irb(main):040> stack.push(2)
=> [1, 2]
# now the *.pop method returns the last-in-first-out value (2 in this case)
irb(main):041> stack.pop
=> 2
# now let's push a new arbitrary value, say 345.
irb(main):042> stack.push(345)
=> [1, 345]
# popping it off again...
irb(main):043> stack.pop
=> 345
# now let's pop the last value in the stack
irb(main):044> stack.pop
=> 1
# and we're left right where we started! 
irb(main):045> stack
=> []
```

So that’s pretty nifty, I would say! Seems like a nice structure for LIFO data buffers, too. 

Arrays also have iterator methods (like `*.each` and `*reverse_each`) that we might discuss a bit later for implementing things like queues and buffers. 

---

**Hashes**

Collections are buckets for objects. The `hash` structure implements a label (the `key`) for each object contained within. 

Pythonistas, think about `Dict` structures. 

A hash is a collection of `key-value` pairs created with the `<key> => <value>` syntax within curly brackets (`{}`) as follows: 

``` ruby
irb(main):050> numbers = {1 => 'one', 2 => 'two'}
=> {1 => "one", 2 => "two"}
irb(main):051> numbers.class
=> Hash
```

The values can be retrieved using a similar syntax to arrays, like this: 

``` ruby
irb(main):053> numbers[1]
=> "one"
irb(main):054> numbers[2]
=> "two"
```

Like `arrays`, `hashes` can contain an eclectic mix of different objects, like this listing from the book: 

``` ruby
irb(main):055> stuff = {:array => [1, 2, 3], :string => 'Hi, Mom!'}
=> {array: [1, 2, 3], string: "Hi, Mom!"}
```

(This is a bit of a preview of the `:symbol` syntactic feature in Ruby, I think.)

Retrieving these values is straightforward, 

``` ruby
irb(main):057> stuff[:string]
=> "Hi, Mom!"
```

But, wait, what if we omit the `:` before `string`?

``` ruby
irb(main):056> stuff[string]
(irb):56:in '<main>': undefined local variable or method 'string' for main (NameError)
Did you mean?  String
	from <internal:kernel>:168:in 'Kernel#loop'
	from /usr/local/Cellar/ruby/3.4.1/lib/ruby/gems/3.4.0/gems/irb-1.14.3/exe/irb:9:in '<top (required)>'
	from /usr/local/opt/ruby/bin/irb:25:in 'Kernel#load'
	from /usr/local/opt/ruby/bin/irb:25:in '<main>'
```

Ah, interesting. Clearly there is a distinction between `string` and `:string` in this context. Stay tuned to figure out why! 

Still, this is quite straightforward, as a `hash` behaves like an `array` but instead of accessing the data with an integer index, the `hash` accesses the data using an arbitrary `key`. 

**Symbols (A Sidebar)**

Here we go! The book says:

>The last hash is interesting because I’m introducing a symbol for the first time. A symbol is an identifier preceded with a colon, like :symbol. Symbols are great for naming things or ideas. Although two strings with the same value can be different physical strings, identical symbols are the same physical object.

Tate, Bruce. Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages (Pragmatic Programmers) (p. 49). (Function). Kindle Edition. 

The example given in the book shows that an object’s ID (as I understand it, it’s unique place where it lives in the system memory?): 

``` ruby
irb(main):058> 'string'.object_id
=> 414704
irb(main):059> 'string'.object_id
=> 424112
irb(main):060> ':string'.object_id
=> 434136
irb(main):061> :string.object_id
=> 3891468
irb(main):062> :string.object_id
=> 3891468
```

Notice the ID numbers for the <u>Symbol</u> `:string` are identical indicating the reference to symbol `:string` "lives" in the same memory location. Meanwhile, the return values for `*.object_id` when the method is applied to `'string'` are all different, indicating that they “live” in different locations in the system memory. 

**Now Back to Hashes!**



---

**Code Blocks and Yield**



---

**Defining Classes**



---

**Mixin Classes and How To Write Them**



---

**Modules, Enumerable, and Sets**



---

**Wrapping Up Day 2**



---

**Day 2 Self-Study**

**Find & Answer:**

*1. Find out how to access files with and without code blocks. What is the benefit of the code block?*





*2. How would you translate a hash into an array?  Can you translate arrays to hashes?*





*3. Can you iterate through a `hash`?*





*4. You can use Ruby arrays as stacks. What other common data structures do arrays support?*





**Do & Build:**

*1a. Print the contenst of an array of sixteen numbers, four numbers at a time, using just `each`.*





*1b. Now, do the same with `each_slice` in `Enumerable`.*





*2. The `Tree` class was interesting, but it did not allow you to specify a new tree with a clean user interface. Let the initializer accept a nested structure of hashes. You should be able to specify a tree like this: `{'grandpa' => {'dad' => {'child 1' => {}, 'child 2' => {} }, 'uncle' => {'child 3' => {}, 'child 4' => {} } } }`.*





*3. Write a simple `grep` that will print the lines of a file having any occurrences of a phrase anywhere in that line. You will need to do a simple regular expression match and read lines from a file. (This is surprisingly simple in Ruby.) If you want, include line numbers in the output.*



---

*Closing out Day 2:*



---

