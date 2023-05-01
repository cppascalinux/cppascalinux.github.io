---
title: Secretary Problems and High-order Sum of Harmonic Series
date: 2023-04-30 00:27:10
tags: [summation, combinatorics, algorithm, discrete math, generating function]
categories: combinatorics
---

# Background

## The story
So I was reading this [lecture note](https://www.cs.cmu.edu/afs/cs.cmu.edu/academic/class/15850-f20/www/notes/lec27.pdf) about secretary problems, and I was trying to prove Theorem 24.3 myself. I also obtained an sum representing the probability which is different from the one in the lecture note. However I had some difficulty proving that the two sums are equilvalent (if not given this problem). After a day of suffering, using the summation by parts technique, I found that the sum was in fact a high-order sum of the harmonic series, and that this sum has a pretty simple representation.

<!--more-->

## Secretary problems
Here we define the secretary problems. Suppose there are $n$ items, each with a value $v_i\ge0$, which you doesn't know in advance. Now these items are presented to you one by one, in uniformly random order. Each time you are presented with an item and its value, you can choose to pick it and end the game. Or, you can choose to pass, and request the next item. Notice that once an item has been rejected, it cannot be picked anymore. Now design an algorithm to maximize the expected value you get.

Let $V_{\max}$ denote the maximum value. Then there is a simple strategy with expected value $\frac14\mathbb E[V_{\max}]$: we pass the first half, and record the maximum value; then for the second half, we take the first item that has value greater then the recorded value. Obviously we can take the maximum value with probability $\ge\frac14$, so the expected value is at least $\frac14\mathbb E[V_{\max}]$.

## Wait-and-pick algorithms
The above algorithm is called a wait-and-pick algorithm. In fact, with a more detailed analysis, we can obtain a better result than $\frac14\mathbb E[V_{\max}]$. Suppose there are $n$ items, and we pass the first $m$ items and record the maximum value. Then for the rest $n-m$ items, we take the first one that is larger than the recorded value. Then we can analyze the probability that we pick the $V_{\max}$.

My first thought is to enumerate over the maximum value of the first $m$ items. Without loss of gernerality, we may assume that the $\{\text{value of items}\}=[n]$. Then the probability that the maximum over the first $m$ items being $k$ is
$$
	\frac{\binom{k-1}{m-1}}{\binom nm}
$$
Conditioned on the first $m$ items chosen, we may look at the permutation of the rest $n-m$ items. Notice that there are $n-k$ items larger than $k$ in the second portion, and the probability that $n$ being the first item to appear is $\frac1{n-k}$. So overall, the probability can be written as
$$
	\Pr[\text{Pick }n]=\sum_{k=m}^{n-1}\frac{\binom{k-1}{m-1}}{\binom nm}\frac1{n-k}=\frac1{\binom nm}\sum_{k=m}^{n-1}\binom{k-1}{m-1}\frac1{n-k}
$$

However, we might take a different approach as in the lecture note, that we enumerate over the position of $n$ in the sequence. Suppose that $n$ appears at the $k$'th position, where $k>m$. Then the event that we pick $n$ is equivalent to the event that the maximum of the first $k-1$ items lies within first $m$ items, or the "pass portion". Therefore we can write the expression:
$$
	\Pr[\text{Pick }n]=\sum_{k=m+1}^n\frac1n\frac m{k-1}=\frac mn\sum_{k=m}^{n-1}\frac1k
$$
And this expression is much easier to analyze than the previous one. By approximating the harmonic sequence with natural logarithm, we can see that for $m\approx \frac n{\text e}$, the probability is maximized at $\frac1{\text e}$, so our expected value is at least $\frac1{\text e}\mathbb E[V_{\max}]$.

So now we have two expressions for $\Pr[\text{Pick }n]$. However, they look completely different! Are they really equilvalent? Well, by this problem we know the answer is yes. However, if not given the background of the secretary problem and wait-and-pick algorithm, how are we gonna prove this equation?


# Equation of sums
We can write down the following equation:
$$\begin{equation}
	\sum_{k=m}^{n-1}\binom{k-1}{m-1}\frac1{n-k}=\binom{n-1}{m-1}\sum_{k=m}^{n-1}\frac1k\label{eq1}
\end{equation}$$
And we want to prove this equation, without relying on the secretary problem we just described. In other words, we want to sum up the left-hand side.

## Failed attempts
My first intuition is *High-order differences*. This trick can be used to prove the following equation: (See *Concrete Mathematics*, pp. 187-192)
$$
	\sum_k\binom nk\frac{(-1)^k}{x+k}=\frac1{\binom{x+n}nx}
$$
This equation might somehow look similar to the our target. However, even with the trick of negating the upper index (See *Concrete Mathematics*, p. 164), I was stuck and couldn't simplify the sum.

Then I tried to use *generating functions* since the sum looked like some convolution. But again I was stuck, as convolution couldn't handle terms involving $n$. (Most likely the all-mighty generating functions can solve this problem, but I'm not so familiar with it. QwQ) **(Update: see [Generating function](#generating-function))** Then I realized that, perhaps I have to try some more classical and brute methods.

## Summation by parts
This is what brings real hope to the problem. We can use the summation by parts trick on the left-hand side of $\eqref{eq1}$:
$$\begin{align*}
	\sum_{k=m}^{n-1}\binom{k-1}{m-1}\frac1{n-k}
	&=\sum_{k=1}^{n-m}\binom{n-k-1}{m-1}\frac 1k\\
	&=\sum_{k=1}^{n-m}\left(\binom{n-k-1}{m-1}-\binom{n-k-2}{m-1}\right)\left(\sum_{i=1}^k\frac 1i\right)\\
	&=\sum_{k=1}^{n-m}\binom{n-k-2}{m-2}H_k
\end{align*}$$
Here we define $H_k:=\sum_{i=1}^k\frac1i$. After one round of summation by parts, we have reduced the lower index of the combination numbers by one. Therefore after many rounds, we cna fully eliminate the combination numbers! Let $S_{l,k}$ denote the $k$'th term of the $l$'th order prefix sum of the harmonic series. More specifically, we define $S_{l,k}$ as follows:
$$\begin{align*}
	S_{0,k}&:=\frac1k\\
	S_{1,k}&:=\sum_{i=1}^k S_{0,i}\\
	&\vdots\\
	S_{l,k}&:=\sum_{i=1}^k S_{l-1,i}\\
\end{align*}$$
Then by applying summation by parts repeatedly, we obtain that
$$\begin{align*}
	\sum_{k=1}^{n-m}\binom{n-k-1}{m-1}S_{0,k}
	&=\sum_{k=1}^{n-m}\binom{n-k-2}{m-2}S_{1,k}\\
	&\vdots\\
	&=\sum_{k=1}^{n-m}\binom{n-k-m}0S_{m-1,k}\\
	&=\sum_{k=1}^{n-m}S_{m-1,k}\\
	&=H_{m,n-m}
\end{align*}$$
Now the toxic sum has been simplified to just a single symbol. However a new problem arises: how to prove $S_{m,n-m}$ equals the right-hand side of $\eqref{eq1}$? Well, now we can easily apply induction. We want to prove the following equation:
$$
	S_{m,n}=\binom{n+m-1}{m-1}(H_{n+m-1}-H_{m-1})
$$
First, for $m=1$, the equation obviously holds. Then suppose that it holds for some $m$. Then for $S_{m+1,n}$, we have
$$\begin{align*}
	S_{m+1,n}
	&=\sum_{i=0}^nS_{m,i}\\
	&=\sum_{i=0}^n\binom{i+m-1}{m-1}(H_{i+m-1}-H_{m-1})\\
	&=-H_{m-1}\sum_{i=0}^n\binom{i+m-1}{m-1}+\sum_{i=0}^nH_{i+m-1}\binom{i+m-1}{m-1}\\
	&=-H_{m-1}\binom{n+m}m+\sum_{i=0}^{n-1}(H_{i+m-1}-H_{i+m})\left(\sum_{j=0}^i\binom{j+m-1}{m-1}\right)+H_{n+m-1}\sum_{i=0}^n\binom{i+m-1}{m-1}\\
	&=-H_{m-1}\binom{n+m}m-\sum_{i=0}^{n-1}\frac1{i+m}\binom{i+m}m+H_{n+m-1}\binom{n+m}m\\
	&=\binom{n+m}m(H_{n+m-1}-H_{m-1})-\frac1m\sum_{i=0}^{n-1}\binom{i+m-1}{m-1}\\
	&=\binom{n+m}m(H_{n+m-1}-H_{m-1})-\frac1m\binom{n+m-1}{m}\\
	&=\binom{n+m}m(H_{n+m-1}-H_{m-1})-\binom{n+m}m\frac{n}{m(n+m)}\\
	&=\binom{n+m}m(H_{n+m-1}-H_{m-1})+\binom{n+m}m\left(\frac1{n+m}-\frac1m\right)\\
	&=\binom{n+m}m(H_{n+m}-H_m)
\end{align*}$$
where we have again used summation by parts trick in the above deduction.

Therefore we conclude that for $\eqref{eq1}$, $LHS=S_{m,n-m}=RHS$.

# Conclusion
I was trying to figure out a proof using combinatorial techniques, and all methods didn't work until I tried summation by parts to repeatedly reduce the lower index of the combination numbers. In the course of working on the proof, I also made a somehow surprising discovery: the high-order sums of harmonic series actually has a pretty simple representation. Maybe this result has some potential use for other works.

# Generating function
So after I completed this blog, I suddenly realized this problem can actually be solved with generating function easily. (And I was dumb QwQ) First, we write the left-hand side of $\eqref{eq1}$ as
$$\begin{equation}
	\sum_{k=0}^{n-m-1}\binom{m+k-1}{m-1}\frac1{n-m-k}\label{eq2}
\end{equation}$$
Then recall that we can actually write down the generating function for the two multiplicants:
$$\begin{align*}
	\sum_{k\ge0}\binom{m+k-1}{m-1}x^k&=\frac1{(1-x)^m}=:f(x)\\
	\sum_{k\ge1}\frac1{k}x^k&=-\ln(1-x)=:g(x)
\end{align*}$$
Then by convolution, $\eqref{eq2}$ is actually equal to $[x^{n-m}](f(x)g(x))$. Define $h(x):=f(x)g(x)$, then we only need to compute $\frac1{(n-m)!}h^{(n-m)}(0)$. So from now on, we focus on the derivatives of $h(x)=\frac{-\ln(1-x)}{(1-x)^m}$.
$$\begin{align*}
	h'(x)&=\frac{-m\ln(1-x)}{(1-x)^{m+1}}+\frac1{(1-x)^{m+1}}\\
	h''(x)&=\frac{-m(m+1)\ln(1-x)}{(1-x)^{m+2}}+\frac{m+(m+1)}{(1-x)^{m+2}}\\
	&\vdots
\end{align*}$$
Let $q_i(x)$ denote the second term of $h^{(i)}(x)$. Then it's not hard to see that
$$\begin{align*}
	q_0(x)&=0\\
	q_1(x)&=\frac{1}{(1-x)^{m+1}}\\
	q_2(x)&=\frac{2m+1}{(1-x)^{m+2}}\\
	&\vdots\\
	q_{i+1}(x)&=\frac{m+i}{1-x}q_i(x)+\frac{\frac{(m+i-1)!}{(m-1)!}}{(1-x)^{i+1}}
\end{align*}$$
Then by induction, we can prove that
$$
	q_i(x)=\frac1{(1-x)^{i+1}}\frac{(m+i-1)!}{(m-1)!}\sum_{k=m}^{m+i-1}\frac1k
$$
Therefore we have
$$
	h^{(n-m)}(0)=q_{n-m}(0)=\frac{(n-1)!}{(m-1)!}\sum_{k=m}^{n-1}\frac1k
$$
And $\eqref{eq2}$ is equal to
$$
	\frac1{(n-m)!}h^{(n-m)}(0)=\binom{n-1}{m-1}\sum_{k=m}^{n-1}\frac1k
$$
which is exactly the right-hand side of $\eqref{eq1}$.