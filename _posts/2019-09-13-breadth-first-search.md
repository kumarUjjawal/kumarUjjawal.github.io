---
layout: post
title: "Looking at Breadth First Search Through Algorithm"
date: 2019-09-13
---

Breadth-first search is one of the simplest algorithms for searching a graph or traversing a tree. In BFS, you must traverse all the nodes in layer 1 before you move to the nodes in layer 2. That is
1. First move horizontally and visit all the nodes of the current layer
2. Move to the next layer

BFS expands the frontier between the discovered and undiscovered vertices uniformly across the breadth of the frontier. To keep track of all the vertices which have been discovered BFS colors each vertex white, gray, or black. In the beginning all the vertices are white and as they get discovered turned into gray then into black. All vertices which are adjacent to black vertices have been discovered while the gray vertices might have some vertices which are undiscovered. BFS takes the input adjacency lists.

BFS algorithm constructs a breadth-first tree, initially containing only its root, which is the source vertex. Whenever a white vertex is discovered in the course of scanning the adjacency list of an already discovered vertex, the vertex and the edge are added to the tree.

Given a vertex _G_ with vertex _V_ and edge _E_ with the source vertex _s_, the BFS systematically, explores the edges of _G_ to discover every vertex that is reachable from the source vertex _s_.  BFS algorithm works on both directed and undirected graphs.

The total running time of BFS is O(V+E). Thus, breadth-first search runs in time linear in the size of the adjacency-list representation of graph _G_. The algorithm uses a first-in, first-out queue to manage the set of gray vertices.

```
BFS(G,s)
1. for each vertex u ∊ V[G] - {s}
2. 	do color[u] ⟵ White
3. 		d[u] ⟵ ∞
4.		π[u] ⟵ NIL
5. color[s] ⟵ Gray
6. d[s] ⟵ 0
7. π[s] ⟵ NIL
8. Q ⟵ ∅
9. ENQUEUE(Q,s)
10.while Q ≠ ∅
11.	do u ⟵ DEQUEUE(Q)
12. 		for each v ∊ Adj[u]
13.			do if color[v] =  Gray
14.				then color[v] ⟵ Gray
15.			         d[v] ⟵ d[u] + 1
16.			         π[v] ⟵ u
17. 			         ENQUEUE(Q,v)
18. 		color[u] ⟵ Black
```