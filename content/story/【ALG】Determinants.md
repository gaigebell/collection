---
title: ALG | Determinants
draft: false
tags:
  - example-tag
---
 
The rest of your content lives here. You can use **Markdown** here :)

## Definition


$$
\det A = a_{11}\det A_{11} - a_{12}\det A_{12} + \cdots +(-1)^{1+n}a_{1n}\det A_{1n}
$$


#### $(i,j)$-cofactor

$$
C_{ij}=(-1)^{i+j}\det A_{ij}
$$


## Properties of Determinants

#### THEOREM 3

Let $A$ be a square matrix

1. Replacement $A\to B$, then $\det A = \det B$
2. Interchange $\det B = -\det A$
3. Scaling $\det B = k\cdot \det A$


$$
\det A^T = \det A
$$

$$
\det AB = (\det A)(\det B)
$$


#### Cramer's Rule

$$
x_i = \dfrac{\det A_i(\mathbf b)}{\det A}
$$


#### THEOREM 8

$$
A^{-1} = \dfrac{1}{\det A}{\rm adj\,} A
$$



### THEOREM 10

> [!hint]
> Let $T:\mathbb R^2\to \mathbb R^2$
> 
> $$
> \{{\rm area\, of\,} T(S)\} = |\det A|\cdot \{{\rm area\, of\,} S\}
> $$
> 
> $$
> \{{\rm volume\, of\, } T(S)\} = |\det A|\cdot \{{\rm volume\, of\,} S\}
> $$




