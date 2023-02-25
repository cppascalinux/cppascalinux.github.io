---
title: Optimality of Grover's Algorithm
date: 2023-02-24 23:10:56
tags: [quantum, linear algeket, algorithm]
categories: quantum
---

# Introduction

Let's start with the *SAT* problem: suppose we are given a boolean SAT formula with $n$ variables and $poly(n)$ clauses, and we want to know whether there exists a satisfiying assignment. This problem is known to be *NP-complete*, thus is generally believed to be hard to solve on a classical computer. The *exponential time hypothesis* states that satisfiability of SAT cannot be solved in $2^{o(n)}$ time. However, with *Grover's algorithm* on a quantum computer, one can determine the existence of satisfying assignment, or even find one, within time $O\left(2^{\frac n2}\right)$! In fact, for any search problem with $N$ candidates and $M$ solutions, *Grover's algorithm* can find one solution with $O\left(\sqrt{\frac NM}\right)$ operations, while a classical algorithm usually takes $O(\frac NM)$ operations.

This implies that quantum computers are inherently much more powerful that classical computers, right? Well, yes but no. On one hand, *Grover's algorithm* does offer a sqaure root speed up over classical algorithms. On the other hand, however, one can prove that for a general unstructured search problem with $N$ candidates and $M$ solutions, no quantum algorithms take less than $\Theta\left(\sqrt{\frac NM}\right)$ operations to find a solution! This can be huge disappointment for quantum enthusiasts, who might believe that quantum computers might offer a super-polynomial or even exponential speed up over classical computers for general unstructured problems. We will dive into this topic in the following paragraphs.

# Basic facts

## Definition of general search problems

> Let $S=\{0,1\}^n$ be the search space, and $R\subset S$ be the solutions, we define $N=|S|,M=|R|$. Let $f_R:S\to\{0,1\}$ be a function that checks whether a given solution is valid, i.e. $f_R(x)=1\iff x\in R$. To find a solution, we are allowed to query $f_R$ as an oracle in our algorithm design. (For a quantum algorithm, we are given an "oracle operation" $O_R$ such that $O_R\ket x\ket b=\ket x\ket{b\oplus f_R(x)}$) The efficiency is measured by the number of oracle queries.

## Hardness of general search problems

With no other assumptions about the structure of the problem, the best a classical algorithm can do is to guess a solution randomly. Obviously it takes $\Theta\left(\frac NM\right)$ oracle queries on average to find a solution. However, for a quantum computer, it only takes $\Theta\left(\sqrt{\frac NM}\right)$ oracle queries to do so, using *Grover's algorithm*. The procedure of the algorithm is left out in this post, and you are encouraged to check it out yourself.

# Optimality of Grover's algorithm

First, we introduce a lemma to bound the sum of vector norms:

## Lemma 1

> Let $\{\alpha_i\}$ and $\{\beta_i\}$ be some finite sequence of vectors in a inner product space. Then we have
> $$\begin{align}
> 	\sum_i\|\alpha_i+\beta_i\|^2&\le\left(\sqrt{\sum_i\|\alpha_i\|^2}+\sqrt{\sum_i\|\beta_i\|^2}\right)^2\\
> 	\sum_i\|\alpha_i+\beta_i\|^2&\ge\left(\sqrt{\sum_i\|\alpha_i\|^2}-\sqrt{\sum_i\|\beta_i\|^2}\right)^2
> \end{align}$$

## Proof of [Lemma 1](#lemma-1)

By the triangle inequality, we have

$$\begin{align}
	\sum_i\|\alpha_i+\beta_i\|^2
	&\le\sum_i(\|\alpha_i\|+\|\beta_i\|)^2\\
	&=\sum_i\|\alpha_i\|^2+\sum_i\|\beta_i\|^2+2\sum_i\|\alpha_i\|\|\beta_i\|
\end{align}$$

Then we apply Cauchy-Schwarz inequality on the third term:

$$\begin{align}
	\sum_i\|\alpha_i+\beta_i\|^2
	&\le\sum_i\|\alpha_i\|^2+\sum_i\|\beta_i\|^2+2\sqrt{\sum_i\|\alpha_i\|^2}\sqrt{\sum_i\|\beta_i\|^2}\\
	&=\left(\sqrt{\sum_i\|\alpha_i\|^2}+\sqrt{\sum_i\|\beta_i\|^2}\right)^2
\end{align}$$

The other side is similar:

$$\begin{align}
	\sum_i\|\alpha_i+\beta_i\|^2
	&\ge\sum_i(\|\alpha_i\|-\|\beta_i\|)^2\\
	&=\sum_i\|\alpha_i\|^2+\sum_i\|\beta_i\|^2-2\sum_i\|\alpha_i\|\|\beta_i\|\\
	&\ge\sum_i\|\alpha_i\|^2+\sum_i\|\beta_i\|^2-2\sqrt{\sum_i\|\alpha_i\|^2}\sqrt{\sum_i\|\beta_i\|^2}\\
	&=\left(\sqrt{\sum_i\|\alpha_i\|^2}-\sqrt{\sum_i\|\beta_i\|^2}\right)^2
\end{align}$$

Now we are going to prove the main result:

## Theorem 1

> All quantum algorithms that find a solution with $O(1)$ probability requires $\Omega\left(\sqrt{\frac NM}\right)$ oracle queries.

## Proof of [Theorem 1](#theorem-1)

Without loss of generality, we may assume that the quantum algorithm uses $m$ qubits for some $m>n$. It applies $W$ unitary operations, interleaved with $W$ oracle operations. More specifically, let $\ket\psi$ be the initial state of the register, we compute $\ket{\psi_W^R}:=U_WO_RU_{W-1}O_R\cdots U_1O_R\ket\psi$, and then measure the first $n$ qubits of $\ket{\psi_W^R}$ as an answer. We may assume that the oracle queries are done on the first $n$ qubits, and the result is XORed with the $(n+1)$'th qubit. (Otherwise we can "swap" the queried qubits with the first $(n+1)$ qubits, and then swap them back after the query, as there is no limit on the unitary operations we apply.)

We define
$$\begin{align}
	\ket{\psi_k^R}&:=U_kO_RU_{k-1}O_R\cdots U_1O_R\ket\psi\\
	\ket{\psi_k}&:=U_kU_{k-1}\cdots U_1\ket\psi\\
	D_k&:=\sum_{R\subset S}\left\|\psi_k^R-\psi_k\right\|^2
\end{align}$$

### Upperbound of $D_W$

For the first half of the proof, we upperbound $D_k$ by $4k^2\binom{N-1}{M-1}$ using induction:

$$\begin{align}
	D_{k+1}
	&=\sum_{R\subset S}\left\|U_{k+1}O_R\psi_k^R-U_{k+1}\psi_k\right\|^2\\
	&=\sum_{R\subset S}\left\|O_R\psi_k^R-\psi_k\right\|^2\\
	&=\sum_{R\subset S}\left\|O_R(\psi_k^R-\psi_k)+(O_R-I)\psi_k\right\|^2\\
\end{align}$$

Notice that $O_R$ and $I$ can be written as
$$\begin{align}
	O_R&=\sum_{x\notin R}\ket x\bra x\otimes I\otimes I+\sum_{x\in R}\ket x\bra x\otimes X\otimes I\\
	I&=\sum_{x\notin R}\ket x\bra x\otimes I\otimes I+\sum_{x\in R}\ket x\bra x\otimes I\otimes I
\end{align}$$

Therefore we have
$$\begin{align}
	D_{k+1}
	&=\sum_{R\subset S}\left\|O_R(\psi_k^R-\psi_k)+\left(\sum_{x\in R}\ket x\bra x\otimes(X-I)\otimes I\right)\psi_k\right\|^2
\end{align}$$

To apply [Lemma 1](lemma-1), we upper bound the first term:

$$\begin{align}
	\sum_{R\subset S}\left\|O_R(\psi_k^R-\psi_k)\right\|^2
	&=\sum_{R\subset S}\left\|(\psi_k^R-\psi_k)\right\|^2\\
	&=D_k\\
\end{align}$$

and the second term:

$$\begin{align}
	&\sum_{R\subset S}\left\|\left(\sum_{x\in R}\ket x\bra x\otimes(X-I)\otimes I\right)\psi_k\right\|^2\\
	&=\sum_{R\subset S}\sum_{x\in R}\braket{\psi_k|(\ket x\bra x\otimes(2I-2X)\otimes I)|\psi_k}\\
	&=2\binom{N-1}{M-1}\sum_{x\in S}\braket{\psi_k|(\ket x\bra x\otimes(I-X))|\psi_k}\\
	&=2\binom{N-1}{M-1}(\braket{\psi_k|\psi_k}-\braket{\psi_k|I\otimes X\otimes I|\psi_k})\\
	&\le4\binom{N-1}{M-1}
\end{align}$$

By induction, $D_k\le4k^2\binom{N-1}{M-1}$, and with [Lemma 1](#lemma-1) we conclude that
$$\begin{align}
	D_{k+1}
	&\le\left(2k\sqrt{\binom{N-1}{M-1}}+2\sqrt{\binom{N-1}{M-1}}\right)^2\\
	&=4(k+1)^2\binom{N-1}{M-1}
\end{align}$$

### Lowerbound of $D_W$

For the second half of the proof, we lowerbound $D_k$ by $\Omega(1)\binom NM$. First, we define the projection matrix onto the subspace spanned by the solutions $R$:

$$\begin{align}
	P_R:=\sum_{x\in R}\ket x\bra x\otimes I
\end{align}$$

Then again, we split $D_W$ into two parts, and try to apply [Lemma 1](#lemma-1):

$$\begin{align}
	D_W
	&=\sum_{R\subset S}\left\|(I-P_R)\psi_W^R+(P_R\psi_W^R-\psi_W)\right\|^2\\
\end{align}$$

For the first term, we have $\braket{\psi_W^R|(I-P_R)|\psi_W^R}\le\frac12$, as we may assume that the probability of success is no less than $\frac12$ for this quantum algorithm. Thus we write down

$$\begin{align}
	\sum_{R\subset S}\left\|(I-P_R)\psi_W^R\right\|^2\le\frac12\binom NM
\end{align}$$

For the second term, we have

$$\begin{align}
	\left\|P_R\psi_W^R-\psi_W\right\|^2
	&=1+\braket{\psi_W^R|P_R|\psi_W^R}-2\Re\braket{\psi_W|P_R|\psi_W^R}\\
	&\ge\frac32-2\|P_R\psi_W\|^2\\
	&=\frac32-2\braket{\psi_W|P_R|\psi_W}\\
\end{align}$$

Summing over $R$,

$$\begin{align}
	\sum_{R\subset S}\left\|P_R\psi_W^R-\psi_W\right\|^2
	&\ge\frac32\binom NM-2\sum_{R\subset S}\sum_{x\in R}\braket{\psi_W|(\ket x\bra x\otimes I)|\psi_W}\\
	&=\frac32\binom NM-2\binom{N-1}{M-1}\sum_{x\in S}\braket{\psi_W|(\ket x\bra x\otimes I)|\psi_W}\\
	&=\frac32\binom NM-2\binom{N-1}{M-1}\\
	&=\left(\frac32-2\frac MN\right)\binom NM
\end{align}$$

We may assume that $\frac MN\le\frac14$. (In fact, for $\frac MN>\frac 14$, the problem is trivial as one only needs less than 4 trials on average using a naive algorithm.) We then apply [Lemma 1](#lemma-1):

$$\begin{align}
	D_W
	&\ge\left(\sqrt{\frac32-2\frac MN}-\sqrt{\frac12}\right)^2\binom NM\\
	&\ge\left(\frac32-\sqrt2\right)\binom NM
\end{align}$$

Combining the lowerbound and upperbound of $D_W$, we have

$$\begin{align}
	&\left(\frac32-\sqrt2\right)\binom NM\le D_W\le4W^2\binom{N-1}{M-1}\\
	&\implies W\ge\frac{2-\sqrt2}4\sqrt{\frac NM}\\
	&\implies W\ge\Omega\left(\sqrt{\frac NM}\right)
\end{align}$$

Which completes our proof.

# Summary

Although the proof is elegant, the result is rather disappointing, as some of us might expect more computational power out of a quantum computer. As we have learnt, assuming no structure for the solutions, a quantum computer can offer at most a polynomial speed up. (However, for problem with nice structures, we might be able to achieve exponential speedup, a example would be integer factoring, which can be solved effectively with quantum fourier transform, while no known classical algorithm can do factorization effectively.) This doesn't imply $\textbf{BQP}\subset \textbf{NP}$ however, as there might be some yet unknown structures for the solution space of $\textbf{NP}$-complete problems which we can exploit.

I read the original proof on *Quantum Computation and Quantum Information*, page 269-271. However, the proof only considered the situation where $M=1$, so I spent like a whole afternoon generalizing it to arbitrary $M$. That's when I came up with the idea of projection matrix in the second part of the proof. However, while I was writing this post, I realized that the original proof only allowed queries of "phase oracle", rather than the stantard oracle case, yet I don't know how to transform a phase oracle to a standard oracle. (If you know how to do the reduction, please contact me, and I'd like to learn about it!) So I spent another afternoon modifying the proof so that it applys to standard oracle, though making the proof more tedious.