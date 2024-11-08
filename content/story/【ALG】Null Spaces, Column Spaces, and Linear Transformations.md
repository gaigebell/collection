---
title: ALG | Null Spaces, Column Spaces, and Linear Transformations
draft: false
tags:
  - algebra
---
### Null Space

When we talk about null space, we're talking in the context of matrix algebra.

So only a matrix have null space.

#### Definition

>[!hint] Null Space
>The **null space** of an $m\times n$ matrix $A$, written as ${\rm Nul\;} A$, is the set of all solutions of the homogeneous equation $A{\bf x} = {\bf 0}$.
>$$
>{\rm Nul\;}A=\{{\bf x}:{\bf x}\in \mathbb R^n\\, and \,A{\bf x} = {\bf 0}\}
>$$

For simplicity, null space is the solution space of equation $A{\bf x} = {\bf 0}$

#### Theorem 2

> [!hint] null space is the solution space to $A{\bf x} = {\bf 0}$
> the set of all solutions is a subspace of $\mathbb R^n$
> 
> (We have $m$ equations and $n$ unknowns)

To think in a dynamic way, ${\rm Nul\,} A$ contains all the ${\bf x}$ that turns $A$ to ${\bf 0}$. Or to say $A$ is a transformation that turns all the ${\bf x} \in {\rm Nul\,} A$ to ${\bf 0}$.

Then $A:{\bf x} \mapsto {\bf 0}$

#### Description

When we solve $A{\bf x}={\bf 0}$ , we'll get every variable in the end.

Some of them are free variables.

We rewrite the solution in terms of these free variables.

Decompose the vector, free variables will become the weights. And their corresponding vectors span ${\rm Nul\,} A$

>[!example] Example
>$$
>\begin{bmatrix}
>x_1 \\ x_2\\ x_3 \\ x_4 \\ x_5
>\end{bmatrix}
>=
>\begin{bmatrix}
>2x_2+x_4-3x_5 \\ x_2 \\ -2x_4+2x_5 \\ x_4 \\ x_5
>\end{bmatrix}
>=
>x_2\underbrace{
>\begin{bmatrix}
>2\\ 1\\ 0\\ 0\\ 0
>\end{bmatrix}
>}_{\bf u}
>+x_4
>\underbrace{
>\begin{bmatrix}
>1\\ 0\\ -2\\ 1\\ 0
>\end{bmatrix}
>}_{\bf v}
>+ x_5
>\underbrace{
>\begin{bmatrix}
>-3\\ 0\\ 2\\ 0\\ 1
>\end{bmatrix}
>}_{\bf w}
>$$
>So the null space is spanned by $\{\bf u,v,w\}$


>[!hint] Points
>1. The spanning set is automatically **linearly independent**
>2. the number of vectors in the spanning set = the number of free variables.


### Column Space

When we talk about column space, we're talking in the context of matrix algebra.

So only a matrix has column space.

#### Definition

>[!hint] Column Space
>The **column space** of an $m\times n$ matrix $A$, written as ${\rm Col\;} A$, is the set of all linear combinations of the columns of $A$.
>$$
>{\rm Col\;}A=\operatorname{Span}\{{\bf a}_1,...,{\bf a}_n\}
>$$

#### Theorem 3

> [!hint] The column space of an $m\times n$ matrix $A$ is a subspace of $\mathbb R^m$

To think in a dynamic way, ${\rm Col\,} A$ is the *range* of the linear transformation ${\bf x}\mapsto A{\bf x}$


>[!hint] The column space of an $m \times n$ matrix $A$ is all of $\mathbb R^m$ if and only if the equation $A{\bf x}={\bf b}$ has a solution for each ${\bf b}$ in $\mathbb R^m$


### Kernel and Range

when we talk about kernel and range, we're talking in the context of linear transformation.

The **kernel** of a transformation $T$ is the null space of $T$'s matrix.

The **range** of a transformation $T$ is the column space of $T$'s matrix.


