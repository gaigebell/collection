---
title: ALG | The Inverse of a Matrix
draft: false
tags:
  - example-tag
---
> Content is on the way :-) ...

Invertible

- only for $n\times n$ matrix
- if there is an $n\times n$ matrix $C$ such that $AC=I$ and $CA = I$ 
- $C$ is an **inverse** of $A$ , denoted by $A^{-1}$

invertible -> nonsingular matrix
not invertible -> singular matrix

Theorem 4

$A =\begin{bmatrix}a & b \\c & d\end{bmatrix}$ . If $ad-bc\neq 0$, then $A$ is invertible and

$$
A^{-1} = \dfrac{1}{ad-bc}\begin{bmatrix}d & -b \\-c & a\end{bmatrix}
$$

determinant of $A$

$$
\det A = |A| = ad-bc
$$

Theorem 5

If $A$ is an invertible $n\times n$ matrix, then for each $\boldsymbol b \in \mathbb R^n$ , the equation $A\boldsymbol x = \boldsymbol b$ has the unique solution $\boldsymbol x = A^{-1}\boldsymbol b$ 



Theorem 6

1. If $A$ is an invertible matrix, then $A^{-1}$ is invertible $(A^{-1})^{-1} = A$
2. If $A$ and $B$ are $n\times n$ invertible matrices, then so is $AB$.$$(AB)^{-1} = B^{-1}A^{-1}$$
3. If $A$ is an invertible matrix, then so is $A^{T}$. $$(A^T)^{-1}= (A^{-1})^T$$



Elemantary Matrix

represents a row operation on identity matrix

Replacement

$$E = \begin{bmatrix}1 & 0 & 0 \\
0 & 1 & 0 \\
-4 & 0 & 1 
\end{bmatrix}$$

Interchange

$$E = \begin{bmatrix}0 & 1 & 0 \\
1 & 0 & 0 \\
0 & 0 & 1 
\end{bmatrix}$$

Scaling

$$E = \begin{bmatrix}1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 5 
\end{bmatrix}$$

[!important] THEOREM 7

An $n\times n$ matrix $A$ is invertible if and only if $A$ is row equivalent to $I_n$ , and in this case, any sequence of elementary row operations that reduces $A$ to $I_n$ also transforms $I_n$ into $A^{-1}$



An Algorithm for Finding $A^{-1}$

