---
title: ALG | Characterizations of Invertible Matrices
draft: false
tags:
  - algebra
---
> Content is on the way :-) ...



> [!hint] Theorem 1
> $A,B,C$ are matrices of the same size, $r,s$ are scalars
> 1. $A+B=B+A$
> 2. $(A+B)+C=A+(B+C)$
> 3. $A+0=A$
> 4. $r(A+B)=rA+rB$
> 5. $(r+s)A=rA+sA$
> 6. $r(sA)=(rs)A$

Matrix multiplication
$$
AB=C,{\rm where}\,\,\, C_{i,j} = \sum_{k=1}^tA_{i,k}\times B_{k,j}
$$
implicit condition: $A$ is $m\times t$ and $B$ is $t\times n$，then $C$ is $m\times n$

> [!hint] More
> connection with vector equations
> $$
> AB=A[\boldsymbol b_1\;\boldsymbol b_2\; ... \boldsymbol b_p] = [A\boldsymbol b_1\;A\boldsymbol b_2\; ... A\boldsymbol b_p]
> $$
> 
> As for computing a row
> $$
> row_i(AB) = row_i(A)\cdot B
> $$
> How about computing a column
> $$
> col_i(AB) = A\cdot col_i(B)
> $$



## Properties of Matrix Multiplication

> [!hint] Theorem 2
> $A$ is $m\times n$
> 1. $A(BC) = (AB)C$
> 2. $A(B +C) = AB+AC$
> 3. $(B+C)A=BA + CA$
> 4. $r(AB) = (rA)B=A(rB), r$ is scalar
> 5. $I_mA=A=AI_n$



## Transpose of a Matrix

> [!hint] Theorem 3
> 1. $(A^T)^T = A$
> 2. $(A+B)^T = A^T + B^T$
> 3. For any scalar $r$, $(rA)^T = rA^T$
> 4. $(AB)^T = B^TA^T$ (==reverse order==)
> 
> 
> 

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





> Content is on the way :-) ...

> [!hint] Invertible
> 
> - only for $n\times n$ matrix
> - if there is an $n\times n$ matrix $C$ such that $AC=I$ and $CA = I$ 
> - $C$ is an **inverse** of $A$ , denoted by $A^{-1}$
> 
> invertible -> nonsingular matrix
> not invertible -> singular matrix

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

$$
E = \begin{bmatrix}1 & 0 & 0 \\ 0 & 1 & 0 \\ -4 & 0 & 1  \end{bmatrix}
$$

Interchange

$$
E = \begin{bmatrix}0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1  \end{bmatrix}
$$

Scaling

$$
E = \begin{bmatrix}1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 5  \end{bmatrix}
$$


> [!important] THEOREM 7
>
>An $n\times n$ matrix $A$ is invertible if and only if $A$ is row equivalent to $I_n$ , and in this case, any sequence of elementary row operations that reduces $A$ to $I_n$ also transforms $I_n$ into $A^{-1}$



An Algorithm for Finding $A^{-1}$

> Content is on the way :-) ...

> [!hint] THROREM 8 The Invertible Matrix Theorem
> 
> Let $A$ be a square $n\times n$ matrix. Then the following statements are equivalent. That is, for a given $A$, the statements are either all true or all false
> 
> 1. $A$ is an invertible matrix.
> 2. $A$ is row equivalent to the $n\times n$ identity matrix.
> 3. $A$ has $n$ pivot positions
> 4. The equation $A{\bf x}={\bf 0}$ has only the trivial solution.
> 5. The columns of $A$ form a linearly independent set.
> 6. The linear transformation ${\bf x} \mapsto A{\bf x}$ is one to one
> 7. The equation $A{\bf x} = {\bf b}$ has at least one solution for each ${\bf b}$ in $\mathbb R^n$
> 8. The columns of $A$ span $\mathbb R^n$
> 9. The linear transformation ${\bf x}\mapsto A{\bf x}$ maps $\mathbb R^n$ onto $\mathbb R^n$
> 10. There is an $n\times n$ matrix $C$ such that $CA = I$
> 11. There is an $n\times n$ matrix $D$ such that $AD = I$
> 12. $A^T$ is an invertible matrix

^588835




## Partitioned Matrices

