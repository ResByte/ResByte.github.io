---
layout: post
title: "BGL: The graph library"
date: 2015-05-07
---
<p>Graphs are one of the abstract forms to represent connected things. These are used to represent Social Networks, friendships we have with other people. These can also be used to represent transport systems across country. Hence, these play a vital role in analyzing the data with connections to each other.</p>
<p>In general, Graphs are $$G \in <V,E> $$ where V represents vertices and E represents edges connecting vertex. Based on the properties of V and E there are different kinds of graphs, such as directed or undirected graph, cyclic or acyclic graph, weighted or simple graph. </p> 
<p>The data structure to represent these graphs are of two forms, Adjacency Matrix representation and other is Adjacency List representation. Matrix representation is used when the number of edges is much larger than number of vertex and List representation in other cases. Each of the representation has its own benefit but mostly adjacency list is used to denote graph. Therefore, in this post only adjacency list is described.</p>
$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$
