+++
date = 2023-01-23T10:00:00Z
lastmod = 2023-01-23T10:00:00Z
author = "default"
title = "Evaluating Square Roots"
subtitle = "Some ideas to calculate square roots without <cmath>"
tags = ["binary search", "Newton-Raphson"]
categories = ["c++", "algorithms", "numeric"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
+++

## The problem statement

There is a [LeetCode problem](https://leetcode.com/problems/sqrtx/) that asks you implement a `sqrt` function that returns the rounded-down nearest integer value. I am not aware, however, of a problem that asks you to simply reproduce the behavior of the standard library `sqrt` function. Interestingly enough, `sqrt` is not marked `constexpr`. Having our own implementation could be helpful if evaluation of square roots is required at **compile time** (for example to reduce runtime in competitive coding 😉).

## First Solution: Binary Search

The first idea is to solve this problem iteratively using a **binary search algorithm**. The lower and upper bounds are initialized with `0.0` and `x`, respectively. We know that `sqrt` must be somewhere within that range. Next, we calculate the *pivot* of that interval as `auto pivot = lower + (upper - lower) / 2`. Note that `auto pivot = (upper + lower) / 2` is mathematically equivalent but might result in an overflow for large `x`. Now we calculate back what would be the `x` if our current guess `pivot` were indeed the square root of `x`. Squaring `pivot` and subtracting the result from `x` gives us a new variable `delta`. We will evaluate if `delta` is a positive or a negative number. If `delta` is positive, we know `pivot` was chosen too small and we will set `lower` to `pivot`. If `delta` is negative, we know `pivot` was chosen too high and we set `upper` to `pivot`. Note that in either case we half the search space. In a next iteration, we will again calculate `pivot` as the middle between `lower` and `upper`, calculate the square of `pivot`, and compare this value to `x`. This iteration will not necessarily converge due to the way computers handle floating point numbers. It is thus important to add a convergence criterium `epsilon`. If the absolute value of `delta` is smaller then `epsilon`, our function has converged and we can return the current value of `pivot` as the result. 

```c++
constexpr double sqrt(double x) {
  // range of binary search
  double lower = 0.0, upper = x, pivot = lower + (upper - lower) / 2;
  double pow = pivot * pivot;
  double delta = x - pow;

  // convergence criterium
  double const epsilon = 1e-12;

  // iterate until convergence criterium is reached
  while (std::fabs(delta) > epsilon) {
    if (delta > 0) { // pivot too small
      lower = pivot;
    }
    else { // pivot too high
      upper = pivot;
    }
    pivot = lower + (upper - lower) / 2;
    pow = pivot * pivot;
    delta = x - pow;
  }

  return pivot;
}
```

## Second Solution: Newton-Raphson

The second approach takes advantage of the [Newton-Raphson method](https://en.wikipedia.org/wiki/Newton%27s_method) to find roots of a function. Like the first solution, it is an iterative method that improves an initial guess $x_{n}$ until a convergence criterium is hit. During each iteration we obtain a new guess $x_{n+1}$:

{{< math >}}
x_{n+1} = x_{n} - \frac{f(x_{n})}{f'(x_{n})}
{{< /math >}}

In our case, $f(x) = x^2 - a$, where `x` is the square root we want to compute and `a` is the input to our `sqrt` function. Accordingly, we have to iteratively evaluate the expression:

{{< math >}}
x_{n+1} = x_{n} - \frac{x_{n}^2 - a}{2x_{n}} = \frac{2x_{n}^2 - (x_{n} ^ 2 - a)}{2x_{n}} = \frac{x_{n}^2 + a}{2x_{n}} = \frac{1}{2} (x_{n} + \frac{a}{x_{n}})
{{< /math >}}

Here is the code:

```c++
constexpr double sqrt(double a) {
  // Newton-Raphson
  double x0 = a;
  double x1 = 0.5 * (x0 + a / x0);
  double const epsilon = 1e-12;

  // iterate until convergence
  while (std::abs(x0 - x1) > epsilon) {
    x0 = x1;
    x1 = 0.5 * (x0 + a / x0);
  }
  return x1;
}
```

## Notes

It is worth pointing out that either approach is very naïve and can be improved substantially. Many smart people have worked on this problem for over 3000 years! Just check your C standard library implemention and some good numerical agorithm books to get some more ideas.
