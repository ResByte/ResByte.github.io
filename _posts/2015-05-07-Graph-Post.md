---
layout: post
title: "BGL: The graph library"
date: 2015-05-07
---
<p>Graphs are one of the abstract forms to represent connected things. These are used to represent Social Networks, friendships we have with other people. These can also be used to represent transport systems across country. Hence, these play a vital role in analyzing the data with connections to each other.</p>
<p>In general, Graphs are $$G =(V,E) $$ where V represents vertices and E represents edges connecting vertex. Based on the properties of V and E there are different kinds of graphs, such as directed or undirected graph, cyclic or acyclic graph, weighted or simple graph. </p> 
<p>The data structure to represent these graphs are of two forms, Adjacency Matrix representation and other is Adjacency List representation. Matrix representation is used when the number of edges is much larger than number of vertex and List representation in other cases. Each of the representation has its own benefit but mostly adjacency list is used to denote graph. Therefore, in this post only adjacency list is described.Boost has one of the most widely used graph library. Using Boost Graph Library(BGL), the graph as an adjacency list is described in
{% highlight cpp %}
#include "boost/include/adjacency_list.hpp"
{% endhighlight %}	
In order to create an undirected graph, we use the above as 
</p>
{% highlight cpp lineos %}
#include <boost/include/adjacency_list.hpp>
typedef boost::adjacency_list<listS,vecS,undirectedS> Graph;
int main(){
	Graph graph;
	add_edge(0,1,graph);
	add_edge(1,2,graph);
	add_edge(2,3,graph);
	add_edge(1,3,graph);
	add_edge(0,2,graph);	
	
}
{% endhighlight %}	
