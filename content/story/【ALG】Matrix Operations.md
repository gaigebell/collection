---
title: ALG3 | Matrix Operations
draft: false
tags:
  - example-tag
---
> Content is on the way :-) ...



Theorem 1
$A,B,C$ are matrices of the same size, $r,s$ are scalars
1. $A+B=B+A$
2. $(A+B)+C=A+(B+C)$
3. $A+0=A$
4. $r(A+B)=rA+rB$
5. $(r+s)A=rA+sA$
6. $r(sA)=(rs)A$

Matrix multiplication
$$
AB=C,{\rm where}\,\,\, C_{i,j} = \sum_{k=1}^tA_{i,k}\times B_{k,j}
$$
implicit condition: $A$ is $m\times t$ and $B$ is $t\times n$，then $C$ is $m\times n$

connection with vector equations
$$
AB=A[\boldsymbol b_1\;\boldsymbol b_2\; ... \boldsymbol b_p] = [A\boldsymbol b_1\;A\boldsymbol b_2\; ... A\boldsymbol b_p]
$$

As for computing a row
$$
row_i(AB) = row_i(A)\cdot B
$$
How about computing a column
$$
col_i(AB) = A\cdot col_i(B)
$$

Properties of Matrix Multiplication

Theorem 2
$A$ is $m\times n$
1. $A(BC) = (AB)C$
2. $A(B +C) = AB+AC$
3. $(B+C)A=BA + CA$
4. $r(AB) = (rA)B=A(rB), r$ is scalar
5. $I_mA=A=AI_n$



Transpose of a Matrix

Theorem 3
1. $(A^T)^T = A$
2. $(A+B)^T = A^T + B^T$
3. For any scalar $r$, $(rA)^T = rA^T$
4. $(AB)^T = B^TA^T$ (==reverse order==)


