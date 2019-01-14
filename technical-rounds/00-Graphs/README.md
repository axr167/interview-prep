
# Overview of Graph Problems

In graphs vertices model entities and edges model relationships between entities. The point of graph algorithms is:

- Take things that are domain specific
- Model it into a graph
- Solve it using a generic graph algorithms

Classes of problems we can solve using graphs:

## Type 1: Shortest path problems:
  - Can be undirected or directed: This doesnt matter much and same algo can be used for either one
  - Can be unweighted or weighted: Here the distinction matters. Unweighted uses BFS; Weighted uses Dijkstra

## Type 2: Connected Components:
  - Things that has items/elements that are connected to each other. Examples include number of islands problem, Evaluate division, Photoshop's magic wand etc
  - Here we have a couple of different algorithms that we can use such as DFS

How Connected Components problems are different from Shortest Paths problems:

- Unlike shortest paths here we only need to know that a path exists. 
- We care about all the vertices unlike shortest paths problem where we have a fixed source and destination
- Weights are ignored because it is about connectivity
  - Directed/undirected graphs are a bit trickier because A may be connected to B but B may not be connected to A. In this case we say B is downstream from A or the level of B is lower than that of A.

## Type 3: Dependency Ordering
  - Problems where if there is an edge from A to B, there is NEVER an edge from B to A. This is strictly directional.
  - These are directed acyclic (no cycles between nodes) graph problems and is solved using Topological Sort
 
This is shown in the following image:
![dep ordering](https://i.imgur.com/oX4nniz.png)

- Here D depends on B and C both of which depend on A. So there are 2 ways to do D: Either do [A,B,C,D] or do [A,C,B,D]
- Similarly the graph can be represented in the form of a layering of the things that can be done in parallel. Here A is done first. B, C are in the same layer and can be done parallel. D must be done last
- It is mostly used for unweighted problems but we may have weighted variations. So we can ask something like what is the critical/slowest path (26) etc.

## Type 4: Spanning Trees

- Given a graph what is the maximum number of edges that can be deleted so the graph remains connected
- Done for undirected graphs

## Type 5: Bipartite Matching

- Aim is to have a 1:1 correspondence between set 1 and set 2
- 'n' jobs and 'n' people. A person can do only 1 job. Can we perform a matching between the 2? Problem: Not every person can do every job. Can we perform a matching given those conditions?
  - If edges have weights can you maximize weights?

  
# Algorithms

## BFS

- Create queue and add source node to it
- Have a map for distances and add {source, 0} to it
- While queue is not empty pop node
  - For all neighbors of node not in map
  - Add neighbour to map with distance node+1

**To get path length do:**

    private Map<String, Integer> shortestPath(Map<String, Set<String>> graph, String src, String destination) {
        Queue<String> q = new LinkedList<>();
        q.add(src);
        Map<String, Integer> dist = new HashMap<>();
        dist.put(src, 0);
        while(!q.isEmpty()) {
            String node = q.remove();
            for(String s: graph.get(node)) {
                if(!dist.containsKey(s)) {
                    dist.put(s, dist.get(node)+1);
                    q.add(s);
                    // OPTIONAL: When given a destination, this returns as soon as destination is found
                    if(s.equals(destination))
                        return dist;
                }
            }
        }
        return dist;
    }
    
    // Getting shortest path from map of distances. -1 if no path exists.
    int shortestPath = dist.containsKey(destination)? dist.get(destination): -1;
    
**Time complexity:**

- Time to traverse all edges of each vertex: 2 * E because each edge processed twice (A->B and B->A is same) + Processing all vertices V through the queue.
- Time complexity: 2E + V = O(V+E)
- Size of the adjacency list is V+2E so BFS is linearly processing the adjacency list.

**To save the path itself do the following:**

In the distance map add an extra attribute for parent. It should be: 
  
    Map dist <String, Pair(distance, parent)>

And when adding to dist do:

    dist.put(s, new Pair(dist.get(node)+1, node));


# Questions:

### 1. Find shortest path in a graph with multiple sources and multiple destonations

1. What if there are multiple destinations?

- Let dest = set of destinations
- BFS from source If we reach any destination in dest, return

2. What if there are multiple sources?

- Add all sources to queue, add all sources to distance map with a distance of 0
- Do regular BFS

### 2. Connected components problem

**Suppose we have greyscale colorings on a canvas and 2 pixels are similar if they lie within a threshold of 5. Try to simulate the magic wand tool in photoshop where if we click 1 pixel all surrounding pixels that are similar are selected.**

This is a connnected components problem where we have to label each pixel with its correcponding connected component. If we have the following grid:

![grid_cc_q](https://i.imgur.com/3dEGPnj.png)

We would iterate on every element and if element was not visited before we would do bfs and label it so it looked like this:

![grid_cc_r](https://i.imgur.com/osarV0R.png)


The adjacency list is defined as follows:
- 2 nodes must be adjacent
- The nodes must be below the threshold

So the list would look like this:

    [0,0]: {(0,1)}
    [0,1]: {(0,0), (0,2)}
    [0,2]: {(0,1), (1,2)}
    ...
