---
title: The Algebra of Independent Nonnegative Integer Random Variables (IntRV)
date: 2022-03-25 20:32:13
categories:
- Science
tags:
- Math
- Note
---

I introduce a algebra for manipulating independent nonnegative integer random variables, which I
call "IntRV". These random variables are of particular interest in discrete mathematics and computer
science. In fact, the reason why I use this abbrevation "IntRV" is for the convenience of programming
(for example implementing a class under the name "IntRV"). I will give an example where this algebra
exhibits great power.

<!-- more -->

# Definition

Despite the name "algebra", what I introduce in the following do not exactly meet the definition
of algebra in mathematics.

We are working with independent nonnegative integer random variables, or IntRV, over $\mathbb{R}_+$.
As the name suggests, IntRV are random variables that can take the value of a nonnegative integer.
Moreover, every two IntRV should be independent. The trivial cases are constant IntRV, which includes
$0,1,2,\dots$.

There are many operators in IntRV's algebra. I categorize all of them by the number of operants.
In the following, $a,b,c,\ldots$ represents nonnegative real numbers, $p,q$ represents real number
between 0 and 1, $X,Y,Z,\dots$ represents IntRV.

- Unary operators:
  - Scalar multiplication: $cX$
  - Mixing with 0: $p\rightarrow X$
  - Maximum power: $\max^nX$
  - Power: $X^n$
- Binary operators:
  - Add: $X+Y$
  - Mutiplication: $X\cdotp Y$
  - Mixing: $X\oplus Y$
  - Maximum: $\max(X,Y)$

We detail them one by one.

## Scalar multiplication

