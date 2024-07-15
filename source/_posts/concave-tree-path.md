---
title: Concavity of edge/vertex-disjoint paths on trees
date: 2024-06-24 22:24:02
tags: [graph theory, tree, concavity]
categories: proofs
---

# Introduction

Suppose we have a tree with $n$ vertices, and an integer $1\le k\le n$. Each edge $e$ of the tree has weight $w(e)\in\mathbb R$, and we define the weight of a path as the sum of weight of edges on the path. Now we want to select at most $k$ **edge-disjoint** paths on the tree, and maximize the total weight of the selected paths. Another version of the problem asks one to select at most $k$ **vertex-disjoint** paths. A simple algorithm is to use dynamic programming: let $f(x,i,b)$ denote the maximum total weight by choosing $i$ paths in the subtree of $x$, where $b$ denotes whether or not $x$ itself is contained in one of the paths. However, this dynamic programming runs in time $O(n^2)$. Another solution is, by *observing* that the answer is concave with respect to $k$, one can use *alien's trick* to optimize the dynamic programming algorithm to $O(n\log n)$ complexity. The concavity is intuitive: when we are trying to add a new path, our new reward is always getting smaller. However, it's not easy to give a rigorous proof of this claim. As far as I know, this is the first full proof of such claim. (I haven't done much survey, so please email me if you have seen a proof somewhere else!)

<!--more-->

# The case of edge-disjoint paths

To begin with, we need to explore the relationship between endpoints and paths. First, we extend the definition of *endpoints* from paths to arbitrary subgraph. For any vertex $v$, let $N(v)$ denote the set of edges incident to $v$.

**Definition 1** *Let $T=(V,E)$ be a tree, and $E'\subseteq E$ be any subset of edges. We define the endpoints of $E'$, $EP(E')$, as the vertices incident to an odd number of edges, or more precisely,*
$$
	EP(E')=\{v\in V: |N(v)\cap E'|\text{ is odd}\}
$$

Because $\sum_{v\in V}|N(v)\cap E'|=2|E'|$ is even, $|EP(E')|$ is always even. In fact, $EP$ is a bijection from all subsets of $E$ to all **even** subsets of $V$. This can be proved by giving the inverse mapping: for an arbitrary even subset of vertices $V'\subseteq V$, we define $E'$ as the set of edges who split $V'$ into two odd subsets. Now we show that any subset of edges can be decomposed into edge-disjoint paths:

**Lemma 2** *Let $T=(V,E)$ be a tree, $E'\subseteq E$ be any subset of edges, and let $|EP(E')|=2k$. Then $E'$ can be decomposed into $k$ edge-disjoint paths $E'=\bigsqcup_{i=1}^k L_i$, and $EP(E')=\bigsqcup_{i=1}^k EP(L_i)$.*

**Proof sketch:** Induct on the size of the tree. Let $e\in E$ be an arbitrary edge, then $e$ splits $T$ into two subtrees $T_1=(V_1,E_1)$ and $T_2=(V_2,E_2)$. By induction, both $E'\cap E_1$ and $E'\cap E_2$ can be split into edge-disjoint paths. If $e\notin E'$, then taking the union of the two decompositions finishes the proof. Otherwise, link $e$ with two paths (possibly empty) from $T_1$ and $T_2$.<div style="text-align: right"> $\blacksquare$ </div>

We use **$k$-subset** to denote the subset of edges with exactly $k$ endpoints. Because of Lemma 2, finding the weighted maximum $k$ edge disjoint paths is equivalent to finding the weighted maximum $k$-subset. For a subset of edges $E'\subseteq E$, denote $W(E')$ as the total weight of $E'$, i.e. $W(E')=\sum_{e\in E'}w(e)$. The key to our proof is the following lemma, which shows that the change in the optimal answer is really small when we increase $k$ by $1$.

**Lemma 3** *Let $T=(V,E)$ be a tree, and let $E_k\subseteq E$ denote any weighted maximum $k$-subset, let $E_{k+1}\subseteq E$ denote any weighted maximum $(k+1)$-subset. Then $E_k$ can be extended into a weighted maximum $(k+1)$-subset by adding two endpoints from $E_{k+1}$. In other words, there exists two endpoints $u,v\in EP(E_{k+1})\setminus EP(E_k)$, such that $EP^{-1}(EP(E_k\cup\{u,v\}))$ is also a weighted maximum $(k+1)$-subset.*

**Proof:** Let $\tilde E=E_k\Delta E_{k+1}$ denote the symmetric difference between $E_k$ and $E_{k+1}$. Let $V_k=EP(E_k),V_{k+1}=EP(E_{k+1})$, and similarly let $\tilde V=V_k\Delta V_{k+1}$. By calculating the parity of vertices, one can verify that $EP(\tilde E)=\tilde V$. We define a new weight function $\tilde w:E\to\mathbb R$, such that
$$
	\tilde w(e)=\begin{cases}
		0,&e\notin\tilde E\\
		-w(e),&e\in\tilde E\cap E_k\\
		w(e),&e\in\tilde E\cap E_{k+1}
	\end{cases}
$$
Let $\tilde k=\frac{|\tilde V|}2$, then by Lemma 2, $\tilde E$ can be decomposed into $\tilde k$ edge-disjoint paths $L_1,\ldots,L_{\tilde k}$. Since each vertex in $\tilde V$ either belongs to $V_k$ or $V_{k+1}$, we can categorize the paths into 3 types:

1. Both endpoints are in $V_k$.
2. Both endpoints are in $V_{k+1}$.
3. One endpoint is in $V_k$, while the other is in $V_{k+1}$.

Since $|V_{k+1}|=2k+2$ and $|V_k|=2k$, we have $|\tilde V\cap V_{k+1}|=|\tilde V\cap V_k|+2$. Therefore there is at least one path of type 2. Without loss of generality, suppose this path is $L_1$, then we claim that $\tilde W(\bigcup_{i=2}^{\tilde k}L_i)=0$. This is because, if $\tilde W(\bigcup_{i=2}^{\tilde k}L_i)>0$, then for $E_k'=E_k\Delta(\bigcup_{i=2}^{\tilde k}L_i)$, we have $W(E_k')>W(E_k)$ and $|EP(E_k')|=2k$, contradicting the assumption that $E_k$ is the weighted maximum $k$-subset. Similarly, if $\tilde W(\bigcup_{i=2}^{\tilde k}L_i)<0$, then for $E_{k+1}'=E_{k+1}\Delta(\bigcup_{i=2}^{\tilde k}L_i)$, we have $W(E_{k+1}')>W(E_{k+1})$ and $|EP(E_{k+1}')|=2k+2$, contradicting the assumption that $E_{k+1}$ is the weighted maximum $(k+1)$-subset. Thus, we have $\tilde W(L_1)=\tilde W(\tilde E)=W(E_{k+1})-W(E_k)$. Let $E_k^*=E_k\cup EP(L_1)$, then we have $W(E_k^*)=W(E_{k+1})$. Therefore $E_k^*$ is the subset of edges we are looking for.<div style="text-align: right"> $\blacksquare$ </div>

Now it is time to state and prove our main theorem regarding concavity of the maximum weight.

**Theorem 4** *Let $T=(V,E)$ be a tree, where $w:E\to\mathbb R$ is a weight function for edges. Let $ans_k$ denote the maximum weight of $k$-subsets. Then for $0\le k\le\lfloor\frac n2\rfloor-2$, we have $ans_{k+2}-ans_{k+1}\le ans_{k+1}-ans_k$.*

**Proof:** Let $E_k$ be an arbitrary weighted maximum $k$-subset, and let $V_k$ denote its endpoints. By Lemma 3, there exists 4 vertices $v_1,v_2,v_3,v_4\notin V_k$ such that $EP^{-1}(V_k\cup\{v_1,v_2,v_3,v_4\})$ is a weighted maximum $(k+2)$-subset. We define a new weight function $w':E\to\mathbb R$ as follows:
$$
	w'(e)=\begin{cases}
		-w(e),&e\in E_k\\
		w(e),&e\notin E_k
	\end{cases}
$$
Then $W'(EP^{-1}(\{v_1,v_2,v_3,v_4\}))=ans_{k+2}-ans_k$. By Lemma 2, $EP^{-1}(\{v_1,v_2,v_3,v_4\})$ can be decomposed into 2 edge-disjoint paths $L_1$ and $L_2$. Since $W'(L_1\cup L_2)=ans_{k+2}-ans_k$, either $2W'(L_1)$ or $2W'(L-2)$ is at least $ans_{k+2}-ans_k$. Without loss of generality, assume the former case. Since $|EP(E_k\Delta L_1)|=2k+2$, we have $ans_{k+1}\ge W(E_k\Delta L_1)=ans_k+W'(L_1)\ge ans_k+\frac{ans_{k+2}-ans_k}2$. This gives $ans_{k+2}-ans_{k+1}\le ans_{k+1}-ans_k$.<div style="text-align: right"> $\blacksquare$ </div>

Now we can state our original problem, and prove it using Lemma 2 and Theorem 4.

**Corollary 5** *Let $T=(V,E)$ be a tree, where $w:E\to\mathbb R$ is a weight function for edges. Let $path_k$ denote the maximum total weight of no more than $k$ edge-disjoint paths. Then $path_k$ is concave with respect to $k$, i.e. $path_{k+2}-path_{k+1}\le path_{k+1}-path_k$ for $k\ge 0$.*

**Proof:** One can see that $path_k=\max_{i=0}^k ans_k$ where $ans_k$ is defined in Theorem 4. This means that $path_k$ is the same as $ans_k$ when $ans_k$ is increasing, and stay fixed when $ans_k$ starts decreasing. Therefore $path_k$ is also concave.<div style="text-align: right"> $\blacksquare$ </div>

# The case of vertex-disjoint paths

On the union of vertex-disjoint paths, no vertex is incident to more than 2 edges. In fact, similar to Lemma 2, we can decompose such "low degree" subset of edges into vertex-disjoint paths:

**Lemma 6** *Let $T=(V,E)$ be a tree, $E'\subseteq E$ be any subset of edges, and let $|EP(E')|=2k$. If for any vertex $v\in V$, $|N(v)\cap E'|\le2$, then $E'$ can be decomposed into $k$ **vertex**-disjoint paths $E'=\bigsqcup_{i=1}^k L_i$, and $EP(E')=\bigsqcup_{i=1}^k EP(L_i)$.*

**Proof:** By Lemma 2, $E'$ can be decomposed into $k$ **edge**-disjoint paths $E'=\bigsqcup_{i=1}^k L_i$. Since $|N(v)\cap E'|\le2$ and $EP(E')=\bigsqcup_{i=1}^k EP(L_i)$, $L_i$ must also be vertex-disjoint.<div style="text-align: right"> $\blacksquare$ </div>

We use **$k$-LD(low degree)subset** to denote the subset of edges with exactly $k$ endpoints, such that each vertex is incident to at most 2 edges in this subset. By Lemma 6, finding the weighted maximum $k$ vertex-disjoint paths is equivalent to finding the weighted maximum $k$-LDsubset. Then similar to Lemma 3, we have the following lemma for $k$-LDsubset:

**Lemma 7** *Let $T=(V,E)$ be a tree. Let $E_k$ denote any weighted maximum $k$-LDsubset, and let $E_{k+1}$ denote any weighted maximum $(k+1)$-LDsubset. Then $E_k$ can be extended into a weighted maximum $(k+1)$-LDsubset by adding two endpoints from $E_{k+1}$. In other words, there exists two endpoints $u,v\in EP(E_{k+1})\setminus EP(E_k)$, such that $EP^{-1}(EP(E_k\cup\{u,v\}))$ is also a weighted maximum $(k+1)$-LDsubset.*

**Proof Sketch:** Let $V_k$, $V_{k+1}$, $\tilde E$, $\tilde V$, $\tilde k$ and $\tilde w$ be defined as in the proof of Lemma 3. Then by Lemma 2, $\tilde E$ can be decomposed into $\tilde k$ **edge**-disjoint paths $L_1,\ldots,L_{\tilde k}$. However, these paths might not be vertex-disjoint, since the vertices can have a maximum degree of $4$ over $\tilde E$. We say that a vertex $v\in V$ is **critical** if $|N(v)\cap(\tilde E\cap E_k)|>0$ and $|N(v)\cap(\tilde E\cap E_{k+1})|>0$, in other words, it is incident to edges from both $\tilde E\cap E_k$ and $\tilde E\cap E_{k+1}$. We then modify $L_1,\ldots,L_{\tilde k}$ such that for each path $L_i$, for each non-endpoint critical vertex $v$ on $L_i$, it is connected to an edge from $E_k$ and an edge from $E_{k+1}$. To do this, each time we find a path and an critical vertex on it violating such requirment. Since the vertex is critical, it must be present on another path, connecting to edges from a different source. Then we simply swap the two paths starting from such critical vertex.

![Illustration of "swapping"](swap.png)

Now for every path $L_i$, and every critical vertex inside $L_i$, its two incident edges are from different sources. Thus for any union of paths $L'=L_{i_1}\cup\cdots\cup L_{i_d}$, both $E_k\Delta L'$ and $E_{k+1}\Delta L'$ are LDsubsets. Then similar to the proof of Lemma 3, one can argue that "adding" (XORing, to be exact) one of these paths to $E_k$ gives a weighted maximum $(k+1)$-LDsubset.<div style="text-align: right"> $\blacksquare$ </div>

Now it is time to state and prove our main theorem regarding concavity of the maximum weight, but the vertex-disjoint version.

**Theorem 8** *Let $T=(V,E)$ be a tree, where $w:E\to\mathbb R$ is a weight function for edges. Let $ans_k$ denote the maximum weight of $k$-LDsubsets. Then for any non-negative integer $k$ such that there exists a $(k+2)$-LDsubset, we have $ans_{k+2}-ans_{k+1}\le ans_{k+1}-ans_k$.*

**Proof Sketch:** We use the same set of notations as in the proof of Theorem 4. If $L_1$ and $L_2$ are vertex-disjoint, then the same proof carries on. Otherwise, they have exactly one vertex in common. Then we can use the same technique as in the proof of Lemma 7: we can swap part of the paths to make them valid for "adding".<div style="text-align: right"> $\blacksquare$ </div>

Similar to Corollary 5, we have the following corollory regarding vertex-disjoint paths.

**Corollary 9** *Let $T=(V,E)$ be a tree, where $w:E\to\mathbb R$ is a weight function for edges. Let $path_k$ denote the maximum total weight of no more than $k$ vertex-disjoint paths. Then $path_k$ is concave with respect to $k$, i.e. $path_{k+2}-path_{k+1}\le path_{k+1}-path_k$ for $k\ge 0$.*