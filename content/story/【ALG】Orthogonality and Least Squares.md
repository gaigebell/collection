---
title: ALG | Orthogonality and Least Squares
draft: false
tags:
  - example-tag
---
 
### **Inner Product, Length, and Orthogonality**

#### **Inner Product**

>[!hint]
>$$
>{\mathbf u}\cdot {\mathbf v} = {\mathbf u}^T{\mathbf v}
>$$

#### **The Length of a Vector**

>[!hint]
>$$
>\| \mathbf v\| = \sqrt{\mathbf v\cdot \mathbf v} = \sqrt{\sum_{i=1}^n v_i^2}
>$$


#### **Distance in $\mathbb R^n$**

>[!hint]
>$$
>{\rm dist}(\mathbf u,\mathbf v) = \|\mathbf u - \mathbf v\|
>$$


#### **Orthogonal Vectors**

> [!hint]
> $$
> \mathbf u \cdot \mathbf v = 0
> $$

>[!hint] The Pythagorean Theorem
>$$
>\|\mathbf u + \mathbf v\|^2 = \|\mathbf u\|^2 + \|\mathbf v\|^2
>$$


#### **Orthogonal Complements**

> [!hint]
> 1. A vector $\mathbf x$ is in $W^{\perp}$ if and only if $\mathbf x$ is orthogonal to every vector in a set that spans $W$.
> 2. $W^{\perp}$ is a subspace of $\mathbb R^n$

##### Theorem 3

> [!hint] ${\rm Row\,}A,{\rm Col\,}A,{\rm Nul\,} A$
> $$
> ({\rm Row\,}A)^{\perp} = {\rm Nul\,}A
> $$
> $$
> ({\rm Col\,}A)^{\perp} = {\rm Nul\,}A^T
> $$

#### **Angles in $\mathbb R^2$ and $\mathbb R^3$**

$$
\mathbf u\cdot \mathbf v = \|\mathbf u\|\|\mathbf v\|\cos \vartheta
$$

### **Orthogonal Sets**

##### Theorem 4

>[!hint] Orthogonal Sets
>If $S = \{\mathbf u_1, ...,\mathbf u_n\}$ is an orthogonal set of nonzero vectors in $\mathbb R^n$, then $S$ is linearly independent and hence is a basis for the subspace spanned by $S$


##### Definition

> [!hint] Orthogonal Basis
> An orthogonal basis for a subspace $W$ of $\mathbb R^n$ is a basis for $W$ that is also an orthogonal set.


##### Theorem 5

> [!hint]
> Let $\{\mathbf u_1,...\mathbf u_p\}$ be an orthogonal basis for a subspace $W$ of $\mathbb R^n$ . For each $\mathbf y$ in $W$, the weights in the linear combination
> 
> $$
> \mathbf y = \sum_{i=1}^n c_i\mathbf u_i
> $$
> 
> are given by
> 
> $$
> c_j = \dfrac{\mathbf y\cdot \mathbf u_j}{\mathbf u_j\cdot u_j}\,\,\,\, (j=1,...,p)
> $$

#### **An Orthogonal Projection**

>[!hint] projection
>$$
>\hat{\mathbf y} = {\rm proj}_L \mathbf y = \dfrac{\mathbf y\cdot \mathbf u}{\mathbf u\cdot \mathbf u}\mathbf u
>$$


#### **Orthonormal Sets**

orthogonal set of unit vectors

##### Theorem 6

>[!hint]
>An $m\times n$ matrix $U$ has orthonormal columns if and only if $U^TU=I$ 

##### Theorem 7

>[!hint]
>Let $U$ be an $m\times n$ matrix with orthonormal columns, and let $\mathbf x$ and $\mathbf y$ be in $\mathbb R^n$. Then
>
>1. $\|U\mathbf x\| = \|\mathbf x\|$
>2. $(U\mathbf x)\cdot (U\mathbf y) = \mathbf x\cdot \mathbf y$
>3. $(U\mathbf x)\cdot (U\mathbf y) = 0$ if and only if $\mathbf x\cdot \mathbf y = 0$

An orthogonal matrix is a square invertible matrix $U$ such that $U^{-1} = U^T$

#### **Orthogonal Projections**

##### Theorem 8

>[!hint] The Orthogonal Decomposition Theorem
>Let $W$ be a subspace of $\mathbb R^n$. Then each $\mathbf y$ in $\mathbb R^n$ can be written uniquely in the form 
>
>$$
>\mathbf y = \hat{\mathbf y} + \mathbf z
>$$
>
>where $\hat{\mathbf y}$ is in $W$ and $\mathbf z$ is in $W^\perp$. In fact, if $\{\mathbf u_1,...,\mathbf u_p\}$ is any orthogonal basis of $W$, then 
>
>$$
>\hat{\mathbf y} = \sum_{i=1}^p \dfrac{\mathbf y \cdot \mathbf u_i}{\mathbf u_i\cdot \mathbf u_i}\mathbf u_i
>$$
>
>and $\mathbf z = \mathbf y - \hat{\mathbf y}$



#### **Properties of Orthogonal Projections**

Theorem 9

> [!hint] The Best Approximation Theorem
> Let $W$ be a subspace of $\mathbb R^n$, let $\mathbf y$ be any vector in $\mathbb R^n$, and let $\hat{\mathbf y}$ be the orthogonal projection of $\mathbf y$ onto $W$. Then $\hat{\mathbf y}$ is the closest point in $W$ to $\mathbf y$, in the sense that 
> 
> $$
> \|\mathbf y - \hat{\mathbf y} \| < \|\mathbf y - \mathbf v\|
> $$
> 
> for all $\mathbf v$ in $W$ distinct from $\hat{\mathbf y}$

Theorem 10

> [!hint] Theorem 10
> If $\{\mathbf u_1,...\mathbf u_p\}$ is an orthonormal basis for a subspace $W$ of $\mathbb R^n$, then 
> 
> $$
> \mathrm{proj}_{W}{\mathbf y} = (\mathbf y\cdot \mathbf u_1)\mathbf u_1 + (\mathbf y\cdot \mathbf u_2)\mathbf u_2 + \cdots + (\mathbf y\cdot \mathbf u_p)\mathbf u_p
> $$
> 
> If $U = [\mathbf u_1\,\mathbf u_2\,\cdots\, \mathbf u_p]$, then
> 
> $$
> \mathrm{proj}_W\mathbf{y} = UU^T\mathbf{y}
> $$
> 
> for all $\mathbf y$ in $\mathbb R^n$


#### **The Gram-Schmidt Process**

Theorem 11

> [!hint] The Gram-Schmidt Process
> Given a basis $\{\mathbf x_1,...,\mathbf x_p\}$ for a nonzero subspace $W$ of $\mathbb R^n$, define
> $$
> \begin{aligned}
> \mathbf v_1 &= \mathbf x_1 \\[2ex]
> \mathbf v_2 &= \mathbf x_2 - \dfrac{\mathbf x_2\cdot \mathbf v_1}{\mathbf v_1\cdot \mathbf v_1} \mathbf v_1 \\[2ex]
> \mathbf v_3 &= \mathbf x_3 - \dfrac{\mathbf x_3\cdot \mathbf v_1}{\mathbf v_1\cdot \mathbf v_1} \mathbf v_1 - \dfrac{\mathbf x_3\cdot \mathbf v_2}{\mathbf v_2\cdot \mathbf v_2} \mathbf v_2 \\[2ex]
> \vdots \\[2ex]
> \mathbf v_p &= \mathbf x_p - \sum_{i=1}^{p-1}\dfrac{\mathbf x_p \cdot \mathbf v_i}{\mathbf v_i\cdot \mathbf v_i}\mathbf v_i
> \end{aligned}
> $$
> 
> Then $\{\mathbf v_p\}$ is an orthogonal basis for $W$. 
> $$
> \mathrm{Span}\{\mathbf v_k\}=\mathrm{Span}\{\mathbf x_k\}\,\,,\mathrm{for}\,1\leqslant k\leqslant p
> $$

#### **QR Factorization of Matrices**

Theorem 12 ^52e47d

> [!hint] The QR Factorization
> If $A$ is an $m\times n$ matrix with linearly independent columns
> then $A$ can be factored as $A=QR$, where $Q$ is an $m\times n$ matrix whose columns form an orthonormal basis for $\mathrm{Col}\, A$ 
> and $R$ is an $n\times n$ upper triangular invertible matrix with positive entries on its diagonal.



### **Least-Squares Problems**

Definition
>[!hint] Definition
>If $A$ is $m\times n$ and $\mathbf b$ is in $\mathbb R^m$, a least-squares solution of $A{\mathbf x}=\mathbf b$ is an $\hat{\mathbf x}$ in $\mathbb R^n$ such that
>$$
>\|\mathbf b - A\hat{\mathbf x}\| \leqslant \|\mathbf b - A\mathbf x\|
>$$
>for all $\mathbf x$ in $\mathbb R^n$ 
 

#### **Solution of the General Least-Squares Problems**

$$
\hat{\mathbf b} = \mathrm{proj}_{\mathrm{Col\,}A}\mathbf b
$$

We need to solve

$$
A\hat{\mathbf x} = \hat{\mathbf b}
$$

#### Theorem 13

> [!hint]
> The set of least-squares solutions of $A\mathbf x = \mathbf b$ coincides with the nonempty set of solutions of the normal equations $$A^TA\mathbf x = A^T\mathbf b $$



#### Theorem 14

> [!hint]
> Let $A$ be an $m\times n$ matrix. The following statements are logically equivalent:
> 1. The equation $A\mathbf x = \mathbf b$ has a unique least-squares solution for each $\mathbf b$ in $\mathbb R^m$ $\Leftrightarrow$
> 2. The columns of $A$ are linearly independent. $\Leftrightarrow$
> 3. The matrix $A^TA$ is invertible. $\Leftrightarrow$
> 4. This unique solution is $$\hat{\mathbf x} = (A^TA)^{-1}A^T\mathbf b$$


#### **Alternative Calculations of Least-Squares Solutions**

#### Theorem 15
> [!hint] 
>Given an $m\times n$ matrix $A$ with linearly independent columns, let $A=QR$ be a QR factorization of $A$ as in Theorem12([[【ALG】Orthogonality and Least Squares#^52e47d]])
>Then, for each $\mathbf b$ in $\mathbb R^m$, the equation $A\mathbf x = \mathbf b$ has a unique least-squares solution, given by
>$$
>\hat{\mathbf x} = R^{-1} Q^T \mathbf b
>$$

### **Inner Product Spaces**

#### **Definition**

> [!hint] Definition
> An **inner product** on a vector space $V$ is a function that, to each pair of vectors $\mathbf u$ and $\mathbf v$ in $V$, associates a real number $\langle\mathbf u, \mathbf v \rangle$ and satisfies the following axioms, for all $\mathbf u, \mathbf v$ and $\mathbf w$ in $V$ and all scalars $c$ :
> 1. $\langle \mathbf u, \mathbf v \rangle$ = $\langle \mathbf v, \mathbf u \rangle$
> 2. $\langle \mathbf u + \mathbf v, \mathbf w\rangle = \langle \mathbf u, \mathbf w\rangle + \langle \mathbf v,\mathbf w\rangle$
> 3. $\langle c\mathbf u,\mathbf v\rangle = c\langle \mathbf u,\mathbf v\rangle$
> 4. $\langle \mathbf u, \mathbf u\rangle \geqslant 0$ and $\langle \mathbf u,\mathbf u\rangle = 0$ if and only if $\mathbf u = \mathbf 0$
> 
> A vector space with an inner product is called an **inner product space**

#### **Two Inequalities**

> [!hint] The Cauchy-Schwarz Inequality
> For all $\mathbf u, \mathbf v$ in $V$,
> $$
> |\langle\mathbf u, \mathbf v\rangle| \leqslant ||\mathbf u||\;||\mathbf v||
> $$


> [!hint] The Triangle Inequality
> For all $\mathbf u, \mathbf v$ in $V$,
> $$
> ||\mathbf u + \mathbf v||\leqslant ||\mathbf u|| + ||\mathbf v||
> $$









