---
title: ALG | Vector Spaces and Subspaces
draft: false
tags:
  - algebra
---

### Vectors can be... anything!

#### Definition

> [!important] Vector space and vectors
> 
> vector space $V$ (nonempty set)
> 
> objects: vectors
> 
> operations: vector addition, multiplication by scalars
> 
> The axioms must hold for all vectors
> 
> 1. $\bf u + \bf v$ is in $V$
> 2. $\bf u + \bf v = \bf v + \bf u$
> 3. $(\bf u + \bf v) + \bf w = \bf u + (\bf v + \bf w)$
> 4. There is a **zero** vector $\bf 0$ in $V$ such that $\bf u + \bf 0 = \bf u$
> 5. $-\bf u$ exists such that $\bf u + (-\bf u) = \bf 0$
> 6. $c\bf u$ is in $V$
> 7. $c({\bf u} + {\bf v}) = c{\bf u} + c{\bf v}$
> 8. $(c+d){\bf u}= c{\bf u} + d{\bf u}$
> 9. $c(d{\bf u}) = (cd){\bf u}$
> 10. $1{\bf u} = \bf u$
> 
> > [!faq] Is $\bf 0$ and $-\bf u$ unique?
> > Yes. Can be prooved by using axioms above.


### Representation

#### Geometric vectors

vectors can be regarded as arrows(directed line segments) in geometric space.

#### Signals (discrete-time)

Let's imagine we have a machine producing a signal at every moment.

denote moment as $t$, and signal as $y$. 

Then a series signals is $\{y_t\} = (...,y_{-2},y_{-1},y_{0},y_{1},y_{2},...)$. Signals are doubly infinite sequences of numbers.

If we regard signals as vectors, we can check whether signals satisfy the axioms above.

Imagine a signal $\{y_t\}$ and another signal $\{z_t\}$ .

- $\{y_t\} + \{z_t\} = \{y_t + z_t\}$ holds.

- $c\{y_t\} = \{cy_t\}$ holds.

Therefore, we can primarily assert that signals can be regarded as vectors. In fact, all the axioms can by verified in space of signals (we denote it as $\mathbb S$).

#### Polynomials

An $n$-degree polynomial ${\bf p}(x)$ is
$$
{\bf p}(x) = a_0 + a_1x+a_2x^2+\cdots  + a_n x^n
$$

A set of $n$-degree polynomial, let's denote it as ${\mathbb P}_n$ , is also a vector space to some extent.

What are enternal are that $x,x^2,...,x^n$ . They are fixed.

So the coefficients are varied. $(a_0,a_1,a_2,...,a_n)$ . (Here's a tip: $n$-degree polynomials' coefficients vectors is in $\mathbb R^{n + 1}$)

Imagine ${\bf p}(x)$ and ${\bf q}(x)$ are $3$-degree polynomial. And coefficients of ${\bf q}$ is denoted as $b_0,b_1,...$

- ${\bf p}(x) + {\bf q}(x) = (a_0 + b_0) + (a_1 + b_1)x + (a_2+b_2)x^2 + (a_3+b_3)x^3 = {\bf p + q}(x)$ holds.

- $c{\bf p}(x) = (c{\bf p})(x)$ holds.

It's very likely to be a vector space!

#### More generally... some real-valued functions

Since polynomials can be regarded as vectors, is there any more general case of functions can be regarded as vectors?

Think about a function $\bf f$ :

$$
{\bf f}(x) = 4+\sin x+2\ln x
$$

And another function ${\bf g}$ :
$$
{\bf g}(x) = 3 + x
$$

> [!faq] What is unchanged?
> $(x,\sin x,\ln x)$

>[!faq] What are the coefficients
>$(a_0,a_1,a_2,a_3)$
>
>${\bf f} = (4,0,1,2)$
>
>${\bf g} = (3,1,0,0)$

- ${\bf f}(x) + {\bf g}(x) = {\bf f + g}(x)$ holds.
- $c{\bf f}(x) = (c{\bf f})(x)$ holds.

Wouldn't you agree that it's very likely to be a vector space?!

Imagine a set $F$ of all the functions like below:

$${\bf f}(x) = a_0 + a_1\cdot f_1(x) + a_2\cdot f_2(x) + \cdots + a_n\cdot f_n(x)$$

$F$ is a vector space.

> [!warning] What shall we be careful of?
> $f_i(x)$ is only with respect to $x$. 

### Subspaces

#### Definiton

>[!important] Subspace
>A subspace of a vector space $V$ is a subset $H$ of $V$ that has 3 properties:
>
>1. The zero vector of $V$ is in $H$
>2. $H$ is closed under vector addition.
>3. $H$ is closed under mutilplication by scalars.
>
>> [!note] In fact...
>> Property 3 $\Rightarrow$ property 1.
>> 
>> $0{\bf u} = \bf 0$
>
>>[!danger] Attention!
>>Can we say $\mathbb R^2$ is a subspace of $\mathbb R^3$ ?
>>
>>No.
>>
>>Because it's obvious that $\begin{bmatrix}0\\0 \end{bmatrix} \neq \begin{bmatrix}0 \\ 0 \\ 0\end{bmatrix}$
>>
>>We can say
>>$$
>>H = \left\{ \begin{bmatrix} s \\ t \\ 0\end{bmatrix} :s,t\in {\mathbb R} \right\}
>>$$
>>is a subspace of $\mathbb R^3$


In a geometric perspective, subspaces are always through the origin of a vector space.


### A Subspace Spanned by a Set

In this part, we'll learn how to express a subspace by using notations of span.

#### Theorem 1

>[!hint] vectors span a subspace
>If ${\bf v}_1,...,{\bf v}_p$ are in a vector space $V$, then $\operatorname{Span}\{{\bf v}_1,...,{\bf v}_p\}$ is a subspace of $V$.

So if a subspace is $\operatorname{Span}\{{\bf v}_1,...,{\bf v}_p\}$ ,then for every ${\bf u}$ in the subspace we have

$$
{\bf u} = \sum_{i=1}^pc_i{\bf v}_i
$$

that is ${\bf u}$ is a linear combination of $\{{\bf v}_1,...,{\bf v}_p\}$


