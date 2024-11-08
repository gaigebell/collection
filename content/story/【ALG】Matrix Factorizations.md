---
title: ALG | Matrix Factorizations
draft: false
tags:
  - algebra
---


Given a matrix $A$ , find the echelon form of $A$ .

This is what we've done in previous chapter. 

Now we find that the process of transforming $A$ into its echelon form is a series of row operations. Here's the question: is there a matrix representing the operations?

We are seeking for $A=LU$. Here $U$ is the echelon form of $A$, and $L$ is what we are looking for.

Here's a fun fact: $L$ is always a lower triangular matrix.

So, here's the case:

$$
A = \begin{bmatrix}1 &0 &0 &0 \\ * & 1 & 0 & 0 \\ * &* & 1 & 0 \\ * & * & * & 1\end{bmatrix}\begin{bmatrix}\square &* &* &* &* \\ 0 & \square & * & * & * \\ 0 &0 & 0 & \square &* \\ 0 & 0 & 0 & 0 & 0\end{bmatrix}
$$

An obvious advantage of $LU$ factorization is that: $Ax=b\Rightarrow L^{-1}Ax=(L^{-1})b \Rightarrow Ux = (L^{-1})b$ 

Solving linear systems can be transformed into matrix operations. Previously, we can only perform matrix operations on invertible $A$. But now, we can solve all $Ax=b$.

> [!note] Algorithm for an LU factorization
> 1. Reduce $A$ to an echelon form $U$ by a sequence of row replacement operations, if possible
> 2. Place entries in $L$ such that the same sequence of row operations reduce $L$ to $I$
> 
> > [!example] Example
> > $$
> > \begin{aligned}
> > A = \begin{bmatrix}\color{red}2&4&-1&5&-2\\\color{red}-4&-5&3&-8&1\\\color{red}2&-5&-4&1&8\\\color{red}-6&0&7&-3&1\end{bmatrix}
> > \sim\begin{bmatrix}2&4&-1&5&-2\\0&\color{red}3&1&2&-3\\0&\color{red}-9&-3&-4&10\\0&\color{red}12&4&12&-5\end{bmatrix}\\[2ex]\sim\begin{bmatrix}2&4&-1&5&-2\\0&3&1&2&-3\\0&0&0&\color{red} 2&1\\0&0&0&\color{red}4&7\end{bmatrix}
> > \sim\begin{bmatrix}2&4&-1&5&-2\\0&3&1&2&-3\\0&0&0&2&1\\0&0&0&0&\color{red}5\end{bmatrix} = U
> > \end{aligned}
> > $$
> > $$
> > \begin{aligned}
> > &\begin{bmatrix}2\\-4\\2\\-6\end{bmatrix}\begin{bmatrix}3\\-9\\12\end{bmatrix}\begin{bmatrix}2\\4\end{bmatrix}\begin{bmatrix}5\end{bmatrix}
> > \\[2ex]
> > &\begin{bmatrix}
> > 1&0&0&0\\-2&1&0&0\\1&-3&1&0\\-3&4&2&1
> > \end{bmatrix}=L
> > \end{aligned}
> > $$





