---
title: "A Sequence Problem"
layout: default
tags: [allonge]
---

Here are the first sixteen elements of a sequence:

```
.
*
(*)
(*.)
((*))
(*..)
(**)
(*...)
((*.))
((*).)
(*.*)
(*....)
(*(*))
(*.....)
(*..*)
(**.)
```

And the next sixteen:

```
(((*)))
(*......)
((*.)*)
(*.......)
(*.(*))
(*.*.)
(*...*)
(*........)
(*(*.))
((*)..)
(*....*)
((*.).)
(*..(*))
(*.........)
(***)
(*..........)
```

What is the next element in the sequence?

### solving sequence problems

Problems with this rough form appear regularly in "intelligence" tests. To solve them, you need a couple of different things:

First, you need some facility with manipulating abstract relationships and patterns. You need a modicum of deductive and inductive logical thought. This is probably what people are talking about when they talk about using a problem like this to measure "intelligence." It's not the only kind of intelligence, of course, but it certainly is one or more kinds of intelligence.

But intelligence (whether intelligence-singular or multiple types of specialized intelligence) is not the only thing you need to figure this out. Intelligence is *necessary but not sufficient*. You also need experience with tools.

### tooling

What kinds of tools are we talking about?

Imagine we try to prove the Pythagorean Theorem from scratch, with no mathematical training. This requires intelligence, obviously, but there is something else: It is much harder to prove a geometry theorem from first principles, than if we've already had some exposure to plane geometry and can use the notation and methods taught in school.

The fact is, tools matter for solving problems, and experience with the tools greatly influences your ability to solve problems in a particular domain.

Solving sequence problems requires intelligence, yes, but it is greatly assisted by experience solving sequence problems and exposure to the tools for solving sequence problems.

One tool is to make a hypothesis about how the sequence is constructed, test it against the examples given, and if it fits, derive the next value from your hypothetical rules for constructing the sequence.

The more experience you have with sequence problems, the more hypotheses you are likely to consider and the faster you will generate and test them.

One hypothesis to try is that this is a sequence where there is a fixed transformation on each element to derive the next element. If that was the case, we would look for `f` where:

```
f. -> *
f* -> (*)
f(*) -> (*.)

...

f(***) -> (*..........)
```

If we solve for `f`, we can then apply `f(*..........)` and we'd have our answer.

The recognition that sequences have patterns like "repeated application of a function" is a powerful insight. You could, of course, make a tremendous leap unassisted and work this out. Or, you could have been exposed to the idea in a book or in school, in which case you are demonstrating your experience with the tools of mathematical thinking.

But of course, these are two different things. And if you want to measure one, you may wind up accidentally measuring the other. This is the main problem with "brain teasers" as programming interview questions. We often want to measure "smarts," but we instead measure "experience with abstract problems."

The argument about whether the ability to solve sequence problems applies to the ability to write software, often comes down to the difference between the raw intelligence, which very well may to apply to programming, and the experience with specific math tools, which may not.[^more]

[^more]: There's another conjecture that organic exposure to mathematics is strongly correlated with programming ability. The archetype from my generation is the nerd who subscribed to Scientific American just for Martin Gardner's "Mathematical Recreations" column, and who reads Raymond Smullyan for fun. This may or may not be a reasonable conjecture, but modern thought is that while it may have some positive signal, it has many false negatives. Another, even more glaring flaw is that when there are financial incentives for pretending to have organic exposure to mathematics, people will fake this by purchasing entire books devoted to learning how to solve math problems, just to pass job interviews. In which case, you are testing someone's ability to cram for exams, which is not the same thing at all, and may end up excluding someone who chose to read about combinatorial logic instead of solving sequence problems.

But back to our sequence. You can stop here if you haven't solved the problem and care to work on it yourself.

---

[![Numbers (c) 2012 Morebyless](/assets/images/numbers.jpg)](https://www.flickr.com/photos/morebyless/9423385629)

### the sequence

Another general form for sequences is that they are a mapping from some well-known sequence to another. The sequence above could be a code or representation for the words of the American Declaration of Independence. Or Pantone colours. Or more likely, some well-known sequence of numbers.

In that case, the sequence above could be something like:

```
f0 -> .
f1 -> *
f2 -> (*)
f3 -> (*.)

...

f31 -> (*..........)
```

If that was the case, the sequence would be a representation of the Natural Numbers (also called the non-negative integers), in order, from `0` through `15` in the first list, and `16` through `31` in the second list. If we knew that this was a list of the natural numbers, we would know that the next number is going to be `32`, and if we know `f`, we can apply `f32 ->` and derive the next item in the list.

How can we verify our hypothesis? Well, the natural numbers have some patterns, and we could see if the sequence we have has similar patterns. For example, do all the even or odd items have something in common?

To make things easier, let's play with the sequence in JavaScript. Here's some code that "prints" each element along with our hypothetical relationship:

```javascript
const s = ['.', '*', '(*)', '(*.)', '((*))', '(*..)',
  '(**)', '(*...)', '((*.))', '((*).)', '(*.*)', '(*....)',
  '(*(*))', '(*.....)', '(*..*)', '(**.)', '(((*)))',
  '(*......)', '((*)*)', '(*.......)', '(*.(*))',
  '(*.*.)', '(*...*)', '(*........)', '(*(*.))', '((*)..)',
  '(*....*)', '((*.).)', '(*..(*))', '(*.........)',
  '(***)', '(*..........)'];

for (let i = 0; i < s.length; i = i + 1)
  console.log(`f${i} -> `+ s[i]);

f0 -> .
f1 -> *
f2 -> (*)
f3 -> (*.)
f4 -> ((*))
f5 -> (*..)
f6 -> (**)
f7 -> (*...)
f8 -> ((*.))
f9 -> ((*).)
f10 -> (*.*)
f11 -> (*....)
f12 -> (*(*))
f13 -> (*.....)
f14 -> (*..*)
f15 -> (**.)
f16 -> (((*)))
f17 -> (*......)
f18 -> ((*)*)
f19 -> (*.......)
f20 -> (*.(*))
f21 -> (*.*.)
f22 -> (*...*)
f23 -> (*........)
f24 -> (*(*.))
f25 -> ((*)..)
f26 -> (*....*)
f27 -> ((*.).)
f28 -> (*..(*))
f29 -> (*.........)
f30 -> (***)
f31 -> (*..........)
```

If you haven't solved it yet, feel free to stop here and take advantage of these two tools: The hypothesis that this is a mapping from the natural numbers to some representation, and a snippet of JavaScript that facilitates playing with the elements of the sequence.

---

[![Sequence #1073 © 2010 fdecomite](/assets/images/cubes.jpg)](https://www.flickr.com/photos/fdecomite/5174624496)

### some observations

Shall we continue? One thing we can do is look at the even elements:

```javascript
for (let i = 0; i < s.length; i = i + 2)
  console.log(`f${i} -> `+ s[i]);

f0 -> .
f2 -> (*)
f4 -> ((*))
f6 -> (**)
f8 -> ((*.))
f10 -> (*.*)
f12 -> (*(*))
f14 -> (*..*)
f16 -> (((*)))
f18 -> ((*)*)
f20 -> (*.(*))
f22 -> (*...*)
f24 -> (*(*.))
f26 -> (*....*)
f28 -> (*..(*))
f30 -> (***)
```

And the odd elements:

```javascript
for (let i = 1; i < s.length; i = i + 2)
  console.log(`f${i} -> `+ s[i]);

f3 -> (*.)
f5 -> (*..)
f7 -> (*...)
f9 -> ((*).)
f11 -> (*....)
f13 -> (*.....)
f15 -> (**.)
f17 -> (*......)
f19 -> (*.......)
f21 -> (*.*.)
f23 -> (*........)
f25 -> ((*)..)
f27 -> ((*.).)
f29 -> (*.........)
f31 -> (*..........)
```

Interesting. The first odd is `*`, which we hypothesize is `1`. All subsequent odds end in `.)`, while none of the evens end in `.)`. A bunch of the odds are even "more extreme," they start with `(*`, then have one or more dots ending in `.)`.

We can express that with a regular expression. Annoyingly, we have to escape everything because this sequence consists entirely of characters that have a special meaning in regular expressions: `/^\(\*\.+\)$/`.

Let's use it:

```javascript
for (let i = 0; i < s.length; i = i + 1)
  if (s[i].match(/^\(\*\.+\)$/))
    console.log(`f${i} -> `+ s[i]);

f3 -> (*.)
f5 -> (*..)
f7 -> (*...)
f11 -> (*....)
f13 -> (*.....)
f17 -> (*......)
f19 -> (*.......)
f23 -> (*........)
f29 -> (*.........)
f31 -> (*..........)
```

This sequence looks very familiar, but it's missing something. Where is `f2`? If we modify our regular expression to match zero or more dots instead of one or more, we get:

```javascript
for (let i = 0; i < s.length; i = i + 1)
  if (s[i].match(/^\(\*\.*\)$/))
    console.log(`f${i} -> `+ s[i]);

f2 -> (*)
f3 -> (*.)
f5 -> (*..)
f7 -> (*...)
f11 -> (*....)
f13 -> (*.....)
f17 -> (*......)
f19 -> (*.......)
f23 -> (*........)
f29 -> (*.........)
f31 -> (*..........)
```

Aha! These are prime numbers. `(*)` is the first prime, `(*.)` is the second, `(*..)` is the third, and so forth, up to `(*..........)` being the eleventh prime. If our hypothesis is correct, `.` is a zero and `*` is a one.

Our special "exception"--two--fits, it's an exception in number theory as well: Two is the only even prime number. This discovery is encouraging, let's observe something else:

Each prime is indicated by a one in its position and a zero in the previous positions. We know something like this. Our standard numerical notation (e.g. base ten) uses positions. A number in a particular position indicates how much to multiply the position's value, and we sum all the values together.

Maybe this uses the same method, but with primes instead of powers of a base like ten? Let's check it out. If that were the case, then given `(*.)` for three and `(*)` for two, we would expect `(**)` to be five ("three plus two"). But no, it's six. Which is three *times* two.

Let's try another. `(*...)` is seven, and `(*.)` is three. `(*.*.)` is 21, seven times three, not eleven. It looks like this is a multiplicative scheme. And if we check all the numbers that don't have nested parentheses, that's exactly what we have.

This all works out, even `(***)` for thirty (five times three times two). But what about nested parentheses? Well, four appears to be `((*))` if our hypothesis is correct. That would be two times two, and there's no way to derive four from multiple primes.

So `((*))` must me some way of multiplying two by itself. We know that, it's two to the power of two. If we stare at it a bit, we see that one is `*`, and two is `(*)`, so that's a little like saying that two is `(1)` So if we look at `((1))`, we can take the inner `(1)` and turn it into two: `(2)` which is like saying two times two. So when we have nested parentheses, we are substituting a parenthesized expression anywhere a dot or asterisk could go.

This explains nine (`((*).)`) and twenty-five (`(*)..`). But how about eight? That's `((*.))`, which is like `(3)`. Obviously we can't say that `(3)` means multiplying two by three. It must mean *raising two to the third power*.

And now the whole thing is bare.

### prime factorization

This notation expresses the numbers zero and one as special cases. Everything larger uses the parentheses to represent numbers as their prime factorization. For example, twenty-eight is seven to the power of one times two to the power of two (`7ꜛ1 ⨉ 2ꜛ2`). Seven is `(*...)`, two is `(*)`, and thus twenty-eight is `(*..(*))`.

[prime factorization]: https://en.wikipedia.org/wiki/Table_of_prime_factors

Each position is the exponent for that prime, also called its *multiplicity*. It looks a little weird because everything is smooshed together, but if we use Lisp's s-exprs, it's easier to see how it works when an exponent is itself an expression:

`f28 -> (* . . (*))`

This representation is recursive, so if `(*..(*))` is 28, then `((*..(*)).)` is `3ꜛ28`, or 22,876,792,454,961. Likewise, consider:

```
f2 -> (*)
f4 -> ((*))
f16 -> (((*)))
f65536 -> ((((*))))

...
```

This is `2ꜛ1`, `2ꜛ2ꜛ1`, `2ꜛ2ꜛ2ꜛ1`, `2ꜛ2ꜛ2ꜛ2ꜛ1` and so forth ad infinitum.

Many of the numeric properties derived from factorizing numbers are obvious from direct inspection of this notation. For example:

- [Prime numbers](https://en.wikipedia.org/wiki/Prime_number) have just one prime factor raised to the power of one, thus they all have the form `(*` followed by zero or more `.`s followed by `)`.
- [Composite numbers](https://en.wikipedia.org/wiki/Composite_number) have any other form beginning with `(` and ending with `)`.
- [Odd numbers](https://en.wikipedia.org/wiki/Odd_number) do not have two as a factor, so they must end with `.)`.
- [Even numbers](https://en.wikipedia.org/wiki/Even_number) have two as a factor, so they must end with `*)` or `))`.
- A [semiprime](https://en.wikipedia.org/wiki/Semiprime) is a compound number consisting of two primes multiplied by each other or one prime squared. Thus, it is either:
  1. `((*)` followed by zero or more `.`s, followed by `)`, or;
  2. `(*`, followed by zero or more `.`s, followed by `*`, followed by zero or more `.`s, followed by `)`
- A [square number](https://en.wikipedia.org/wiki/Square_number) has even multiplicity for all prime factors. `*` is a square, and also every number of the form `()` where each position is either a `.` or a parenthesized representation of an even number (see above).
- A [powerful number](https://en.wikipedia.org/wiki/Powerful_number) has multiplicity above one for every prime factor, therefore a powerful number is represented as a `(`, followed by either `.`s or parenthesized expressions, followed by `)`.
- A [square-free number](https://en.wikipedia.org/wiki/Square-free_integer) is represented as a `(*`, followed by zero or more `.`s or `*`s, followed by `)`.
- A [prime power](https://en.wikipedia.org/wiki/Prime_power) is represented as a `(`, either a `*` or a parenthesized expression, followed by zero or more `.`s, followed by `)`.

There are many more. What they all have in common is that they can be determined with fairly simple pattern matching from this representation.

### closing thought

Representations are celebrated for what they make easy. As we saw above, this notation makes all sorts of questions based on factorization easy. And it is much more compact than our base-n representation, the built-in exponentiation scales, well, exponentially.

However, to be useful as a general-purpose representation, it would have to be easy to work with for routine tasks like addition and subtraction. And while converting from this representation seems straightforward, requiring only multiplication and exponentiation, converting to this representation is one of the hardest problems in number theory!

A fun exercise is to write a program to generate the sequence. A FizzBuzz with a side of curiosity, so to speak. 

(discuss on [hacker news](https://news.ycombinator.com/item?id=14482561))

---

[![Children at Hiawatha Playfield, 1912 Item 29278, Don Sherwood Parks History Collection (Record Series 5801-01), Seattle Municipal Archives.](/assets/images/teeter-totter.jpg)](https://www.flickr.com/photos/seattlemunicipalarchives/8450857233)

### author's afterword

Earlier in this essay, I touched on the problem with using questions like this to test "intelligence." The crux of the argument was that besides testing for intelligence, it also tests for exposure to the tooling.

The conclusion is that we really shouldn't draw conclusions about someone's intelligence, much less fitness for programming, from their ability to solve a problem like this in the context of a job interview.

A similar dynamic is in play when we compare someone who has seen the problem to someone who hasn't. If Alice is posing the problem to Bob, Alice can easily appear to be smarter than Bob!

It seems silly when I write it out like this, obviously Alice posing the problem to Bob doesn't mean Alice is smarter or more capable than Bob. But all too often, this is the exact dynamic in job interviews. It's easy for interviewers to arrogantly fixate on an interviewee's struggle as evidence of them being unable to come up with the "obvious" solution.

And the interviewee can contract a bad case of intimidation, feeling they are not smart enough or good enough to work in a place full of smart people who do nothing but solve math problems for fun.

This generalizes to all interview problems, whether mathematical or not. Never assume that struggling with a problem implies that the interviewee must not be as smart as the interviewer, who has the advantage of having studied the problem at leisure.

And while you're thinking about that, ask yourself this question: If Donovan writes a blog post about math, or programming, or anything at all, and Carol finds it unfamiliar, should she presume that Donovan is smarter and more experienced than she is?

No, for the same reasons. Writing a blog post is evidence that Donovan carefully selected something he felt he knew, and then spent an undetermined amount of time writing, researching and polishing his words.

Carol, reading it extemporaneously, should not worry that she is in any way less intelligent or even less experienced than Donovan. Writing a blog post is a scenario where the author picks the problem and the tools necessary to solve the problem.

It is not evidence of anything other than an enthusiasm for sharing, and I encourage everyone to enjoy blog posts in that spirit. Please do not worry that you may be less gifted or unworthy in any way.

---

### an example program to generate the sequence without using integers or arrays

<script src="https://gist.github.com/raganwald/4c16884cfc194e11ea121a89da3419d4.js"></script>

---

### notes
