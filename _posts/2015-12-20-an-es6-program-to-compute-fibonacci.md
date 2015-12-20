---
title: An ES6 function to compute the nth Fibonacci number
layout: default
tags: [allonge]
---

![FIbonacci Spiral](/assets/images/fibonacci.png)

Once upon a time, people would ask for a [fizzbuzz](http://raganwald.com/2007/01/dont-overthink-fizzbuzz.html "Don't Overthink FizzBuzz") program to weed out the folks who can't string together a few lines of code. We can debate when and how such things are appropriate interview questions, but one thing that is always appropriate is to use them as inspiration for practising our own skills.[^candid]

[^candid]: Actually, let me be candid: I just like programming, and I find it's fun, even if I don't magically transform myself into a 10x programming ninja through putting in 10,000 hours of practice. But practice certainly doesn't hurt.

There are various common problems offered in such a vein, including FizzBuzz itself, computing certain prime numbers, and computing Fibonacci number. [A few years back][2008], I had a go at writing my own Fibonacci function. When I started researching approaches, I discovered an intriguing bit of matrix math, so I learned something while practicing my skills.[^closed]

[2008]: http://raganwald.com/2008/12/12/fibonacci.html

[^closed]: There is a [closed-form solution](http://en.wikipedia.org/wiki/Fibonacci_number#Closed-form_expression) to the function `fib`, but floating point math has some limitations you should be aware of before [using it in an interview](http://raganwald.com/2013/03/26/the-interview.html).

## enter the matrix

One problem with calculating a Fibonacci number is that naïve algorithms require _n_ additions. There are some interesting things we can do to improve on performing _n_ operations.

In this solution, we observe that we can express the Fibonacci number `F(n)` using a 2x2 matrix that is raised to the power of _n_:

    [ 1 1 ] n      [ F(n+1) F(n)   ]
    [ 1 0 ]    =   [ F(n)   F(n-1) ]


On the face of it, raising someting to the power of _n_ turns _n_ additions into _n_ multiplications. _n_ multiplications sounds worse than _n_ additions, however there is a trick about raising something to a power that we can exploit. Let's start by writing some code to multiply matrices:

### multiplying matrices

[Multiplying two matrices](http://www.maths.surrey.ac.uk/explore/emmaspages/option1.html "Matrices and Determinants") is a little interesting if you have never seen it before:

    [ a b ]       [ e f ]   [ ae + bg  af + bh ]
    [ c d ] times [ g h ] = [ ce + dg  cf + dh ]

Our matrices always have diagonal symmetry, so we can simplify the calculation because _c_ is always equal to _b_:

    [ a b ]       [ d e ]   [ ad + be  ae + bf ]
    [ b c ] times [ e f ] = [ bd + ce  be + cf ]

Now we are given that we are multiplying two matrices with diagonal symmetry. Will the result have diagonal symmetry? In other words, will `ae + bf` always be equal to `bd + ce`? Remember that `a = b + c` at all times and `d = e + f` provided that each is a power of `[[1,1], [1,0]]`. Therefore:

    ae + bf = (b + c)e + bf
            = be + ce + bf
    bd + ce = b(e + f) + ce
            = be + bf + ce

That simplifies things for us, we can say:

    [ a b ]       [ d e ]   [ ad + be  ae + bf ]
    [ b c ] times [ e f ] = [ ae + bf  be + cf ]

And thus, we can always work with three elements instead of four. Let's express this as operations on arrays:

    [a, b, c] times [d, e, f] = [ad + be, ae + bf, be + cf]

Which we can code in JavaScript:

{% highlight javascript %}
let times = (...matrices) =>
  matrices.reduce(
    ([a,b,c], [d,e,f]) => [a*d + b*e, a*e + b*f, b*e + c*f]
  );

times([1,1,0]) // => [1, 1, 0]
times([1,1,0], [1,1,0]) // => [2, 1, 1]
times([1,1,0], [1,1,0], [1,1,0]) // => [3, 2, 1]
times([1,1,0], [1,1,0], [1,1,0], [1,1,0]) // => [5, 3, 2]
times([1,1,0], [1,1,0], [1,1,0], [1,1,0], [1,1,0]) // => [8, 5, 3]
{% endhighlight %}

Very interesting.

### exponentiation with matrices

To get exponentiation from multiplication, we could write out a naive implementation that constructs a long array of copies of `[1,1,0]` and then calls `times`:

{% highlight ruby %}
let naive_power = (matrix, n) =>
  times(...new Array(n).fill([1,1,0]));

naive_power([1,1,0], 1) // => [1, 1, 0]
naive_power([1,1,0], 2) // => [2, 1, 1]
naive_power([1,1,0], 3) // => [3, 2, 1]
naive_power([1,1,0], 4) // => [5, 3, 2]
naive_power([1,1,0], 5) // => [8, 5, 3]
{%endhighlight %}

Now let's make an observation: instead of accumulating a product by iterating over the list, let's [Divide and Conquer](http://www.cs.berkeley.edu/~vazirani/algorithms/chap2.pdf). Let's take the easy case: Don't you agree that `times([1,1,0], [1,1,0], [1,1,0], [1,1,0])` is equal to `times(times([1,1,0], [1,1,0]), times([1,1,0], [1,1,0]))`? And that this saves us an operation, since `times([1,1,0], [1,1,0], [1,1,0], [1,1,0])` is implemented as:

{% highlight ruby %}
times([1,1,0],
  times([1,1,0],
    times([1,1,0],[1,1,0]))
{%endhighlight %}

Whereas `times(times([1,1,0], [1,1,0]), times([1,1,0], [1,1,0]))` can be implemented as:

{% highlight ruby %}
let double = times([1,1,0], [1,1,0]),
    quadruple = times(double, double);
{%endhighlight %}

This only requires two operations rather than three. Furthermore, it is recursive. `naive_power([1,1,0], 8)` requires seven operations. However, it can be formulated as:

{% highlight ruby %}
let double = times([1,1,0], [1,1,0]),
    quadruple = times(double, double),
    octuple = times(quadruple, quadruple);
{%endhighlight %}

Now we only need three operations compared to seven. Of course, we left out how to deal with odd numbers. Fixing that also fixes how to deal with even numbers that aren't neat powers of two:

{% highlight ruby %}
let power = (matrix, n) => {
  if (n === 1) return matrix;

  let halves = power(matrix, Math.floor(n / 2));

  return n % 2 === 0
         ? times(halves, halves)
         : times(halves, halves, matrix);
}

power([1,1,0], 1) // => [1, 1, 0]
power([1,1,0], 2) // => [2, 1, 1]
power([1,1,0], 3) // => [3, 2, 1]
power([1,1,0], 4) // => [5, 3, 2]
power([1,1,0], 5) // => [8, 5, 3]
{%endhighlight %}

Now we can perform exponentiation of our matrices, and we take advantage of the symmetry to perform _log2n_ multiplications.

### and thus to fibonacci

Thusly, we can write our complete fibonacci function:

{% highlight ruby %}
let matrixFibonacci = (n) =>
  n < 2
  ? n
  : power([1,1,0], n - 1)[0]

new Array(20).fill(1).map((_, i) => matrixFibonacci(i))
  // => [0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,1597,2584,4181]
{%endhighlight %}

We're done. And this is a win over the typical recursive or even iterative solution for large numbers, because while each operation is more expensive, we only perform _log2n_ operations.[^notfastest]

[^notfastest]: No, this isn't [the fastest implementation](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-talk/194815 "Fast Fibonacci method") by far. But it beats the pants off of a naïve iterative implementation.

(This is a translation of a blog post written in [2008])

---

notes: