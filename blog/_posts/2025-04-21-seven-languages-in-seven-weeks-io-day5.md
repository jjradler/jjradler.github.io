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

Greetings! It has been a couple of weeks since I last worked on this project. Things got a little nuts in life for a few minutes, but we’re back now and getting back into this weird, lovely prototype language! 

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

Pseudocoding this out starting with the naive looping approach to the sequence: 

``` pseudocode
DEFINE method fib with targetIndex parameter
	SET currentValue to 1
	SET previousValue to 0
	SET nextValue to 0

	FOR(SET index to 0, SET stopIndex to targetIndex, SET stepSize to 1)
		IF index < 2 THEN
			SET nextValue to currentValue
		ELSE
			SET nextValue to currentValue + previousValue
		END
	
		SET previousValue to currentValue
		SET currentValue to nextValue

	Print the currentValue
	END
END
```

One implementation of this algorithm in `Io` code could be:

``` io
/* fibonacci.io */
/* First the "naive" loop-based approach: */ 
fib := method(targetIndex,
	currentValue := 1
	previousValue := 0
	nextValue := 0

	for(i, 0, targetIndex, 1,
		if(i < 2) then(nextValue := currentValue) else(nextValue := currentValue + previousValue);
		previousValue := currentValue;
		currentValue := nextValue;
	)

	currentValue println
)

/* Now for a little test */ 
fib(11)
```

This works, running `fibonacci.io` on the Command Line. 

Another method is to use recursion. Indeed, this is *the classic* recursion use case that is brought up in programming cases around the world whenever recursion is introduced. 

Here again we’ll use the pseudocode representation to start for clarity. First, let’s look at what’s required in most languages for using recursion. As anyone who has played with this technique knows, you can lock up an application pretty quickly by spiraling down into oblivion if the correct conditions are not set up first. 

*Every* recursive function requires at minimum two components: 

1. *The Base Case:* The base case is what keeps things sane and in control. Essentially, this is the smallest subdivision of the problem and sets the “floor” to keep the function from falling into an infinite void...
2. *The Recursive Case:* This case is where the function will call itself with modified arguments to traverse the space of smaller problems. Without getting into too much detail about recursion, this is where the magic happens. 

Now, given that information, we need a base case. The classic base case for a Fibonacci sequence, `fib(N)` for `N < 2`, as the zeroth through second indices of the sequence correspond to `0, 1, 1, …`. 

As it turns out, there are a *bunch* of possible recursive solutions for recursion, but since this post is about methods in `Io` and not about recursion itself, let’s use the simple, naive approach to recursion as well: 

``` pseudocode
DEFINE fib_recursive with target index N
	IF N <= 1 THEN print the base case value N
	ELSE CALL fib_recursive(N - 1) + CALL fib_recursive(N - 2)
```

This seems very, very terse — even in pseudocode. But is it correct?

The solution we implement in `Io`:

``` Io
/* fibonacci_recursive.io*/
fib := method(targetValue, 
	if(targetValue <= 1) then(targetValue println) else(fib(targetValue - 1) + fib(targetValue - 2))
)
```

As a quick aside, looking through [the `Io` language reference documentation](https://iolanguage.org/guide/guide.html#Objects-Methods), I found a very simple syntax for pulling command line arguments into `Io` programs to make testing a little quicker: 

``` io
System args at(1)
```

As with many other programming languages, `Io` uses a list of arguments (recalling how we work with `list` structures in `Io`), that we can access using the `at(index)` method. The item at `System args` index 0 is the name of the file, and the item at index 1 is the first command line argument, *formatted as a string*. This pattern continues for two or more command line arguments, accessible using the same pattern, `at(index)`. 

We need the value we would pass into the program as an integer. Conveniently, `Io` has a built-in method that is quite useful here: `asNumber`. The `asNumber` message passed to a string converts the string  object into a `Number` object. Therefore, we can pass in the number we need for evaluating `fib(n)` as follows: 

``` io
System args at(1) asNumber
```

So adding this to the bottom of our program like this: 

``` io
/* fibonacci_recursive.io*/
fib := method(targetValue,
	if(targetValue <= 1) then(value := targetValue) else(value := fib(targetValue - 2) + fib(targetValue - 1));
    returnValue := value;
)

fib(System args at(1) asNumber) println
```

gives us the ability to pass in a value on the Command Line and test the function!

Finally, testing it on the command line, running it as we always do from an `Io` file, we see the results of our tests!

``` shell
$ io fibonacci_recursive.io 7
13
$ io fibonacci_recursive.io 20
6765
$ io fibonacci_recursive.io 30
832040
```

However, as stated, this is indeed a naive attempt to recursively calculate the Fibonacci sequence. It can actually be quite slow for larger numbers as we aren’t employing more efficient techniques like memoization. For example, there is a delay of several seconds to calculate `fib(30)`. 

---

*2. How would you change `/` to return `0` if the denominator is zero?*

Ok, so this seems rather daunting at first until we remember that `operators` are *messages* between objects in `Io`, producing a result. For example, the code `10 / 2` yields value `5`, but what’s really happening is that the <u>message</u> `/` is being passed to `10` with the `Number` with value `5` <u>passed as a parameter</u>! 

It could be rewritten using a more explicit method syntax as `10 /(2)`. 

Thus, we need to adapt the behavior of the `/(Number)` method on `Number` objects to accomplish this task of making a “safe division”. We can lay this out more clearly as a specification to simplify our discussion a bit. 

<u>Specification:  The divide method `/()` returns value `0` for the case where the argument, `arg`, of `/(arg)` equals `0`.</u>

Currently, the behavior of the `/` special method (operator, remember!) in the REPL is as follows: 

``` io
Io> 10 / 0
==> inf
Io> 0 / 0
==> nan
```

Probing the nature of `Number` objects (like `0`, for example) shows us that there is indeed a `slot` named `/`. 

```io
Io> Number slotNames
==> list(floor, floatMax, atan2, floatMin, unsignedIntMax, asHex, >>, |, isGraph, shiftLeft, isPrint, shortMin, shortMax, factorial, ceil, squared, atan, asJson, asString, %, asUint32Buffer, cos, bitwiseAnd, bitwiseXor, justSerialized, isNan, at, tan, isUppercase, isControlCharacter, cubed, clip, sin, abs, -, isEven, +, isInASequenceSet, isDigit, acos, isOdd, asCharacter, permutations, toBase, minMax, constants, pow, asSimpleString, exp, repeat, roundDown, between, log10, &, integerMin, bitwiseOr, asOctal, longMin, log2, isSpace, asUppercase, bitwiseComplement, toBaseWholeBytes, isLowercase, log, max, <<, isLetter, combinations, isAlphaNumeric, asBuffer, round, min, asNumber, longMax, sqrt, negate, ^, asLowercase, isPunctuation, toggle, **, asin, /, *, shiftRight, integerMax, isHexDigit, asBinary, unsignedLongMax, mod)
```

This implies that we can modify the `slot` for the special method `/` to change the behavior to meet our specification outlined above! 

However, I am reluctant to alter the original method without backing it up somehow in case I break the interpreter (we’ve seen how that’s possible and not recommended!). 

Note here that we need to reference the slot name in double-quotes for the `getSlot()` method to be able to make any sense of it. (I think this might just be a quirk of the slot being named with a special character?)

``` io
Number setSlot("standardDivide", Number getSlot("/"))
```

This should make it so `standardDivide` replicates the behavior of the original operator in the `/` slot. Let’s test it! 

``` io
Io> Number setSlot("standardDivision", Number getSlot("/"))
==> Number_/()
```

Testing this, we see that it works: 

``` io 
Io> 10 standardDivision(2)
==> 5
```

Ok, that makes me feel a bit better about this next part:  let’s have some fun with the builtin operator `/`. We’ll change its behavior by essentially intercepting the `target` (in this case we’ll call it `denominator` just for clarity) and checking its value, only passing it on to the `standardDivision()` operator we made out of the original division operator previously. Good thing we kept that! 

``` io
Number setSlot("/", method(denominator, if(denominator == 0) then(return(0)) else(return (standardDivision(denominator)))))
```

So does it work? Checking it on the REPL: 

``` io
Io> 10 / 0
==> 0
Io> 10 / 2
==> 5
Io> 344.5 / 0.0
==> 0
```

Bingo! That worked.  

Such power… but so much responsibility…

---

*3. Write a program to add up all of the numbers in a two-dimensional array.*

Let’s create a simple 2-dimensional array to work with first. 

``` io
a := list( list(0, 1), list(2, 3))
```

It’s a little clunky, but it should work. Now the easiest way I can think of for doing the sum is to sum each row list (the lists inside the outer list), write those values to another output list, and then sum those values. We can build it into a slot called `matrixSum` since lists already have a `sum` slot built in (which is quite convenient!)

A little prototype of this in the REPL shows us the basic syntax for achieving this: 

``` io
Io> sumList := list()
==> list()
Io> a foreach(row, sumList push(row sum))
==> list(1, 5)
Io> sumReturn := sumList sum
==> 6
```

That’s what we expect to see. Now we can build that into a method living in a `slot` we will call `matrixSum`. 

``` io
a setSlot("matrixSum", method(
	sumList := list(); 
	self foreach(row, sumList push(row sum));
	sumList sum;
	)
)
```

Testing this out in the REPL, we see that it works! 

Note: I made a little display method for the matrix `a` called `matrixWrite` that displays it in an unflattened format. It uses a very similar structure to what I use here to perform the summation on the matrix (*i.e.,* liberal use of the `foreach` built-in method). 

``` io
Io> a setSlot("matrixSum", method(
... sumList := list();
... self foreach(row, sumList push(row sum));
... sumList sum;
... )
... )
==> method(
    sumList := list; self foreach(row, sumList push(row sum)); sumList sum
)
Io> a matrixWrite // Remember that this is a custom method. 
0  1
2  3
Io> a matrixSum  // and this sums up correctly. 
==> 6

```



---

*4. Add a slot called `myAverage` to a list that computes the average of all the numbers in a list. What happens if there are no numbers in a list? (Bonus: Raise an `Io` exception if any item in the list is not a number.*

This seems pretty straightforward except for the error checking. What does the [`Io` language manual](https://iolanguage.org/guide/guide.html#Syntax-Operators) have to say? 

Looks like we can use a pretty straightforward exception checking system using the `Exception` object and its associated slot/method `raise()`. Nice and easy. 

``` io
List myAverage := method(
	if(self size == 0) then(return(nil)) else(
		s := 0;
		self foreach(i, v,
			if(value type != "Number") then(Exception raise("A non-numeric value was encountered at index #{i}!")) else(s = s + v)
			)
		return(s / (self size));
	)
)

```

I defined a list called `c` and passed the `myAverage` message to it as follows: 

``` io
Io> c
==> list(4, 5, 3, 6, 3)
Io> c myAverage
==> 4.2000000000000002
```

Seems to work correctly! 

---

*5. Write a prototype for a two-dimensional list. The `dim(x, y)` method should allocate a list of `y` lists that are `x` elements long. `set(x, y, value)` should set a value and `get(x, y)` should return that value.*

Three method specifications are needed here. 

1. The `dim(x, y)` method needs to allocate a list of `y` lists of length (size) `x`. 
2. The `set(x, y, value)` method sets the `value` at index `(x, y)`
3. The `get(x, y)` method will return the value at index `(x, y)`

The first thing we should do is look under the hood a little and see what `List` objects have built-in that can help us out. To do that, we fall back on our old friend from Day 1: 

``` io
Io> List slotNames
==> list(second, removeFirst, swapIndices, sortByKey, containsIdenticalTo, pop, containsAny, contains, itemCopy, empty, third, mapFromKey, detect, asMessage, removeSeq, union, rest, reverseReduce, groupBy, removeAll, setSize, asString, appendSeq, indexOf, containsAll, atInsert, justSerialized, with, intersect, at, appendIfAbsent, max, atPut, ListCursor, min, join, foreach, sort, difference, cursor, asMap, first, sum, append, last, myAverage, reverseInPlace, removeAt, asSimpleString, insertBefore, preallocateToSize, flatten, select, selectInPlace, asEncodedList, unique, size, sortInPlaceBy, sortKey, mapInPlace, isNotEmpty, remove, map, slice, copy, push, reduce, sortBy, insertAt, uniqueCount, reverseForeach, sortInPlace, asJson, sliceInPlace, prepend, exSlice, average, insertAfter, removeLast, capacity, reverse, isEmpty, fromEncodedList)

```

Lots of ready-to-use message slots in the `List` object category already, including what looks like a promising memory allocation message: `preallocateToSize`. Having this in place, we can now allocate an arbitrary list of size `x` for each of the `y` lists we need to produce with the `dim(x,y)` message slot we specified above.

Another thing we need to include is a reference to `self` in the methods, which is described [in the `iolang` programming guide](https://iolanguage.org/guide/guide.html). This behaves the same way as other object oriented languages for this purpose, although I think this might be a bit more nuanced in advanced applications of the `self` slot in `Io`. 

Also, remember the syntax we discussed for `for` loops: 

`for(<counter>, <start>, <end>, <optional step>, <do message>)`

So let’s create an arbitrary object called `List2D` and give it a few tricks using the `do()` message at cloning.: 

``` io
// list2d.io
List2D := Object clone do(
	// initialize the object with an empty list.
	init := method(
		data := List clone
	)
	
	// dim(x, y)
	dim := method(x, y, 
		self data preallocateToSize(y)
		for(i, 0, y - 1, data append(list() preallocateToSize(x)))
		self		// return `self` 
	)
)
```

Let’s see if this works. If it compiles and the slot sizes seem appropriate, we can move on to the `set(x,y)` and `get(x,y)` method definitions. 

Let’s add some test calls to the bottom of the file `list2d.io` that I wrote just now and see what it does…

```io
// tests
a := List2D clone
x := 2
y := 3
a dim(x, y)
write("Array Size =", a data size, "\n")
for(i, 0, y-1, write("Array Size at", i, "=", a data at(i) size, "\n"))
```

And, flipping the switch… 

``` shell
$ io list2d.io
Array Size =3
Array Size at0=0
Array Size at1=0
Array Size at2=0
```

… weird.  Does the `size` method for an array return `0` for an empty, but preallocated array? 

``` io
Io> test_list := list()
==> list()
Io> test_list preallocateToSize(10)
Io> test_list size
==> 0
```

<u>Ok, that’s good to know.</u> The dimensions will probably change when we start stuffing values into those arrays, so for now let’s press on. 

Now we need a setter method `set(x,y)` to load the values so we can test our hypothesis. Note we had to dive into the `slotNames` for `List` to find another helpful method `atInsert(<index>, <value>)`: 

``` io
List2D := Object clone do(
 ...
	set := method(x, y, value
		self data at(y) atInsert(x, value)
	)
)
```

Now we need to create a simple getter `get(x,y)` method on `List2D` to be able to access these neat features! 

``` io 
List2D := Object clone do(
...
...
	get := method(x, y, 
		return self data at(y) at(x)
	)
)
```

Adding some test scripts to the bottom of my `Io` file `list2d.io`, we can test the functionality of the prototype we created and see if we got it right. 

``` io
// tests 
...
...
// iteratively load the array with the sum of each pair of indices for now.
for(i, 0, y-1,
    for(j, 0, x - 1, a set(j, i, j+1))
)

// then we'll check the dimensions now that it's all loaded...
write("Array Size =", a data size, "\n")
for(i, 0, y-1, write("Array Size at", i, "=", a data at(i) size, "\n"))

// then we'll spit all the values back out again iteratively.
for(i, 0, y-1,
    for(j, 0, x - 1, write("[",j,",",i,"] = ", a get(j, i),"\n"))
)
```

… Drumroll…

``` shell
$ io list2d.io
Array Size =3
Array Size at0=0
Array Size at1=0
Array Size at2=0
Array Size =3
Array Size at0=2
Array Size at1=2
Array Size at2=2
[0,0] = 1
[1,0] = 2
[0,1] = 1
[1,1] = 2
[0,2] = 1
[1,2] = 2
```

Sick! It actually worked! We also can note here that our hypothesis above about the empty, preallocated list of arbitrary dimension `x` returning `0` when it is passed the `size` message seems to be correct. We know this because once we’ve loaded in our test values, the sizes update to what we would expect. So that’s interesting. 

---

*6. Bonus: Write a transpose method so that (`new_matrix get(y, x)) == matrix get(x, y)` on the original list.*

Sure, let’s give this a whirl. Transposes are cool because they open up a whole bunch of additional matrix operations you can do. I assume there are some built-ins or libraries someplace in `Io` that can do these. 

To start, let’s define the <u> matrix transpose </u>: 

For a given matrix `matrix`, and its transpose `matrix_T`, a correct transposition satisfies: 

`matrix_T[y,x] = matrix[x,y]`

for all x and y. 

``` io
// list2d.io
List2D := Object clone do(
		...
    // matrix transpose
    transpose := method(
        x := self data at(0) size
        y := self data size
        x_T := y
        y_T := x

        matrix_T := self clone
        matrix_T dim(x_T, y_T)

        for(i, 0, y - 1,
            for(j, 0, x - 1,
                value_T := self get(j, i)
                matrix_T set(i, j, value_T)
            )
        )
        return matrix_T
    ) 
 	...
 	)
 
//tests
...
...
 
matrix_T := matrix transpose

"Matrix = " println
matrix printMatrix
"Matrix_T = " println
matrix_T printMatrix
```

This gives us a not at all optimized but straightforward to follow algorithm for creating the transpose by copying the parameters of the original matrix and then using the methods we built out earlier to get and set the values for the transpose. 

I added a dumb little `matrixPrint` method to List2D to show that this works a little more clearly on the command line: 

``` shell
$ io list2d.io
...
...
...
Matrix =
[1 2
1 2
1 2
]
Matrix_T =
[1 1 1
2 2 2
]
```

Sure, the formatting is a little messed up, but you get the idea. I could sink more time into adding some conditionals to the `matrixPrint` method but… meh. 

So that worked! Yay! 

---

*7. Write the matrix to a file and read the matrix from a file.*
Most modern programming languages have inbuilt file I/O methods in their standard libraries, so [checking the `Io` language reference once more](https://iolanguage.org/guide/guide.html#Primitives-Networking) it looks like we can do this without any really fancy footwork, at least. 

``` io
f := File with("file_name.txt")
f openForUpdating
f write("matrix data")
f close
```

Honestly? I tried to make this work with my usual tricks of opening the file and iteratively writing the data as characters into the array like I would have in Python, but I suspect that approach might not be what the `File write()` method was created for.

Strangely, the documentation for the `File write()` command did not make it very clear to me why it was not working. There’s surely some little “gotcha” in there, but I didn’t find it. 

I eventually found a solution that made it work, but it wasn’t my own. [You can find it here - it’s quite clever.](https://www.ybrikman.com/blog/2012/02/04/seven-languages-in-seven-weeks-io-day-2/) I had trouble finding out some of the keywords in the documentation, but pay special note to the `serialized` keyword which is a slot associated with `Sequence` objects. 

Moving on for now. I’ll try to revisit this later. 

---

*8. Write a program that gives you ten tries to guess a random number from 1 to 100. If you would like, give a hit of ‘hotter’ or ‘colder’ after the first guess.*

Main components we need here include: 

1. A number generator. 
3. an interactive loop with command-line I/O. 
4. a loop counter with a limit of ten tries.
5. Values for determining if “hotter” or “colder” is printed out to screen.



``` io
// random_game.io
"Welcome to the Random Game. Guess a number from 1 to 100..." println

targetNumber := 79  // for a test. Cannot sort out library issues yet for Random

guessCount := 0
previousDiff := 0
currentDiff := 0

while(guessCount < 10,
    guess := File standardInput readLine("Enter Guess: ") asNumber
    guessCount = guessCount + 1
    currentDiff = (targetNumber - guess) abs
    if(guessCount == 1) then(previousDiff := currentDiff)

    if(currentDiff == 0) then("You got it!" println;break)
    if(currentDiff > previousDiff) then("Getting Colder..." println);
    if(currentDiff < previousDiff) then("Getting Warmer..." println);
    if(currentDiff == previousDiff) then(
        if(guessCount > 1) then("Same difference!" println) else("..." println))
    previousDiff := currentDiff
    write(10 - guessCount, " guesses remaining... Try again!\n")
)

"OK Bye!" println

```

This listing makes use of the `File` objects in the Standard `Io` library (see the reference documentation for more on this.) 

As an aside, I also tried to use the `Random` library from the `Io` language reference, but for the life of me I could not get it to load properly, even when I installed `Io` on my system from the binary as suggested by Stack Overflow. I also tried to use some of the networking features to grab from an online random number generator, but was foiled again by some unknown issue with the library. If I have more time later I will try to work on it more and see if I can’t figure out the issue. 

Anyway, this listing does work as I have written it and it passes some cursory tests I did using the CLI: 

First, I’ll test a number I *know* is correct: 

```shell
$ io random_game.io
Welcome to the Random Game. Guess a number from 1 to 100...
Enter Guess: 79
You got it!
OK Bye!
```

Then I’ll do a few wrong guesses to make sure my conditional statements work correctly. 

```shell
$ io random_game.io
Welcome to the Random Game. Guess a number from 1 to 100...
Enter Guess: 34
...
9 guesses remaining... Try again!
Enter Guess: 54
Getting Warmer...
8 guesses remaining... Try again!
Enter Guess: 65
Getting Warmer...
7 guesses remaining... Try again!
Enter Guess: 76
Getting Warmer...
6 guesses remaining... Try again!
Enter Guess: 79
You got it!
OK Bye!
```

Sweet.  I could (and probably should) write an automated testing suite. I also should note that I did not implement any special message if you run out of guesses. That’s something for another time, I think. 

---

**Summary:**

So far, the most uncomfortable part of using `Io`, and what makes it most different from other languages I’ve seen so far is the lack of `classes` or templates for defining the objects before they are instantiated. <u>Everything is an Object</u> and has a family tree of parents from which the object is cloned. Even just using methods still requires keeping this in mind. 

The other mental block you might run into with this language is rememebering that everything, and I mean, EVERYTHING is an object or a message. Operators or just special messages. Methods are messages. Objects do stuff only when they are passed a message. Frankly, it can be easy to forget that the underlying machinery works like this when you get into the more C-like syntactic structures of conditionals and loops, but don’t be fooled! 

The metaprogramming possibilities of `Io` look pretty intense as well, especially given the ease with which someone can override operator behavior or create a Domain Specific Language within the rules of `Io`. 

Overall, it feels powerful, if a bit daunting, to be able to essentially “rewrite reality” wihin the program space of an `Io` program. It seems like one could get into *all kinds of trouble* if care isn’t taken to really think about what one is writing when programming with `Io`.  

I’ve been reading a bit about `Io` and how it has a specifically constructed concurrency model that can be (allegedly) quite useful and potent. I am looking forward to seeing what else this programming language has hidden under its sleek surface. See you next time! 

---
