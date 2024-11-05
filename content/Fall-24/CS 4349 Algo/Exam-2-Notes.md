> [!important]- Exam 2 Topics
> Greedy Algorithms

# Greedy Algorithms

- They always choose the locally optimal choice, and in total can sometimes produce the mathematically optimal solution globally.

## Activity Selection Problem

- Define
  - Activities ai and aj are compatible if the intervals (si,fi) and (sj,fj) do not overlap
- Goal:
  - Schedule activities that require exclusive use of a shared resource, and find the maximum-size set of compatible activities
- Input:
  - A set S of activities
  - Each activity has a start time si and a finishing time fi where 0 < si < fi < inf

## Huffman Codes

- A binary code that assigns a binary number for every character. using such we can make an optimal prefix code that is used for lossless data compression
- Using variable length codes where the more frequent characters are stored with more bits
  - 1 = a , 10 = b, 11 = c ...
- Algorithm to store data - Starting with the two least frequent characters, merge them in a tree, then recurse - You will end up with a tree of the most common character as the left child, and all other characters down the right child - When you go down one layer, the left child will be the second more common, with the right child having all characters except the two most frequent - We can use this tree to encode the variable length binary numbers for all the characters - This ENSURES that no characters have a prefix that is another character. they create an unambiguous set - For full alphabets, the last letters are very long in bit length, but since the common letters are 1,2,3 bits long, the total file is compressed 20-90%
  > [!example]- Huffman Tree Example
  >
  > - Given Text: ABC BB AAA AAA D DZ BB
  > - Frequency sort {A,B,C,D,Z}
  > - Fixed Length
  >   - Assign each character a number {001,010,011,100,101}
  > - Variable Length
  >   - Assign each character a number (remove leading 0s) {1, 10, 11, 100}
  > - Replace all characters in the text with their binary representation

# Graphs

> [!info]- Graph Terminology
>
> - A graph G = (V, E) where V is the set of vertices, and E is the set of edges
> - Can be Directed or Undirected
> - Can be Weighted or Unweighted
> - Can be Simple:
>   - at most 1 edge per vertex pair
>   - G has no loops
>   - G is undirected
> - Two vertices connected by an edge are considered neighbors
> - In the edge u -> v. U is the predecessor of V, and V is the successor of U
> - Subgraph: All elements and vertices within the subgraph are apart of a subset of the original vertices V and original edges E
> - Walk: A sequence of edges where each successive pair of edges share a vertex
> - Path: A walk where each vertex is used at most once
> - Cycle: A walk that starts and ends at the same vertex
> - Source Vertex: Any vertex with a indegree of 0

- Can be represented by Adjacency Matrices
  - a 2d array with a 1 for connected, 0 for not
  - a 2d array with weight for connected, inf for not connected
- Can be represented by Adjacency Lists
  - a Linked List of Linked Lists showing the edges

## Topological Ordering

> [!info]- Topological Sorting Info
>
> - A Linear ordering of a set of vertices such that if u -> v is an edge then u is placed before v (u < v)
>   - Where a walk is possible that does not repeat vertices, and only touches a vertex after all smaller vertices have already been visited
> - Only works on DAGs
>   - Cycles prevent an ordering where you can linearly follow

> [!example]- DFS Topo Pseudocode
>
> - DFS(Graph)
>   - for vertex in V
>     - if vertex unvisited:
>       - DFS_Visit(vertex)
> - DFS_Visit(vertex):
>   - mark vertex visited
>   - for u in adjacency\[vertex]
>     - DFS_Visit(U)
> - Output
>   - u is the root
>     - w3<-w2<-w1<-u->(u1->u2->u3->u4)<-v
>   - u, w1, w2, w3, v, u1, u2, u3, u4

- DFS Based Topo
  - Have a recursive call for visit which checks all adjacent vertices to the current vertex
  - global time variable to let vertices know what order they are in
    - Each vertex has a property of finishingTime which is set to the global t
    - after we visit a vertex we append to our topo ordering
    - t += 1
  - Because in this example we are marking before the recursive call, the returned list will be in reverse order
  - Choosing which vertex is arbitrary as long as you follow the rules about prerequest
- Longest Path in a DAG
  - The longest path in a weighted graph between two vertices S and T is defined as the path that has the maximum total weight of edges among all possible paths from S to T
  - using the DFS based TOPO we can find the longest path by finding all paths and then sorting by descending order.
  - Can use an array of distances to each vertex
    - have an array of distances, where the source is 0, and distance\[v2] = v1 + distance(v1,v2)
    - Longest path ending at vertex v is of distance\[v]

## Minimum Spanning Tree (MST)

- An undirected graph that connects all vertices with the minimum number of edges (V - 1)
  - MSTs are acyclic, non-unique, and fully-connected and have the minimum weight
- Starting from an arbitrary vertex traverse using DFS. if there is a cycle remove an edge as long as the graph stays connected
- Starting from an empty graph, add an edge to the set as long as it doesn't create a cycle.
  - An edge that keeps the graph as a subset of an MST and not creating a loop, it is called a _safe edge_
- We add safe edges until we have an MST

### Finding a Safe Edge

- Cut:
  - A cut is a partition of V into two sets (S, V-S) V ∩ S = { }
  - Choosing the cut is not unique and is not important for finding the MST
- Cross:
  - An edge crosses the cut if on vertex is in V and one is in S
- Respect:
  - A cut respects a set A of edges if no edge in A crosses the cut
- Light Edge
  - An edge is a light edge if it crosses a cut, and has the minimum weight of any edge crossing the cut
- Steps
  1.  Let A subset of MST
  2.  A cut is made that respects A
  3.  (u,v) is the lightest edge crossing said cut
  4.  in this case (u,v) is a safe edge
- Keep an array called color which keeps track of which vertices are already visited and apart of A
- Use a priority queue

### Prim's Algorithm

- Start from an arbitrary vertex to be the root. While you don't have an MST, add any vertex that leaves the tree
- Using the Cut-light edge method from previous section, grows a tree one edge at a time
- There is only ever one tree in Prim's with its reaching leaves

### Kruskal's Algorithm

- Start from an empty canvas adding edge by edge until you have an MST, also using the Cutting of light edges method
- At each step the algorithm adds the cheapest edge that does not form a cycle
  - The difference between Prim's and Kruskal, prim always grows a single tree, Kruskal can grow many fragments before creating a single MST.
  - The initial phase with empty vertices is called the forest
- Steps
  1.  select an edge with the lowest weight and verify it doesn't form an edge 2. if it doesn't form an edge add it to the graph 3. if it does, discard the edge
- How does algorithm detect a cycle
  - Every vertex / subset forms a tree. This means any edge added to that tree would form a cycle. This means if the edge crosses between two subsets, the edge is valid, otherwise it forms a cycle and can be discarded.
  - if FIND_SET(u) != FIND_SET(v) { UNION(u,v) }

## Single Source Shortest Path

- Triangle Property
  - δ(s,v) <= δ(s,u) + w(u,v)
- Upper bound property
  - v.d >= δ(s,v)
  - once v.d reaches δ(s,v) it never changes
  - if v.d = inf at the end of runtime, there is no path from s to v
- Path Relaxation Property
  - If we relax vertices in order they show in the path, starting from the source to the leaf node, we are guaranteed v.d = δ(s,v)

### Bellman-Ford

- Single source to anywhere shortest path (SSSP), that allows _negative edges_
- if there is a negative cycle the algorithm detects it, otherwise it updates all v.d and v.pi
