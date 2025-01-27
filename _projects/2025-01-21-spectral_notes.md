---
title: "DRAFT: Notes on Spectral Graph Theory"
collection: projects
category: mathematics
permalink: /projects/2025-01-21-spectral_notes
excerpt: 'Writing up notes from Spectral Graph Theory at Yale, with comments of my own'
date: 2025-01-21
---

# Notes on Spectral and Algebraic Graph Theory
***Mostly notes from Dan Spielman's lectures in CPSC 462 SP'25***

- **Spectral**--eigenvalues, eigenvectors
- **Algebraic**--linear equations, operators

## Lecture 1 -- Jan 15, 2025
### Preliminary Notation
Graph \\(G = (V,E)\\). Edges \\((a,b) = (b,a)\\) (undirected), no self-loops, multiedges, default weight \\(=1\\). 

Path \\(V = \\{1, ..., n\\}, E = \\{(a, a+1) s.t. 1 \leq a\\}\\). 

A ring is a path and the edge \\((1,n).\\)

Adjacency matrix will usually be denoted as \\(M\\).

blah blah blah fill out more notation
### Matrices for Graphs
Take the view of a matrix $M$ are an operator mapping a vector $x$ to $Mx$. Also define a quadratic form: the function which maps a vector $x$ to the number $x^TMx$. 

### Diffusion Operator
The diffusion operator is the "most natural" operator associated with a graph $G$. 

Define $D = \text{diag}(d) = M\cdot(\text{the vector of all 1's})$ as the diagonal matrix of degrees associated with a graph. Then the walk/diffusion matrix is

$$W = M D^{-1}.$$

Each $i,j$ entry in $W$ is nonzero with value $1/d(j)$ if vertex $i$ is adjacent to vertex $j$. Notice that if the graph is regular, e.g. each vertex has the same degree $n$, then $W$ simply rescales $M$, where nonzero entries of $M$ all have value $1/n$.

Let the vector $p\in\mathbb{R}^v$ represent how much "stuff" is at vertex in $G$, i.e., $p(a)$ is the amount of "stuff" at vertex $a$. After one timestep, the new distribution of "stuff" around $G$ is $Wp$. 

To see this, consider the case where $p$ is $\delta_a$, the unit vector where $\delta_a(a) = 1$. Then $D^{-1}\delta_a$ is $1/d(a)$ at vertex $a$, and zero elsewhere. $MD^{-1}\delta_a$ is then $1/d(a)$ at every vertex that is a neighbor of $a$, and zero elsewhere. This makes sense intuitively--when all "stuff" is concentrated at vertex $a$, it should diffuse evenly to each of $a$'s neighbors. Since any other vector $p\in \mathbb{R}^v$ is a linear combination of $\delta$'s, the general behavior described above holds. 

Note: if $p$ is a probability distribution, then $Wp$ represents the distribution of "stuff" after one step in a random walk.


### Graph Laplacian
A natural quadratic form associated with a graph is its Laplacian matrix,
$$L_G := D_G - M_G$$
where $D_G$ is the diagonal degree matrix and $M_G$ the adjacency matrix. The graph Laplacian has the degrees of $G$ on its diagonal, $-1$ for neighboring nodes, and $0$ otherwise.

Given a function on the vertices, $x$, the value of the Laplacian quadratic form of a weighted graph where an edge $(a,b)$ has weight $w_{a,b}>0$ is
$$x^TL_Gx = \sum_{(a,b)\in E}w_{a,b} (x(a)-x(b))^2.$$

This form measures the "smoothness" of the function $x \in \mathbb{R}^v$; i.e., it will be small if the function does not vary too much between the vertices connected by any edge. 

### Intuition: Graph Laplacian vs. Continuous Laplacian

Thinking of $L_G := D_G - M_G$ may not feel intuitive. An alternative line of reasoning connects the continuous Laplacian to its discretization as the graph laplacian:

In Euclidean space, the Laplace operator is the divergence of the gradient of a function:

$$ \Delta f = \nabla \cdot \nabla f = \text{div}(\text{grad}(f)).$$

How can we interpret this for graphs?

We define a graph function to be a mapping from every vertex of the graph to a value; this is the meaning of "a function on the vertices," which we defined as $x$. In particular, each entry $x_i$ of $x \in \mathbb{R}^v$ is the value the $i$-th vertex of $G$ maps to.

What about the gradient of such a graph function? 

Naturally, we can understand the discrete analogue for the derivative as the difference, with the "direction" being along each edge. So the gradient of $x$ along the edge $e = (u,v)$ is 

$$\nabla(f) |_{e} = x(u) - x(v).$$

We know the incidence matrix $K$ for a directed graph is $n \times m$, where $n$ is the number of vertices and $m$ is the number of edges such that:  

$$K = \begin{cases}
-1 & \text{if edge } e_j \text{ leaves vertex }v_i \\
1 & \text{if edge } e_j \text{ enters vertex }v_i \\
0 & \text{otherwise}.
\end{cases}
$$

For an undirected graph $G$, we can define the oriented incidence matrix as the incidence matrix for any orientation of $G$. So in the column of edge $e$, there is a $1$ in the row corresponding to one vertex of $e$ and a $-1$ in the row corresponding to the other vertex of $e$, with all other rows as $0$. The oriented incidence matrix is unique up to negation of any of the columns (since negating a column corresponds to reversing the orientation of an edge). 

We can represent the gradient of a function along each edge as 

$$\text{grad}(x) = K^T x.$$

Hence the gradient can be thought of as a different function over the edges of the graph instead of the vertices, as with $x$. 

How do we define divergence on graphs?

In Euclidean space, divergence at a point gives the net outward flux of a vector field. For graphs, the vector field is the gradient of a graph function. So we define the divergence of a function $f$ over the edges of a graph as a mapping from $f$ to $Kf$. 

Then we can define the Laplacian of a graph function as 

$$\Delta x = \text{div}(\text{grad}(x)) = KK^Tx.$$

$KK^T$ is the graph Laplacian! (Also note that being able to factor $L = KK^T$ is why $L$ is positive semidefinite.)

We can understand the gradient / divergence as related to the previous idea of diffusion and distribution of "stuff" at each vertex, which would be represented with a graph function. Intuitively, for continuous spaces, the Laplacian is the second derivative and measures how "smooth" a function is over its domain. The same holds true for graphs. A "smooth" function on graphs would be a function that doesn't change much in value between neighboring vertices. Consider the terms of the Laplacian quadratic forms again:

$$x^TL_Gx = \sum_{(a,b)\in E}w_{a,b} (x(a)-x(b))^2.$$

We can think of this as the Dirichlet energy of a function:

$$
\begin{aligned}
E(x) &= \frac{1}{2}\sum_{a,b \in E}w_{a,b}(x(a)-x(b))^2\\
&= ||K^Tx||^2 \\
&=x^t L x.
\end{aligned}
$$


We can think of this as the Dirichlet energy of a function. 


### Rayleigh Quotient

The Rayleigh quotient of $x$ with respect to matrix $M$ is \\( \frac{x^TMx}{x^Tx} \\).

*Theorem: If \\(M \\) is symmetric and \\(x\\) maximizes the Rayleigh quotient, then \\(Mx = \mu_1 x\\).*
PUT IN THE PROOF

*Theorem: Let \\(M\\) be a \\(n\times n\\) symmetric matrix and \\(x\\) maximize the Rayleigh quotient. Then \\(Mx = \frac{x^TMx}{x^Tx}x.\\)*

PUT IN THE PROOF
## Lecture 2 -- Jan 17, 2025
### Spectral Theorem
Every real symmetric \\(n \times n\\) matrix has \\(n\\) real eigenvalues and orthonormal eigenvectors (which are not necessarily uniquely defined).

### Courant-Fischer (Min-Max Theorem)

### Using Laplacian Quadratic Form to Derive Matrix Structure
The Laplacian matrix is designed to capture the Laplacian quadratic form: 

$$x^T L x = \sum_{(a,b)\in E}w_{a,b}(x(a)-x(b))^2.$$

We can now use the quadratic form to derive the structure of the matrix. 

### Hall's Spectral Graph Drawing
The idea of drawing graphs using eigenvectors was suggested by Hall.

Consider the problem of drawing a graph on a line (i.e., mapping each vertex to a real number). On a straight line, we can't see the graph well, as all the edges overlap. This problem can be fixed by drawing the edges of the graph as curves or by wrapping the line around a circle. 

Let \\(x: V \rightarrow \mathbb{R} \\) be a mapping of each vertex to a real number (can also say \\(x \in \mathbb{R}^V\\)). We would like neighboring vertices to be close to one another. For an unweighted graph, Hall suggests choosing an \\(x\\) minimizing

$$x^T L x = \sum_{(a,b) \in E} (x(a)-x(b))^2.$$

To avoid degeneracy in the solution, we require \\( \vert \vert x \vert \vert = 1\\) and \\(\sum_a x(a) = 0\\). 

## Lecture 3 -- Jan 22, 2025
### Adjacency Matrix Eigenvalues
*Last lecture*: saw Laplacian eigenvalues, showed that \\(\lambda_1 = \lambda_2\\) if and only if the graph is disconnected.

*This lecture*: look at adjacency matrix eigenvalues. If graph is \\(d\\)-regular, then \\(\mu_i = d - \lambda_i \\) and \\( \mu_1 = d\\). We are interested in the irregular case. Will show that \\(\mu_1 = \mu_2\\) if and only if the graph is disconnected, \\(\mu_u = - \mu_1\\) if and only if the graph is bipartite, and that the chromatic number \\(\leq 1 + \left \lfloor {\mu_1} \right \rfloor \\).

### Symmetric Perron-Frobenius 
For a connected, weighted graph, 
- \\(\mu_1 \\) has a strictly positive eigenvector
- \\(\mu_1 > \mu_2 \\)
- \\(\mu_1 \geq \vert \mu_n \vert \\)

## Lecture 4 -- Jan 24, 2025


## Lecture 5 -- Jan 27, 2025 