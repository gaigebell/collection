---
title: ALG | Linearly Independent Sets; Bases
draft: true
tags:
  - algebra
---
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
