---
title: Edge Expansion Implies Spectral Expansion
date: 2023-02-11 20:35:24
tags: [expander, graph, random walk]
categories: Tools
---

The idea of the proof is from [^1], page 429-431.

# Definition

## Spectral Expander

> Let $G$ be an $n$-vertex $d$-regular undirected graph. Let $A$ be its random walk matrix, i.e. $a_{i,j}=\frac 1d |E(i,j)|$. One can see that the largest eigenvalue of $A$ is $1$. We define $\lambda(G)$ as the second-largest eigenvalue of $H$. If $\lambda(G)\le\lambda$ for some $\lambda<1$, then we say $G$ is an $(n,d,\lambda)$*-spectral-expander*.

## Edge Expander

> Let $G=(V,E)$ be an $n$-vertex $d$-regular undirected graph. We call it an $(n,d,\rho)$*-edge-expander* iff
> $$
> \forall S\subset V, |S|\le\frac n2\implies E(S,\bar S)\ge\rho d|S|
> $$

# Edge Expansion Implies Spectral Expansion
In fact, we can show that the two expanders are somewhat "equivalent", i.e. spectral expansion implies edge expansion and vice versa. Here we focus on the hard direction: edge expansion implies spectral expansion.

## Theorem
> An $(n,d,\rho)$-edge-expander is an $(n,d,1-\frac{\rho^2}2)$-spectral-expander.

## Proof
The proof is somewhat tricky. Here the main technical obstacle is that, the eigenvector of $G$ may not be distributed evenly, thus cannot be used to directly estimate the edge expansion. (Try prove it yourself and you'll understand what I mean!)

Let $G$ be an $(n,d,\rho)$-edge-expander, and let $\lambda=\lambda(G)$. We want to show that $\lambda\le1-\frac{\rho^2}2$.

Let $\vec u$ be n unitary eigenvector of eigenvalue $\lambda$. We decompose $\vec u$ into positive and negative parts: $\vec u=\vec v+\vec w$, where $\forall i, v_i\ge0\ge w_i$. Without loss of generality, we may assume that the number of non-zero entries in $\vec v$ is $\le\frac n2$. (Otherwise we can simply take $-\vec u$). We define a special value $Z$:
$$
Z:=\sum_{i,j}a_{i,j}|v_i^2-v_j^2|
$$

Then the theorem follows immediately from the following two claims:

### Claim 1
> $Z\ge2\rho|\vec v|^2$

### Claim 2
> $Z\le\sqrt{8(1-\lambda)}|\vec v|^2$

### Proof of Claim 1
Without loss of generality, we may assume that $v_1\ge v_2\ge\cdots\ge v_n$. Then we can see that 
$$\begin{align*}
	Z&=2\sum_{i>j}a_{i,j}(v_i^2-v_j^2)\\
	&=2\sum_{i>k\ge j}a_{i,j}(v_k^2-v_{k+1}^2)\\
	&=2\sum_{k=1}^{\frac n2}(v_k^2-v_{k+1}^2)\sum_{i>k\ge j}a_{i,j}\\
	&=2\sum_{k=1}^{\frac n2}(v_k^2-v_{k+1}^2)\frac1d|E(\{1,\cdots,k\},\{k+1,\cdots,n\})|\\
	&\ge2\sum_{k=1}^{\frac n2}(v_k^2-v_{k+1}^2)k\rho\\
	&=2\rho\sum_{k=1}^{\frac n2}v_k^2\\
	&=2\rho|\vec v|^2
\end{align*}$$

### Proof of Claim 2
Since $\vec u$ is an eigenvector of $A$, we have $\vec v^TA\vec u=\lambda\vec v^T\vec u=\lambda |\vec v|^2$. On the other hand, $\vec v^TA\vec u=\vec v^TA\vec v+\vec v^TA\vec w$. One can easily see that $\vec v^TA\vec w\le0$. Therefore we have $\vec v^TA\vec v\ge\lambda|v|^2$.

We then compute another sum:
$$\begin{align*}
	&\sum_{i,j}a_{i,j}(v_i-v_j)^2\\
	=&\sum_{i,j}a_{i,j}v_i^2+\sum_{i,j}a_{i,j}v_j^2-2\sum_{i,j}a_{i,j}v_iv_j\\
	=&2|\vec v|^2-2\vec v^TA\vec v\\
	\le&2(1-\lambda)|\vec v|^2
\end{align*}$$

In other words,
$$
	1-\lambda\ge\frac{\sum_{i,j}a_{i,j}(v_i-v_j)^2}{2|\vec v|^2}
$$

Using the Cauchy-Schwarz inequality, one can see that
$$
	\left(\sum_{i,j}a_{i,j}(v_i-v_j)^2\right)\left(\sum_{i,j}a_{i,j}(v_i+v_j)^2\right)\ge\left(\sum_{i,j}a_{i,j}|v_i^2-v_j^2|\right)^2
$$

And hence we have
$$\begin{align*}
	1-\lambda&\ge\frac{\sum_{i,j}a_{i,j}(v_i-v_j)^2}{2|\vec v|^2}\\
	&=\frac{\left(\sum_{i,j}a_{i,j}(v_i-v_j)^2\right)\left(\sum_{i,j}a_{i,j}(v_i+v_j)^2\right)}{2|\vec v|^2\left(\sum_{i,j}a_{i,j}(v_i+v_j)^2\right)}\\
	&\ge\frac{Z^2}{2|\vec v|^2\left(2|\vec v|^2+2\vec v^TA\vec v\right)}\\
	&\ge\frac{Z^2}{8|\vec v|^4}
\end{align*}$$

From which we conclude that $Z\ge\sqrt{8(1-\lambda)}|\vec v|^2$.

## Trivia

This result was later generalized to the case of general reversible Markov chains[^2]. In their paper, the authors defined the "conductance" of a random walk, which corresponds to edge expansion.

In quantum computational complexity, an important result is the *QMA-completeness* of the *2-local hamiltonian problem*[^3]. The authors also adopted this idea to lowerbound the eigenvalue of a specific matrix.

[^1]: Arora S, Barak B. Computational complexity: a modern approach\[M\]. Cambridge University Press, 2009.

[^2]: Sinclair A, Jerrum M. Approximate counting, uniform generation and rapidly mixing Markov chains\[J\]. Information and Computation, 1989, 82(1): 93-133.

[^3]: Kempe J, Kitaev A, Regev O. The complexity of the local Hamiltonian problem\[J\]. Siam journal on computing, 2006, 35(5): 1070-1097.