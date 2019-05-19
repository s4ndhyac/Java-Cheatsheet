# Graphs

- Graph representation -> G = (V, E)
- V = {1,2,3,4} & E = {(1,2),(2,3),(3,4)}

## Types of graphs

- Undirected Graph: \
(x,y) = (y, x) \
max no. of edges = n(n-1)/2
- Directed Graph: \
(x,y) not equal to (y,x)
- Directed Acyclic Graph (DAG): \
Graph w/o cycles
- Multigraph vs Simple Graph \
Multi-edges between two nodes and loops allowed vs not allowed 
- Weighted vs unweighted graph
- Complete graph \
All edges that could exist are present
- Connected graph vs dis-connected graph \
Path exists between any two vertices vs doesn't exist

## Graph Representation

```
class Graph
{
    class Edge 
    {
      int src, dst;
      Edge (int src, int dst)
      {
        this.src = src;
        this.dst = dst;
      }
    }

<!-- It could be a List<Node> instead if val is not and integer or need to store other attribute information such as weights for an edge -->

    List<List<Integer>> adj = new ArrayList<>();
    public Graph(List<Edge> edges)
    {
      for(int i=0; i< edges.size(); i++)
        adj.add(i, new ArrayList<>());
      
      for(Edge e: edges)
      {
        adj.get(e.src).add(e.dst);
        // If undirected graph
        // adj.get(e.dst).add(e.src);
      }
    }
}

```

## BFS and DFS
NOTE: Always prefer iterative BFS (will need Queue anyway) and recursive DFS (will not require extra space/ stack)

### BFS (Iterative)

```


```

### Applications of BFS
- shortest path b/w two edges (path length measured in edges)
- Is a graph bipartite?
- Form MST for unweighted graph
- Find a node in a connected component of a graph
- Ford-Fulkerson method for computing max flow in a flow network
- Serialization/Deserialization of a binary tree in sorted order/ allows tree to be re-constructed in an efficient manner
- Web crawler
- Copying garbage collection, Cheney's algorithm

### DFS (Recursive)

```

```

### Applications of DFS
- Finding connected components in a graph
- Topological sorting in a DAG
- Finding 2-(edge or vertex) connected components
- Finding 3-(edge or vertex) connected components
- Finding the bridges of a graph
- Finding strongly connected components
- Finding biconnectivity in graphs
- Solving puzzles with only one solution such as mazes

## Shortest Path from source to all vertices (Djikstra's)

## Shortest Path from every vertex to every other vertex (Floyd Warshall)

## Detect cycle in a Graph (Union Find)

## MST (Prim's)

## MST (Kruskal's)

## Topological Sort

## Bridges in a Graph


