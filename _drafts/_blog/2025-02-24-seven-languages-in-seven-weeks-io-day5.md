---
layout: post
title: Seven Languages - Day 5 - Io
Category: Programming
tags: [ Io, Seven Languages, Programming, Learning, Prototype Languages ]
description: >
    Notebook from my second day of learning Io (Day 5 overall) from <u>Seven Languages in Seven Weeks</u> by Bruce A. Tate.
---

**Language 2:  Io**

**Day 5 - Introduction to Io (Io – Day 2):**

Greetings! It has been a couple of weeks since I last worked on this project. Things got a little nuts for a hot minute, but we’re back now and getting back into this weird, lovely prototype language! 

Today we will start probing what we can really *do* with `Io` as a programming language by leveraging the slots and messages structures to induce and shape behaviors. 

We will kick things off with loops and conditional statements. Then, moving on to operators we will explore how to shape the behaviors (and probably override) the operators. A deeper dive into messages allows us to get these objects *talking* to one another. Closing things out, we will also explore the concept of reflection in `Io` which will let us explore the ancestor prototypes of an object using a set of built-in methods. 

---

**Conditionals & Loops:**

First things first:  the most important thing to know when working with looping or recursive structures is that `Ctrl + C` breaks out of the loop. 

The very first thing we can try is the `loop` method: 

``` Io
Io 20151111
Io> loop("getting dizzy..." println)
...
getting dizzy...
getting dizzy...
^C
IOVM:
	Received signal. Setting interrupt flag.
getting dizzy...
  current coroutine
  ---------
  Coroutine callStack                  A4_Exception.io 244
  Coroutine backTraceString            A4_Exception.io 274
  Coroutine showStack                  System.io 62
  System userInterruptHandler          [unlabeled] 0
  Sequence println                     Command Line 1
```

This did, however, throw us out of the entire REPL, so we will have to reenter it as we always have (by typing `io` at the command line.)

So why do we need loops? 

They can be useful with concurrency constructs (more on this later), although one would typically prefer to use a *conditional* looping construct to avoid having to kill the entire REPL, among other reasons. 

A conditional looping structure we can use in `Io` is `while`. As with many other languages, the `while` loop takes a condition and a message to evaluate. 

NOTE: In `Io` a semicolon (`;`) concatenates two distinct messages! This feels a bit like concatenating commands in a terminal shell, doesn’t it?

``` Io
Io 20151111
Io> i := 1
==> 1
Io> while(i <= 11, i println; i = i + 1); "This one goes up to 11!" println
1
2
3
4
5
6
7
8
9
10
11
This one goes up to 11!
==> This one goes up to 11!
```

We can see here some familiarity with the `C` languages. Notice that the syntax takes a conditional terminated with a comma, then the execution statement terminated with the semicolon, then an iterating step. 

This one-liner causes two things to happen sequentially: 

	1. We enter the loop with `i := 1` and continue until `i equals 11`
	1. We print “This one goes up to 11!”

![marty-amp](../../assets/img/marty-amp.gif)

Ok, so that’s useful. It’s only one of *the most basic constructs in any usable language*, right? What else have we got…

There is a `for` loop structure that is also useful. It takes the name of the counter, the first value, the last value, an optional increment, and a message with a designated sender. The iterative part of this structure is reminiscent again of `C` language `for` loops. See? 

``` Io 
Io> for(i, 1, 11, i println); "This one goes to 11!" println
1
2
3
4
5
6
7
8
9
10
11
This one goes to 11!
==> This one goes to 11!
```

Same, same. The syntax is a little strange, but it seems pretty easy to use despite the usage of syntactic sugar (like in Python and other scripting languages.)

Similarly, we can use the optional increment like so: 

``` Io
Io> for(i, 1, 11, 2, i println); "This one goes to 11, two!" println
1
3
5
7
9
11
This one goes to 11, two!
==> This one goes to 11, two!
```

Simple enough. 

Although, it feels like we’re just sticking more and more parameters into the `for` loop method call. Turns out you can have an arbitrary number of parameters. The optional parameter in the last example is the fourth parameter. Like in some other languages, `Io` will allow you to attach additional parameters, which is powerful. 

Although it presents yet another new and interesting way to put a hole in your metaphorical foot. The interpreter/compiler for `Io` won’t hold our hands here, so we need to pay attention! 

The book gives us an example: 

``` Io 
Io> for(i, 1, 2, 1, i println, "extra argument")
1
2
==> 2
Io> for(i, 1, 2, i println, "extra argument")
2
==> extra argument
```

Notice how the extra argument `1` is omitted in the second `for` loop. The message is now `“extra argument"`, and stranger still, <u>you’re working in increments of `i println`!</u> which evidently returns `i`? 

If this was buried deeply in a dense codebase you would absolutely have shot yourself in the foot in a difficult-to-debug way. 

So that’s fun. 

![gonna-be-sick](../../assets/img/im-gonna-be-sick.jpg)

Meanwhile, the `if` control structure is a function of the form 

``` Io
if(condition, true code, false code)
```

which feels a bit like ternary conditional statements in other `C`-derived languages. Let’s see how it works in practice: 

``` Io
Io> if(true, "It is true", "It is false.")
==> It is true
Io> if(false) then("It is true") else ("it is false")
==> nil
Io> if(false) then("It is true." println) else("It is false." println)
It is false.
==> nil
```

Notice that the return value for the first statement is `It is true` whereas the return value for the more expanded conditional statements in the two subsequent listings return `nil` values, but will execute the blocks in the `then()` and `else()` statements. In general: 

``` Io
if(condition) then(<executable block>) else(<executable block>)
```

Useful enough, right?

----

**Operators:**

Many prototype languages allow for syntactic sugar to allow *operators*, like object-oriented languages. In general, *operators* are <u>special methods</u> like `/` or `+` that take a special form. 

The operator table can be probed in `Io` like so:

``` Io
Io> OperatorTable
==> OperatorTable_0x600000985e80:
Operators
  0   ? @ @@
  1   **
  2   % * /
  3   + -
  4   << >>
  5   < <= > >=
  6   != ==
  7   &
  8   ^
  9   |
  10  && and
  11  or ||
  12  ..
  13  %= &= *= += -= /= <<= >>= ^= |=
  14  return

Assign Operators
  ::= newSlot
  :=  setSlot
  =   updateSlot

To add a new operator: OperatorTable addOperator("+", 4) and implement the + message.
To add a new assign operator: OperatorTable addAssignOperator("=", "updateSlot") and implement the updateSlot message.
```

Notice the numbers to the left of each operator? This is the precedence of the operators listed to the right of each number. Arguments closer to `0` bind first, just as one would expect from pretty much any other programming languages. Similarly, there are orders of precedence to mathematical operators (remember “Order of Operations” from algebra?) Interestingly, the logical and comparison operators comprise the whole bottom half of the precedence table, with comparison and logical test operators coming first, followed by incrementing operators, rounding out the list with the combined (shorthand) operators in precedence position `13`. 

Just like in algebra, operator precedence can be overridden with `()`. 

Also, it appears the assigment operators (`=`, `:=`, `::=`) are different from the other operators in `Io`. 

That’s cool and all, but why do we need a whole section of this lesson about *operators*? 

Well, it turns out we can define our own operators in `Io`, which seems like a powerful little bit of kit. In this example, the book walks us through defining an `exclusive or` (or `xor`) operator. 

The specification of the `xor` operator is that <u>it returns `true` if exactly one of the arguments is `true`, and `false` otherwise.</u>

Conveniently, `Io` has a built-in mechanism for adding a custom operator to the system operator table! 

``` Io 
Io> OperatorTable addOperator("xor", 11)
==> OperatorTable_0x600000985e80:
Operators
  0   ? @ @@
  1   **
  2   % * /
  3   + -
  4   << >>
  5   < <= > >=
  6   != ==
  7   &
  8   ^
  9   |
  10  && and
  11  or xor ||
  12  ..
  13  %= &= *= += -= /= <<= >>= ^= |=
  14  return
```

Cool! We added `xor` to line `11` in the system operator table. Looks like that was achieved by using the method `addOperator()` defined in a slot on the *object* `OperatorTable`. This follows the same behavior we have already explored for objects and slots, which has a satisfying consistency to it, don’t you think? 

So now what? 

Now, we need to implement the `xor` method *on `true` and `false` objects.* Remember that these are singletons (we discussed this previously) and they are objects like everything else in `Io`. Given that, we should be able to add the `xor` method we need to implement as a slot on each. So how do we do this? 

```Io
Io> true xor := method(bool, if(bool, false, true))
==> method(bool,
    if(bool, false, true)
)
Io> false xor := method(bool, if(bool, true, false))
==> method(bool,
    if(bool, true, false)
)
```

So now, testing this, we see that: 

``` io
Io> true xor true
==> false
Io> true xor false
==> true
Io> false xor true
==> true
Io> false xor false
==> false
```

Seems to work fine! It’s very, very crude and simple as an illustrative example, but it works within this context just as we would expect. 

What happens here is the syntax `true xor true` gets parsed by `Io` as `true xor(true)`, just like the other methods and messages we explored earlier.

---

**Messages:**

In the book, Bruce talks about how one of the committers of `Io` remarked to him that <u>almost everything in `Io` is a message</u>. 

Essentially, everything but comment markers and the comma (`,`) between arguments are actually messages.  

Everything. 

From this, we can infer that the way to mastery of `Io` is to learn how to manipulate messages in various ways beyond basic invocations. 

The capability of most note here is *message reflection*. This is where you can query and characteristic of *any* message and act appropriately. 

In `Io`, a message has three components: 

1. `sender` sends the message
2. `target` executes the message
3. `arguments` give the `target` information needed to execute the message. 

The `call` method gives you access to metadata about any message. the book gives an example using a few objects:  the `postOffice` that gets messages and the `mailer` that delivers them. 

First let’s make the `postOffice`:

``` Io
Io> postOffice := Object clone
==>  Object_0x600000629680:
Io> postOffice packageSender := method(call sender)
==> method(
    call sender
)
```

Next let’s create the `mailer`: 

``` Io
Io> mailer := Object clone
==>  Object_0x6000006d7940:

Io> mailer deliver := method(postOffice packageSender)
==> method(
    postOffice packageSender
)
```

So what did we just do here by creating the method `deliver`?  The `deliver` slot of `mailer` (`mailer deliver`) sends a `packageSender` message to `postOffice`. When the message `packageSender` arrives at `postOffice` we execute the `call` method on `sender`, yielding the metadata about the `sender`. We can see this in action here: 

``` io
Io> mailer deliver
==>  Object_0x6000006d7940:
  deliver          = method(...)
```

Notice that `mailer` is `Object_0x6000006d7940`, which is reflected by the `mailer deliver` method when it reaches `postOffice`. 

It seems a bit convoluted, but if you take a moment to trace the calls and substitute into the methods of the `target` (i.e., the `postOffice`) it should become clearer. 

In other words, the object with the `deliver` method (`mailer`) is the object that sent the message (the `sender`). Turns out we cal also get the `target` by defining another method in a slot on `postOffice` like this: 

``` io
Io> postOffice messageTarget := method(call target)
==> method(
    call target
)
Io> postOffice messageTarget
==>  Object_0x600000629680:
  messageTarget    = method(...)
  packageSender    = method(...)
```

We can see here that invoking the `messageTarget` slot on `postOffice` gives us information on the `postOffice` itself. 

The original message name and arguments are obtained like this: 

``` io
Io> postOffice messageArgs := method(call message arguments)
==> method(
    call message arguments
)
Io> postOffice messageName := method(call message name)
==> method(
    call message name
)
Io> postOffice messageArgs("one", 2, :three)
==> list("one", 2, : three)
Io> postOffice messageName
==> messageName
```

It’s pretty clear from this that `Io` has a number of methods already available for performing message reflection. So when does `Io` compute a message? 

Arguments are passed as values on stacks in most programming languages. The book notes that Java computes each value of a parameter first and then places those values on the stack. 

This is not the case with `Io`. 

`Io` passes the message itself and the context, then the receivers evaluate the message. 

Indeed, you can implement control structures with messages in `Io`. Harkening back to the `if` statement in `Io`, the form is `if(booleanExpression, trueBlock, falseBlock)`. If you wanted to implement an `unless`, you could do the following: 

``` io
/* io/unless.io */
unless := method(
	(call sender doMessage(call message argAt(0))) 
	ifFalse(call sender doMessage(call message argAt(1))) 
	ifTrue(call sender doMessage(call message argAt(2)))
)

unless(1==2, write("One is not two\n"), write("one is two\n"))
```

In this example it is important to note that `doMessage` is like `eval` in Ruby, but lower-level. Where Ruby’s `eval` evaluates a string as a piece of code, `doMessage` executes an arbitrary message. 

Remember that `call` provides access to the inner workings of the object `message`. [From the `Io` documentation](https://iolanguage.org/reference/): the method `argAt()` returns the message's `argNumber` arg. This is shorthand for same as `call message argAt(argNumber)`. So this example uses message reflection methods to change the flow of the program given how `Io` parses and interprets the structure of the `unless()` method’s own structure! 

Furthermore, `Io` is interpreting the message parameters but it delays binding and execution. 

In contrast, a typical Object Oriented language interpreter/compiler would evaluate all the arguments – including both of the code blocks – and place those *return values* on the stack. 

`Io` instead, follows this flow: 

Say the object `westley` sends a message like `princessButtercup unless(trueLove, ("It is false" println), ("It is true" println))`. The following steps would occur: 

1.  `westley` object sends the message. 
2. `Io` takes the interpreted message the context (*i.e.,* the call sender, target, and message) and puts it all on the stack.
3. Now `princessButtercup` evaluates the message. There is no `unless` slot, so `Io` climbs back up the chain of prototypes until it finds `unless`. 
4. `Io` begins executing the `unless` message. First, it executes `call sender doMessage(call message argAt(0))`. This simplifies to `westley trueLove`. Here we will follow the story of The Princess Bride (as the book does!) and state that `westley` has a slot `trueLove` that evaluates to `true`. 
5. Since the message is not false (*i.e.,* it is true!) the third code block in `unless` is executed: `call sender doMessage(call message argAt(2))` which simplifies down to `westley ("It is true" println)`. 

Essentially, we make use of the face that `Io` is not executing the arguments to compute a return value up front, instead only simplifying the statements as needed to propagate through the `unless` control structure. 

We’ve looked at the message *behavior* with reflection, but can we look at the *state* of an object? Turns out we can with… 

---

**Reflection:**

Again, reflection helps us out here. We can iterate up the prototype chain for an object using the method we define called `ancestors` on the `Object` object. Therefore, clones (any clones!) will carry the `ancestors` method in the following listing. Then we create a few objects and probe their state and slots with the `ancestors` method. 

``` io
/* io/animals.io */
Object ancestors := method(
    prototype := self proto
    if(prototype != Object,
        writeln("Slots of ", prototype type, "\n---------------")
        prototype slotNames foreach(slotName, writeln(slotName))
            if(prototype != nil, writeln(prototype ancestors))
      )
)

Animal := Object clone
Animal speak := method(
    "ambiguous animal noise" println)

```

First, the `Animal` prototype is created from `Object`. From `Animal` we create `Duck`, and from `Duck` we create `disco`. Then calling `ancestors` on `disco`, we can list out the properties of the prototypes further up the chain from `disco`. 

Running this code yields: 

``` shell
$ io animals.io
Slots of Duck
---------------
walk
type
speak
Slots of Animal
---------------
type
speak
false
nil
```

Every object has its prototype, and each of those prototypes has its own slots. In `Io`, dealing with reflection has *two parts*. 

First, we looked at *message reflection* in the `postOffice` example. Then we looked at *object reflection* with the `animals.io` listing. *Object reflection* means dealing with objects and the slots on those objects. Note that there are zero classes. None. Anywhere. 

---

**Io Day 2 Self-Study:**

Lots of activities today! Let’s get started! 

**Do:**

*1. A Fibonacci sequence starts with two 1s. Each subsequent number is the sum of the two numbers that came before: 1, 1, 2, 3, 5, 8, 13, 21,… Write a program to find the nth Fibonacci number. `fib(1) ==> 1`, `fib(4) ==> 3`. As a bonus, solve the problem with both recursion and with loops.*

``` io
/* fibonacci.io */ 

```





*2. How would you change `/` to return `0` if the denominator is zero?*

Here we need to modify (override?) an existing operator (`/`) to have different behavior when `0` is passed as an argument to the `/` message (method). So first let’s probe the `/` operator in the REPL to see what it normally does and how it goes about doing it: 

``` io
```





```io


```





*3. Write a program to add up all of the numbers in a two-dimensional array.*

Let’s create a simple 2-dimensional array to work with. 

``` io
```





*4. Add a slot called `myAverage` to a list that computes the average of all the numbers in a list. What happens if there are no numbers in a list? (Bonus: Raise an `Io` exception if any item in the list is not a number.*

``` io


```





*5. Write a prototype for a two-dimensional list. The `dim(x, y)` method should allocate a list of `y` lists that are `x` elements long. `set(x, y, value)` should set a value and `get(x, y)` should return that value.*

``` io


```





*6. Bonus: Write a transpose method so that (`new_matrix get(y, x)) == matrix get(x, y)` on the original list.*

``` io
```





*7. Write the matrix to a file and read the matrix from a file.*

``` io 
```





*8. Write a program that gives you ten tries to guess a random number from 1 to 100. If you would like, give a hit of ‘hotter’ or ‘colder’ after the first guess.*

``` io
```







---

**Summary:**



---
