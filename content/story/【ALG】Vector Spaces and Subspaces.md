---
title: ALG | Rank
draft: false
tags:
  - algebra
  - example-tag
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


##### Theorem 9

> [!hint] linearly independent set has maximum size
> If a  vector space $V$ has a basis ${\cal B}$ , then any set in $V$ containing more than $n$ vectors must be linearly dependent.


##### Theorem 10

> [!hint] every basis of $V$ has the same size
> If a vector space $V$ has a basis of $n$ vectors, then every basis of $V$ must consist of exactly $n$ vectors.

##### Definition

> [!hint] dimension
> If $V$ is spanned by a finite set, then $V$ is said to be **finite-dimensional**
> 
> The **dimension** of $V$, written as ${\rm dim} V$, is the number of vectors in a basis for $V$. 
> 
> The dimension of the zero vector space $\{{\mathbf 0}\}$ is defined to be zero. 
> 
> If $V$ is not spanned by a finite set, then $V$ is said to be **infinite-dimensional**


#### Subspaces of a Finite-Dimensional Space

##### Theorem 11

>[!hint] dimension of a subspace is always less than the space 
>$H\subset V$, then
>${\rm dim} H \leqslant {\rm dim} V$
>


##### Theorem 12

>[!hint] The Basis Theorem
>
>$V$ is a $p$ dimensional vector space
>
>- Any **linearly independent** set with size of $p$ is a basis for $V$
>- Any set with size of $p$ that spans $V$ is a basis for $V$


#### The Dimensions of ${\rm Nul}A$ and ${\rm Col} A$

> [!hint ] 
> - The dimension of ${\rm Nul\;} A$ = $n($free variables$)$
> - The dimension of ${\rm Col\;} A$ = $n($pivot columns$)$


Wouldn't you agree with me that almost everything has its basis.

I mean skeleton, framework and basic elements.

They arrange and combine in some way then construct a large bunch of things.

Just like dots produce lines, lines produce shapes and planes, planes produce cubes.

And now, we're looking into vector spaces. Intuition tells us that a vector space is actually made up by simple vectors. 

##### Theorem 4

> [!hint] A way to tell a linearly independent set
> Imagine a set of vectors $\{{\bf v}_1,...,{\bf v}_p\}$ ($p\geqslant 2,{\bf v}_1 \neq {\bf 0}$)
> 
> It is **linealy independent** *if and only if*
> 
> some ${\bf v}_j(j > 1)$ is a *linear combination* of the preceeding vectors, ${\bf v}_1,...,{\bf v}_{j-1}$



##### Definition

> [!hint] Basis
> Imagine $H$ is a subspace of a vector space $V$.
> 
> A set of vectors ${\cal B} = \{{\bf b}_1,...,{\bf b}_p\}$ in $V$ is a **basis** for $H$ if
> 1. ${\cal B}$ is a *linearly independent set*
> 2. $H = \operatorname{Span}\{{\bf b}_1,...,{\bf b}_p\}$

> [!faq] Can you come up with a basis for $\mathbb{R}^n$ ?
> An easy way to do so is to imagine a Cartesian coordinate.
> 
> ![[Pasted image 20241202112936.png]]
> 
> Then you'll find arbitrary vectors on $x,y,z$ axis respectively made up a basis for the space. 

> [!danger] standard basis
>  
> $$
> {\boldsymbol e}_1 = \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0 \end{bmatrix},
> {\boldsymbol e}_2 = \begin{bmatrix}0 \\ 1 \\ \vdots \\ 0 \end{bmatrix},
> \cdots,
> {\boldsymbol e}_n = \begin{bmatrix}0 \\ 0 \\ \vdots \\ n \end{bmatrix}
> $$

#### The Spanning Set Theorem

$$
{\mathbf v}_1 = \begin{bmatrix}1\\ 0\\ 0\end{bmatrix}, 
{\mathbf v}_2 = \begin{bmatrix}0\\ 1\\ 0\end{bmatrix}, 
{\mathbf v}_3 = \begin{bmatrix}1\\ 3\\ 0\end{bmatrix}

$$

They span $H$

![[Pasted image 20241202114703.png]]

As you can see, ${\mathbf v}_3$ can be expressed as a linear combination of ${\mathbf v}_1, {\mathbf v}_2$  and so are ${\mathbf v}_1$ and ${\mathbf v}_2$. So the basis is not $\{{\mathbf v}_1,{\mathbf v}_2,{\mathbf v}_3\}$ but the set without one of them. 

You may be curious, 

> [!question]
>does it mean we can construct the set of basis by **removing** some vectors from the whole set?

The answer is YES.

>[!hint] The Spanning Set Theorem
>Let $S=\{{\mathbf v}_1,...,{\mathbf v_p}\}$ be a set in $V$, and let $H = {\rm Span}\{{\mathbf v}_1,...,{\mathbf v_p}\}$ .
>- If one of the vectors in $S$ - say, ${\mathbf v}_k$ - is a linear combination of the remaining vectors in $S$, then the set formed from $S$ by removing ${\mathbf v}_k$ still spans $H$.
>- If $H\neq \{{\mathbf 0}\}$, some subset of $S$ is a basis for $H$. 

#### Bases for ${\rm Nul\;} A$ and ${\rm Col}\; A$

##### Theorem 6

> [!hint] A way to construct basis for ${\rm Col}\; A$
> The pivot columns of a matrix $A$ form a basis for ${\rm Col}\; A$

> [!danger] Attention
> 
> Look at the 2 matrices below. They are row equivalent. What are their bases?
> 
> $$
> A = \begin{bmatrix}
> 1 & 4 & 0 & 2 & 0 \\
> 0 & 0 & 1 & -1 & 0 \\
> 0 & 0 & 0 & 0 & 1 \\
> 0 & 0 & 0 & 0 & 0 
> \end{bmatrix}
> $$
> 
> $$
> B = \begin{bmatrix}
> 1 & 4 & 0 & 2 & -1 \\
> 3 & 12 & 1 & 5 & 5 \\
> 2 & 8 & 1 & 3 & 2 \\
> 5 & 20 & 2 & 8 & 8 
> \end{bmatrix}
> $$
> 
> Though $A$ is the echelon form of $B$, but pivot columns of $A$ are not pivot columns of $B$.
> 
> Column 1, 3, 5 of $A$, $B$ form their basis respectively.
> 
> Basis of $A$: $$\begin{bmatrix}1\\ 0\\ 0\\ 0 \end{bmatrix},\begin{bmatrix}0\\ 1\\ 0\\ 0 \end{bmatrix},\begin{bmatrix}0\\ 0\\ 1\\ 0 \end{bmatrix}$$
> 
> Basis of $B$: $$\begin{bmatrix}1\\ 3\\ 2\\ 5 \end{bmatrix},\begin{bmatrix}0\\ 1\\ 1\\ 2 \end{bmatrix},\begin{bmatrix}-1\\ 5\\ 2\\ 8 \end{bmatrix}$$



#### Two Views of a Basis

To summary, the size of a basis can not be too large, but can not be too small neither. 

The set of basis is, to some extent, to be exactly of the right size.

$$
\left\{\begin{bmatrix}1\\ 0\\ 0 \end{bmatrix},\begin{bmatrix}2\\ 3\\ 0 \end{bmatrix},\begin{bmatrix}4\\ 5\\ 6 \end{bmatrix}\right\}
$$

- If there are too many vectors, then you might fail the condition of linearly independence. 
	$$\left\{\begin{bmatrix}1\\ 0\\ 0 \end{bmatrix},\begin{bmatrix}2\\ 3\\ 0 \end{bmatrix},\begin{bmatrix}4\\ 5\\ 6 \end{bmatrix},\begin{bmatrix} 7 \\ 8\\ 9 \end{bmatrix}\right\}$$
- If there are too little vectors, then your set might fail to span the space.
	$$\left\{\begin{bmatrix}1\\ 0\\ 0 \end{bmatrix},\begin{bmatrix}2\\ 3\\ 0 \end{bmatrix}\right\}$$

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


##### Theorem 7

> [!hint] The Unique Representation Theorem
> Let ${\cal B} = \{{\mathbf b}_1,...,{\mathbf b}_n\}$ be a basis for a vector space $V$. Then for each ${\mathbf x}\ in V$ , there exists a unique set of scalars $c_1,...,c_n$ such that 
> $$
> {\mathbf x} = c_1{\mathbf b}_1 + \cdots + c_n{\mathbf b}_n
> $$

##### Definition

> [!hint] Coordinate
> Suppose ${\cal B} = \{{\mathbf b}_1,...,{\mathbf b}_n\}$ is a basis for $V$ and ${\mathbf x}\in V$ . The coordinates of ${\mathbf x}$ relative to the basis ${\cal B}$ are the weights $c_1,...,c_n$ such that ${\mathbf x} = c_1{\mathbf b_1}+\cdots+c_n{\mathbf b}_n$


#### Coordinates in ${\mathbb R}^n$

$$
{\mathbf x = P_{\cal B}[{\mathbf x}]_{\cal B}}
$$
$P_{\cal B}$ is the basis, $[\mathbf x]_{\cal B}$ is the coordinate of ${\mathbf x}$ relative to the basis.

#### The Coordinate Mapping


##### Theorem 8

> [!hint] Mapping from $V$ onto ${\mathbb R}^n$
> Let ${\cal B}$ be a basis for a vector space $V$. Then the coordinate mapping ${\mathbf x}\mapsto [{\mathbf x}]_{\cal B}$ is a one-to-one linear transformation from $V$ onto ${\mathbb R}^n$



 
The rest of your content lives here. You can use **Markdown** here :)

#### The Row Space

${\rm Row\;} A = {\rm Col\;} A^T$


#### The Rank Theorem

##### Definition

>[!hint] Definition
>The rank of $A$ is the dimension of the column space of $A$
>$r(A) = {\rm dim}\; A$

##### Theorem 14

>[!hint] The Rank Theorem
>$r(A) + {\rm dim}\;{\rm Nul}\; A = n$


number of pivot columns + number of non-pivot columns = number of columns


### Rank and the Invertible Matrix Theorem

##### Theorem
[[【ALG】Matrix Operations#^588835]]

> [!hint]
> - The columns of $A$ form a basis of ${\mathbb R}^n$
> - ${\rm Col} A = {\mathbb R}^n$
> - ${\rm dim}\;{\rm Col}\; A = n$
> - $rank(A) = n$
> - ${\rm Nul} A = \{{\mathbf 0}\}$
> - ${\rm dim}\; {\rm Nul}\; A = 0$

