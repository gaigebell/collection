---
title: ALG | Symmetric Matrices and Quadratic Forms
draft: false
tags:
  - example-tag
---

## Diagonalization of Symmetric Matrices

symmetric matrix $A^T = A$

### Theorem 1

> [!hint]
> If $A$ is symmetric, then any 2 eigenvectors from different eigenspaces are orthogonal.


Theorem 2

> [!hint]
> An $n\times n$ matrix $A$ is orthogonally diagonalizable if and only if $A$ is a symmetric matrix


## The Spectral Theorem

### Theorem 3

> [!hint] The Spectral Theorem for Symmetric Matrices
>An $n\times n$ symmetric matrix $A$ has the following properties:
>
>1. $A$ has $n$ real eigenvalues, counting multiplicities.
>2. The dimension of the eigenspace for each eigenvalue $\lambda$ equals the multiplicity of $\lambda$ as a root of the characteristic equation.
>3. The eigenspaces are mutually orthogonal, in the sense that eigenvectors corresponding to different eigenvalues are orthogonal.
>4. $A$ is orthogonally diagonalizable.


#### Spectral Decomposition

$$
A = \lambda_1\mathbf u_1\mathbf u_1^T + \lambda_2\mathbf u_2\mathbf u_2^T + \cdots + \lambda_n\mathbf u_n\mathbf u_n^T
$$


### Quadratic Forms

$$
Q(\mathbf x) = \mathbf x^T A \mathbf x
$$


#### Change of Variable in a Quadratic Form

$$
\mathbf x^T A \mathbf x = (P\mathbf y)^T A(P\mathbf y) = \mathbf y^T P^T A P \mathbf y = \mathbf y^T(P^TAP)\mathbf y = \mathbf y^T D\mathbf y
$$

transform the quadratic form into a quadratic form with no cross-product term


#### Theorem 4

>[!hint] The Principal Axes Theorem
>Let $A$ be an $n\times n$ symmetric matrix. Then there is an orthogonal change of variable , $\mathbf x = P\mathbf y$, that transforms the quadratic form $\mathbf x^T A\mathbf x$ into a quadratic form $\mathbf y^T D\mathbf y$ with no cross-product term.

### Classifying Quadratic Forms

#### Definition

> [!hint] Definition
> A quadratic form $Q$ is:
> 1. **positive definite** if $Q(\mathbf x) > 0$ for all $\mathbf x\neq \mathbf 0$
> 2. **negative definite** if $Q(\mathbf x) < 0$ for all $\mathbf x\neq \mathbf 0$
> 3. **indefinite** if $Q(\mathbf x)$ assumes both positive and negative values
> 4. **positive semidefinite** if $Q(\mathbf x)\geqslant 0$ for all $\mathbf x$
> 5. **negative semidefinite** if $Q(\mathbf x) \leqslant 0$ for all $\mathbf x$

#### Theorem 5

> [!hint] Quadratic Forms and Eigenvalues
> Let $A$ be an $n\times n$ symmetric matrix. Then a quadratic form $\mathbf x^TA\mathbf x$ is :
> 