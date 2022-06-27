# 📚 Project Euler

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
3 + 6 + 9 + 12 + 15 + 18 +...999 = 3 \cdot \left(1 + 2 + 3 + 4 + 5 + ... + 333 \right)
$$

The last value of the sum can be expressed in general as `floor(N/3)` (i.e. floor approximation of `N/3`).

This observation is very useful since it let us compute the term very fast using the formula:

$$
3 \cdot \left( \textit{floor}\left( \frac{N}{3} \right) \cdot \frac{\left( \textit{floor}\left( \frac{N}{3} \right) + 1 \right)}{2} \right)
$$

The output will be the sum of the two components:

$$
\text{output} = 3 \cdot \left( \textit{floor}\left( \frac{N}{3} \right) \cdot \frac{\left( \textit{floor}\left( \frac{N}{3} \right) + 1 \right)}{2} \right) + 5 \cdot \left( \textit{floor}\left( \frac{N}{3} \right) \cdot \frac{\left( \textit{floor}\left( \frac{N}{3} \right) + 1 \right)}{2} \right)
$$

_**!!! WAIT: to avoid double counting we have to subtract those terms that are divisible by 3 and 5 (i.e. by 15)**_

$$
\begin{align*}
\text{FINAL OUTPUT} = & 3 \cdot \left( \textit{floor}\left( \frac{N}{3} \right) \cdot \frac{\left( \textit{floor}\left( \frac{N}{3} \right) + 1 \right)}{2} \right) +
               5 \cdot \left( \textit{floor}\left( \frac{N}{5} \right) \cdot \frac{\left( \textit{floor}\left( \frac{N}{5} \right) + 1 \right)}{2} \right) \\
               & - 15 \cdot \left( \textit{floor}\left( \frac{N}{15} \right) \cdot \frac{\left( \textit{floor}\left( \frac{N}{15} \right) + 1 \right)}{2} \right)
\end{align*}
$$

#### Solution:

The solution is `233168`

`Execution time trivial solution: 1.025525 sec`

`Execution time my solution: 0.01123249 sec`

\




### <mark style="background-color:orange;">**Problem 4**</mark>

`A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is 9009 = 91 × 99. Find the largest palindrome made from the product of two 3-digit numbers.`

In this case I've just brute force by looping over all integers starting from biggers one.

#### Solution:

The solution is `906609`

`Execution time brute force: 0.0337 sec`&#x20;

### <mark style="background-color:orange;">**Problem 6**</mark>

`Find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum.`

This is easy since for both terms there is a analytical form:

$$
\sum_{i=1}^N i =  \frac{N(N+1)}{2}
$$

$$
\sum_{i=1}^N i^2 =  \frac{N(N+1)(2N+1)}{6}
$$

Both formula can be proved by induction.

$$
\sum_{i=1}^N i^2 - \left(\sum_{i=1}^N i\right)^2 =  \frac{N(N+1)(2N+1)}{6} - \left(\frac{N(N+1)}{2} \right)^2
$$

#### Solution:

The solution is `25164150`

### <mark style="background-color:orange;">**Problem 7**</mark>

`What is the 10001st prime number?`

We will solve the problem in two step:

1. Find an integer $$N$$ such
2. Find all prime numbers $$p< N$$ and return $$10001 th$$.

#### First step:

I've used the [_**well knwon estimate**_](https://en.wikipedia.org/wiki/Prime\_number\_theorem):

$$
\pi(x) = \frac{x}{\log(x)}
$$

$$\pi(x)$$ is an estimate for the number of prime numbers smaller than $$x$$.

Let $$\pi^{-1}(y)$$ the inverse function so that:

$$
\pi^{-1}(10001)=116683
$$

is the upper bound we will use for the prime numbers search.

#### Second step:

We can now proceed with the second step by looking or all integers smaller than $$116683$$​.

I've used the algorithm known as [_**Sieve of Eratosthenes**_](https://en.wikipedia.org/wiki/Sieve\_of\_Eratosthenes)_**.**_ It return the list of prime numbers smaller than a certain integer.

Once I 've got the list I can easily retrieve the $$10001 th$$ one.

_**Note:**_ It could be that the initial guess $$116683$$ is an integer not big enough, i.e. that there are less than $$10001$$ prime numbers smaller than that value. It that case it would be enough to increase the upper bound and find all prime numbers smaller than new bound. This approach is not really optimal but $$\pi(x)$$ is an estimate and not an exact value

#### Solution:

The solution is `104759`

`Execution time: 0.639 sec`&#x20;
