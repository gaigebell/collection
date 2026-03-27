---
title: ALG | Systems of Linear Equation
draft: false
tags:
  - example-tag
---


> [!note] Elementary Row Operations
> 
> 1. **Replacement**: $\rm row_i\to row_i + \lambda row_j$
> 2. **Interchange**: $\rm swap(row_i, row_j)$
> 3. **Scaling**: $\rm row_i \to \lambda row_i$

Row equivalent

> $A\dfrac{\rm row \,operations}{\,}\to B$


### Existence & Uniqueness Questions




### Row Reduction and Echelon Forms

> [!hint] Definition
> 
> **echelon form**
> 
> 1. All nonzero rows are above any rows of all zeros
> 2. Each leading entry of a row in a column to the right of the leading entry of the row above it
> 3. All entries in a column below a leading entry are zeros
> 
> more conditions to be **reduced echelon form**
> 
> 4. The leading entry in each nonzero row is 1.
> 5. Each leading 1 is the only nonzero entry in its column


#### Theorem 1

> [!hint ] Uniqueness of the Reduced Echelon Form
> Each matrix is row equivalent to one and only one reduced echelon matrix.

### Pivot Positions

### The Row Reduction Algorithm

### Solutions of Linear Systems

### Parametric Descriptions of Solution Sets

### Back-Substitution

### Existence and Uniqueness Questions

#### Theorem 1

> [!hint] Existence and Uniqueness Theorem
> consistent $\Leftrightarrow$ echelon form of the augmented matrix has no row of the form $[0\,\cdot\,0\,b]$ with $b$ nonzero
> If consistent
> 1. unique: no free variables
> 2. infinitely many solutions: at least one free variables


## Vector Equations

## The Matrix Equation $A{\mathbf x}={\mathbf b}$

### Existence of Solutions

> [!hint]
> $A{\mathbf x} = {\mathbf b}$ has a solution $\Leftrightarrow$ ${\mathbf b}$ is a linear combination of the columns of $A$






## Linear Independence

> [!hint] Definition



## The Matrix of a Linear Transformation

> [!hint] Definition
> A mapping $T:\mathbb R^n\to \mathbb R^m$ is said to be **onto** $\mathbb R^m$ if each $\mathbf b$ in $\mathbb R^m$ is the image of *at least one* $\mathbf x$ in $\mathbb R^n$

>[!hint]  Theorem 11
>Let $T:\mathbb R^n\to\mathbb R^m$ be a linear transformation. 
>
>Then $T$ is one-to-one $\Leftrightarrow$ $T(\mathbf x)=\mathbf 0$ has only the trivial solution.





> [!hint] Definition
> A mapping $T:\mathbb R^n \to \mathbb R^m$ is said to be **one-to-one** if each $\mathbf b$ in $\mathbb R^m$ is the image of *at most one* $\mathbf x$ in $\mathbb R^n$

> [!hint] Theorem 12
> Let $T:\mathbb R^n \to \mathbb R^m$ be a linear transformation, and let $A$ be the standard matrix for $T$. Then:
> 
> 1. $T$ maps $\mathbb R^n$ onto $\mathbb R^m$ $\Leftrightarrow$ the columns of $A$ span $\mathbb R^m$
> 1. $T$ is one-to-one $\Leftrightarrow$ the columns of $A$ are linearly independent.



