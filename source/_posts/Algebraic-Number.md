---
title: Algebraic numbers is algebraically closed, proved using elementary linear algebra
date: 2023-02-21 12:20:29
tags: [linear algebra, matrix]
categories: elegant proof
---

Recall the definition of algebraic numbers: $x$ is an algebraic number iff it is a root of some non-zero polynomial with rational coefficients.

# Definition of Algebraic Numbers
> Let $x\in\mathbb C$ be a complex number. We call it an algebraic number iff $\exists n>0$, $\exists a_0,\cdots,a_n\in\mathbb Q, a_n\ne0$, s.t. $\sum_{i=0}^na_nx^n=0$.

# Algebraic number is algebraically closed

The algebraic numbers have an intriguing property: it is algebraically closed. That is, the sum, difference, product and quotient of two algebraic numbers is again algebraic. Moreover, the roots of a non-zero polynomial whose coefficients are algebraic numbers is again algebraic.

<!--more-->

## Theorem 1
> Let $x,y\in\mathbb C$ be some algebraic number, where $x\ne0$. Then $x+y$, $xy$, $x^{-1}$ are all algebraic numbers.

## Theorem 2
> Let $a_0,\cdots,a_n\in\mathbb C$ be some algebraic numbers, where $n>0$ and $a_n\ne0$ holds. Then all the roots of the polynomial $f(x)=\sum_{i=0}^na_nx^n$ are algebraic numbers.

Trivial as it sounds, the two theorems might not so easy to prove unless you are familiar with field theory. (You can pause now and try to prove them yourself!) However, with some matrix tricks, we can prove these theorems with just elementary linear algebra.

# From Polynomials to Matrices

Here we introduce our main tool for proving the two theorems: the *companion matrix*.

## Definition of Companion Matrix
> For $n>0$, let $f(x)=x^n+\sum_{i=0}^{n-1}a_ix^i$ be a polynomial. We define the companion matrix of $f$ to be:
> $$
> C(f)=\begin{bmatrix}
> 	0&0&0&\cdots&0&-a_0\\
> 	1&0&0&\cdots&0&-a_1\\
> 	0&1&0&\cdots&0&-a_2\\
> 	\vdots&\vdots&\vdots&\ddots&\vdots&\vdots\\
> 	0&0&0&\cdots&1&-a_{n-1}
> \end{bmatrix}	
> $$

One can verify that the characteristic polynomial of $C(f)$ is exactly $f$, thus the eigenvalues of $C(f)$ corresponds to the roots of $f$, which leads to the following lemma:

## Lemma 1
> Let $x\in\mathbb C$ be a complex number. Then $x$ is an algebraic number iff $x$ is the eigenvalue of some matrix with rational entries.

### Proof of [Lemma 1](#lemma-1)
For the *if* part, obviously the the coefficients of the characteristic polynomial of a matrix with rational entries are rational. Thus $x$ is the root of a polynomial with rational coefficient, and $x$ is algebraic.  
For the *only if* part, let $f$ be a polynomial with rational coefficients such that $f(x)=0$. Then $x$ is the eigenvalue of the companion matrix $C(f)$, which obviously has rational entries.

# Proof of the Theorems

With the tool of matrix in hand, we can now easily prove [Theorem 1](#theorem-1):

## Proof of [Theorem 1](#theorem-1)

By [Lemma 1](#lemma-1), there exists some rational matrices $G,H$ such that $x$ is an eigenvalue of $G$, while $y$ is an eigenvalue of $H$. Let $\vec u$ and $\vec v$ be the respective eigenvectors. Then we have
$$
(G\otimes I+I\otimes H)\vec u\otimes\vec v=(x+y)\vec u\otimes\vec v
$$
Here $\otimes$ denotes tensor product (Kronecker product). We can see that $(G\otimes I+I\otimes H)$ is a rational matrix, and $x+y$ is an eigenvalue of it. By [Lemma 1](#lemma-1), $x+y$ is an algebraic number.

Similarly, we have
$$
(G\otimes H)\vec u\otimes \vec v=xy\vec u\otimes\vec v
$$
Which implies that $xy$ is an algebraic number.

To show that $x^{-1}$ is an algebraic number, let $f$ be a polynomial such that $f(x)=\sum_{i=0}^na_ix^i=0$, where $a_n\ne0$. By dividing $x^n$ from the equality, we have
$$
\sum_{i=0}^na_{n-i}(x^{-1})^i=0
$$
In other words, for the rational polynomial $g(t)=\sum_{i=0}^na_{n-i}t^n$, we have $g(x^{-1})=0$. Notice that $g$ is non-zero as $a_n\ne0$. This finishes the proof of [Theorem 1](#theorem-1).

## Proof of [Theorem 2](#theorem-2)

We still use matrices as our main weapon, however, the proof is a little more involved. Let $f(x)=\sum_{i=0}^na_ix^i$ be a polynomial, where all $a_i$ are algebraic. By [Theorem 1](#theorem-1), the quotient of two algebraic numbers is again algebraic, thus we may assume that $a_n=1$. Then let $C(f)$ be the companion matrix of $f$:
$$
C(f)=\begin{bmatrix}
	0&0&0&\cdots&0&-a_0\\
	1&0&0&\cdots&0&-a_1\\
	0&1&0&\cdots&0&-a_2\\
	\vdots&\vdots&\vdots&\ddots&\vdots&\vdots\\
	0&0&0&\cdots&1&-a_{n-1}
\end{bmatrix}	
$$
And let $\vec w$ be the eigenvector corresponding to the eigenvalue $x$. Our goal is to construct a matrix with same eigenvalues as $C(f)$, however having only rational entries. Again, we adopt the idea of tensor products: we try to replace each $-a_i$ in $C(f)$ by a matrix.

Let $G_i$ be a rational matrix such that $a_i$ is an eigenvalue of it, and let $\vec{u_i}$ be the corresponding eigenvector. (The existence of $G_i$ is guarenteed by [Lemma 1](#lemma-1)). Then we construct $H_i$ and $\vec v$:
$$
\begin{align*}
	H_i&:=\left(\bigotimes_{j=0}^{i-1}I\right)\otimes G_i\otimes\left(\bigotimes_{j=i+1}^{n-1}I\right)\\
	\vec v&:=\vec{u_0}\otimes\cdots\otimes \vec{u_{n-1}}
\end{align*}
$$
One can verify that $H_i\vec v=a_i\vec v$. Then finally, we construct the block matrix $M$:
$$
M:=\begin{bmatrix}
	0&0&0&\cdots&0&-H_0\\
	I&0&0&\cdots&0&-H_1\\
	0&I&0&\cdots&0&-H_2\\
	\vdots&\vdots&\vdots&\ddots&\vdots&\vdots\\
	0&0&0&\cdots&I&-H_{n-1}
\end{bmatrix}	
$$
Then one can verify that $M\vec w\otimes\vec v=x\vec w\otimes\vec v$. Since $H_i$ are all rational, $M$ is a rational matrix, and $x$ is an eigenvalue of it. Thus by [Lemma 1](#lemma-1), $x$ is an algebraic number. This finishes our proof of [Theorem 2](#theorem-2).

# Summary
This elegant proof transforms the roots of polynomials into eigenvalues of matrices, and use tensor products to manipulate them freely. An alternate proof contains constructions with [resultant](https://en.wikipedia.org/wiki/Resultant), however, the process is more tedious and less intuitive, and requires some knowledge in abstract algebra.