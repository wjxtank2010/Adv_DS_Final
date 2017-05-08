Final project:
Implement Euler-Tour Trees and Link-Cut Trees and compare their performance for dynamic connectivity on trees. (Worse would be to implement just one of them.)

Write up:
Dynamic Connectivity: V fixed, E can be changed
Operations: 1.Add an edge 2.delete an edge
Goal: "quickly" answer is there a path between x and y?

Algorithm:
Preparation: Divide the graph G into lgn different spanning forest subgraphs based on the edge level(from 0 to lgn), Gi is the subgraph consisting of edges that are at level i or less =>  Glgn = G,  Fi will be the minimum spanning tree(based on edge levels). 

Invariant 1: Every connected components of Gi has at most 2^i vertices(could be more than 1 component in Gi)
Invariant 2: F0 ⊆ F1 ⊆ . . . ⊆ Flgn. In other words, Fi = Flgn & Gi, and Flg n is the minimum spanning forest of Glgn, using edge levels as weights.
quertion: will there be a case that Fi isn't the minimum spanning tree for Gi(Fi didn't span all the connected vertices)?
keep an ajancency matrix for each Gi

Operation:

Insert(e=(v,w)):
	1. Set level(e) = lgn, update adjacency lists of v and w(only need to update Glgn matrix)
	2. If v and w are in separate connected component of Flgn, then we add e to Flgn(we can tell this by calling FINDROOT on v and w) 

For insert, FINDROOT and Link operations are needed. 

Delete(e=(v,w)):
	1. Remove e from the adjacency lists of v and w. (takes lgn * lgn since each removal takes lgn and there are up to lgn levels to remove from)
	2. If e in Flgn:
		- delete e from Fi for i >= level(e) (Since F0 ⊆ F1 ⊆ . . . ⊆ Flgn, an edge in level i should also in Fk(k>=i))
		- look for a replacement edge to reconnect v and w(why do we need a replacement?)
			for level e to lgn:
				Let Tv be the tree containing v and Tw the tree containing w.
				Relabel v and w so that |Tv|<= |Tw|, by Invariant 1 we know that |Tv|+|Tw|<= 2^i  =>  2|Tv|<=|Tv|+|Tw|<= 2^i  =>  |Tv| <= 2^(i-1), which means we can push all the edges of Tv down to level i-1
				for each edge e' = (x,y) with x in Tv, and level(e') = i
					if y is in Tw: add(x,y) to Fi,Fi+1,...Flgn and stop
					else: set level(e') to i-1

For delete, CUT and LINK operations are needed.

Connected(v,w):
	FINDROOT(v) and FINDROOT(w) in Flgn to see if they are connected.



Euler tree tour:
Known: record Euler tour path in BST, do reconcatenation for link and cut.
Question:
1. how to represente Euler tour in BST?
one node for each time a node in the represented tree was visited, keyed by the time of visite
2. what if there is a cycle in the graph?

link-cut tree: