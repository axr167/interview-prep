
The point of graph algorithms:

- Take things that are domain specific
- Model it into a graph
- Solve it using a generic graph algorithms

Classes of problems we can solve using graphs:

1. Shortest path problems:
  - Can be undirected or directed: This doesnt matter much and same algo can be used for either one
  - Can be unweighted or weighted: Here the distinction matters. Unweighted uses BFS; Weighted uses Dijkstra

2. Connected Components:
  - Things that has items/elements that are connected to each other. Examples include number of islands problem, Evaluate division etc
  - Unlike shortest paths here we only need to know that a path exists. And unlike shortest paths where we have a source and a destination, we care about all the vertices.
  - Here we have a couple of different algorithms that we can use such as DFS


