---
title: ALG | Eigenvalues and Eigenvectors
draft: false
tags:
  - example-tag
---
### Eigenvectors and Eigenvalues

##### Definition

>[!hint ] Definition
>For a matrix $A$ and a real number $\lambda$, 
>
>$$
>A{\mathbf x} = \lambda {\mathbf x}
>$$
>
>If the equation above **has non trivial solution**. ($A-\lambda I$ is invertible)
>
>Then those non trivial solutions ${\mathbf x}_i$ are **eigenvectors** corresponding to $\lambda$
>
>$\lambda$ is an **eigenvalue**
>
>The set $\{{\mathbf x}_i\}$ is the **eigenspace** of $A$ corresponding to $\lambda$

##### Theorem 1

> [!hint] the position of eigenvalues in a triangular matrix
> The eigenvalues of a triangular matrix are the entries on its main diagonal.
> >[!example]
> >$A - \lambda I$


0 is an eigenvalue of $A$ if and only if $A$ is not invertible.

##### Theorem 2

>[!hint] vectors across eigenspaces are linearly independent
>$\{{\mathbf v}_1,...,{\mathbf v}_r\}$ are eigenvectors that correspond to distinct eigenvalues $\lambda_1,...,\lambda_r$, then
>$\{{\mathbf v}_1,...,{\mathbf v}_r\}$ is linearly independent


### The Characteristic Equation

##### Definition

> [!hint ] characteristic equation
> $$
> {\rm det}(A-\lambda I) = 0
> $$


#### Similarity

##### Definition

> [!hint ] similarity
> $A$ and $B$ ae $n\times n$ matrices.
> 
> If there is an **invertible** matrix $P$, that
> 
> $$
> P^{-1} AP = B
> $$
> 
> then $A$ and $B$ are **similar**
> 
> $A \mapsto P^{-1}AP$ is called a **similarity transformation**


##### Theorem 4

> [!hint] similarity, characteristic polynomial and eigenvalues
> If $n\times n$ matrices $A$ and $B$ are similar,
> 
> then:
> - They have the same character polynomial
> - Hence the same eigenvalues (with the same multiplicities)
>
> >[!warning]
> >1. same eigenvalues $\nRightarrow$ similar
> >2. Similarity is not the same as row equivalence. Row operations change eigenvalues.


### Diagonalization

#### A useful factorization 

$$
A = PDP^{-1}
$$

where $D$ is a diagonal matrix.
- enables us to compute $A^k$ quickly

##### Diagonalizable

> [!hint] diagonalizable
> $A$ is **diagonalizable** $\Leftarrow$ $A$ is similar to a diagonal matrix.
> 
> - you need to have a invertible matrix $P$
> - you need to have a diagonal matrix $D$

##### Theorem 5

> [!hint ] The Diagonalization Theorem
> - An $n\times n$ matrix $A$ is diagonalizable $\Leftrightarrow$ $A$ has $n$ linearly independent eigenvectors.
> - $\Leftrightarrow$ columns of $P$ are $n$ linearly independent eigenvectors of $A$ with corresponding eigenvalues on the diagonal of $D$
> - $\Leftrightarrow$ there are enough eigenvectors to form a basis of ${\mathbb R} ^n$


#### Diagonalizing Matrices

> [!note] Solution
> 1. Find the eigenvalues of $A$
> 2. Find  all linearly independent eigenvectors of $A$
> 3. Construct $D$ with eigenvalues
> 4. Construct $P$ with eigenvectors according to $D$


##### Theorem 6

>[!hint] Another theorem to judge diagonalizable
>$n\times n$ matrix with $n$ **distinct** eigenvalues is diagonalizable

#### Matrices Whose Eigenvalues Are Not Distinct

##### Theorem 7

>[!hint] multiplicity
>1. ${\rm dim\;}{\rm Eig\;} \lambda_k \leqslant n(\lambda_k)$
>2. $A$ is diagonalizable $\Leftrightarrow$ $\sum{\rm dim\;}{\rm Eig\;} \lambda_k = n$ 
>	1. the characteristic polynomial factors completely into linear factors 
>	2. ${\rm dim}\;{\rm Eig}\; \lambda_k = n(\lambda_k)$
>3. $A$ is diagonalizable and ${\cal B}_k$ is a basis for ${\rm Eig}\;\lambda_k$ $\Rightarrow$ all the vectors in ${\cal B}_1,...,{\cal B}_p$ forms an eigenvector basis for ${\mathbb R}^n$



### Eigenvectors and Linear Transformations

#### The Matrix of a Linear Transformation

a vector ${\mathbf x}$ in vector space $V$ has its coordinate when we use a basis ${\cal B}$ in $V$.

We denote it as $[{\mathbf x}]_{\cal B}$

Now we use a linear transformation $T$ to transform ${\mathbf x}$ into $T({\mathbf x})$

The image is a vector space $W$.

Suppose we use a basis ${\cal C}$ in $W$.

Then $T({\mathbf x})$ is written in the form $[T(\mathbf x)]_{\cal C}$.

Our goal is to probe into the relationships among $[{\mathbf x}]_{\cal B},\, {\mathbf x},\, T({\mathbf x}),\, [T({\mathbf x})]_{\cal C}$

> [!question]
> Can we use a matrix $M$ to transform $[{\mathbf x}]_{\cal B}$ into $[T({\mathbf x})]_{\cal C}$ directly?
> 
> namely $$[T(\mathbf x)]_{\cal C} = M[\mathbf x]_{\cal B}$$

Let's write down all the equations that we can have.

- From $[\mathbf x]_{\cal B}$ to ${\mathbf x}$
	$${\mathbf x} = [\mathbf x]_{\cal B}\cal B$$
- From $[T({\mathbf x})]_{\cal C}$ to $T({\mathbf x})$
	$$T(\mathbf x) = [T(\mathbf x)]_{\cal C}\cal C$$
- From ${\mathbf x}$ to $T({\mathbf x})$
	$$T(\mathbf x) = A\mathbf x$$

To sum up, 
$$
[T(\mathbf x)]_{\cal C}{\cal C} = A[\mathbf x]_{\cal B}{\cal B}
$$
which is equivalent to

$$
{\cal C}[T(\mathbf x)]_{\cal C} = A{\cal B}[\mathbf x]_{\cal B}
$$

Therefore 

>[!important]
> $$
> [T(\mathbf x)]_{\cal C} = {\cal C}^{-1}A{\cal B}[\mathbf x]_{\cal B}
> $$
> $$
> M = {\cal C}^{-1}A{\cal B}
> $$
> $M$ tells us the process of the transformation:
> 1. We lift coordinates to the vector space 
> 2. We transform the vector
> 3. We push the vector down to another coordinate system

#### Linear Transformations from $V$ into $V$

Let's continue with the induction above. Think in another way...

$$
\begin{aligned}
T(\mathbf x) &= T({\cal B}[\mathbf x]_{\cal B}) \\[2ex]
& = T(r_1\mathbf b_1+\cdots +r_n\mathbf b_n) \\[2ex]
& = r_1T(\mathbf b_1) + \cdots r_nT(\mathbf b_n) \\[2ex]
& = T({\cal B})[\mathbf x]_{\cal B}\\[2ex]
& = [T]_{\cal B}[\mathbf x]_{\cal B}
\end{aligned}
$$
##### Definition

> [!hint] matrix of $T$ relative to $\cal B$
> $$ T(\mathbf x) = [T]_{\cal B}[\mathbf x]_{\cal B} $$

#### Linear Transformations on ${\mathbb R}^n$

##### Theorem 8

>[!hint] Diagonal Matrix Representation
>
>Suppose $A=PDP^{-1}$, $D$ is a diagonal $n\times n$ matrix.
>
>If $\cal B$ is the basis for ${\mathbb R}^n$, and formed from the columns of $P$
>
>then $D$ is **the $\cal B$-matrix for the transformation $\mathbf x\mapsto A\mathbf x$**

This means $[T]_{\cal B} = D$ .

#### Similarity of Matrix Representations

### Complex Eigenvalues

Sometimes characteristic equations don't always have solutions in $\mathbb R$ .

What should we do when we encounter $\lambda^2 + 1 = 0$ ?

