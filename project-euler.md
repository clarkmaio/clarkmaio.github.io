# Project Euler

**Project Euler** is [this](https://projecteuler.net/). It is a collection of problems (mostly related to algebra and algorithms).

Once you have solved the problem you can submit the solution to verify if it is correct. Here I will present the solution I've found to the problems I've solved.

I will try to give not only the solution but also the _running time_ of the scripts that solve the problem.

You can find the code [here](https://github.com/clarkmaio/ProjectEuler).

Have fun : )



### <mark style="background-color:orange;">**Problem**</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">1</mark>

`If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23. Find the sum of all the multiples of 3 or 5 below 1000.`

This is my idea: we will decompose the final result into the contribution of numbers divisible by 3 and 5.

For example the sum of all numbers divisible by 3 smaller than 1000 is:

$$
f(x) = x * e^{2 pi i \xi x}
$$

The last value of the sum can be expressed in general as `floor(N/3)` (i.e. floor approximation of `N/3`).

This observation is very useful since it let us compute the term very fast using the formula:

$$
f(x) = x * e^{2 pi i \xi x}
$$

The output will be the sum of the two components:

$$
f(x) = x * e^{2 pi i \xi x}
$$

_**!!! WAIT: to avoid double counting we have to subtract those terms that are divisible by 3 and 5 (i.e. by 15)**_

$$
F
$$

#### Solution:

The final solution is: `233168`

`Time performance of trivial solution: 1.025525 sec`

`Time performance of my solution: 0.01123249 sec`

\




### <mark style="background-color:orange;">**Problem 2**</mark>

### <mark style="background-color:orange;">**Problem 4**</mark>

### <mark style="background-color:orange;">**Problem 5**</mark>

### <mark style="background-color:orange;">**Problem 7**</mark>

### <mark style="background-color:orange;">**Problem 8**</mark>
