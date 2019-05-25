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
- Tree
  - undirected acyclic graph in which any two vertices are connected by only one path. 
  - has N-1 edges where N is the no. of vertices
  - each node in a graph can have one or more parents whereas in a tree each node can have only one parent
- Back Edge
  - A back edge is an edge that connects a vertex to a vertex that is discovered before its parent
  - Presence of a back edge implies presence of an alternative path in case the parent of the vertex is removed
- Bridge
  - An edge in a graph b/w vertices u and v is called a bridge if after removing it, there will be no path left b/w them.
- Biconnected
  - It is connected i.e. it is possible to reach every vertex from every other vertex
  - Even after removing any vertex the graph remains connected
- Strong connectivity 
  - A directed graph is strongly connected if there is a directed path from every vertex to every other vertex


## Graph Representation
- Adjacency matrix -> V x V space -> 0s and 1s to represent if an edge exists b/w them
- Adjacency list -> V + E space -> list of adjacent nodes for each node -> makes more sense when the graph is sparse, if weights present then list of pairs for each node

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
public void BFS(int[][] adjList, int v, int src)
{
  boolean[] visited = new boolean[v];
  
  Queue<Integer> q = new LinkedList<>();
  q.offer(src);

  while(!q.isEmpty())
  {
    int u = q.poll();
    visited[u] = true;
    for(int e: adjList[u])
    {
      if(!visited[e])
      {
        q.offer(e);
        visited[e] = true;
      }
        
    }
  }


}

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

### DFS (Iterative)

```
public void DFS_iterative(int[][] adj, int v, int src)
{
  boolean[] visited = new boolean[v];
  Stack<Integer> s = new Stack<>();
  s.push(src);

  while(!s.isEmpty())
  {
    int u = s.pop();
    visited[u] = true;
    for(int e: adj[u])
    {
      if(!visited[e])
      {
        s.push(e);
        visited[e] = true;
      }
        
    }
  }

}
```

### DFS (Recursive)

```

public void DFS_recursive(int[][] adj, boolean[] visited, int v)
{
  visited[v] = true;
  for(int e: adj[v])
  {
    if(!visited[e])
      DFS_recursive(adj, visited, e);
  }
}

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

## MST
- spanning tree -> includes all vertices and all edges of Graph
- MST - spanning tree with minimum cost, there can be multiple
- used for approximating the travelling salesman problem

### Kruskal's MST
- Greedy approach, find the edge with least weight and add to the spanning tree
- steps:
  - Sort the graph edges wrt weights
  - Start adding edges to the MST from the edge with the smallest weight to the largest
  - only add edges if they do not form a cycle, add edges that connect disconnected components
- Union-Find method
    - Disjoint set -> set of disjoint connected components 
    - Subset -> each disjoint connected component
    - Union -> connects/join two subsets
    - Find -> finds which subset a particular component belongs in
- An MST has V-1 edges where V is the no. of vertices
- Union by rank and find by path compression together take less than O(log V) - complement each other nicely
- Complexity -> O(ElogE) (for pq formation/sorting/ordering) + O(Elog V) since no. of edge = V^2 atmost = O(ElogE)
- When graph has more no. of edges use Prim's, when graph is sparse use Kruskal's
- Use Edge class with datamembers src, dst and weights for ease else map of src, map
- Use graph class with members vertices and allEdges to write the algorithm methods
```

public void makeSet(int [] parent)
{
  //create a disjoint set of elements each with a parent pointer to itself
  for(int i=0; i< vertices; i++)
    parent[i] = i;
}

//Find by path compression
//O(log n)
public int find(int[] parent, int vertex)
{
  //find the parent pointer of the given vertex
  if(parent[vertex]!=vertex)
    return find(parent, parent[vertex]);
  return vertex;
}

//Naive Union - WC O(n)
public void union(int[] parent, int x, int y)
{
  //connect/join two disconnected sets i.e.
  //1. find the parent pointers of vertices x and y
  //2. set parent of vertex y as x
  int x_parent = find(parent, x);
  int y_parent = find(parent, y);
  parent[y_parent] = x_parent;
}

//Union by Rank - adding the smaller depth subtree to the larger
// O(log n)
class subset
{
  int parent;
  int rank;
}

public void union(subset[] subsets, int x, int y)
{
  int x_parent = find(subsets, x);
  int y_parent = find(subsets, y);

  if(subsets[x_parent].rank < subsets[y_parent].rank)
    subsets[x_parent].parent = subsets[y_parent].parent;
  else if(subsets[y_parent].rank < subsets[x_parent].rank)
    subsets[y_parent].parent = subsets[x_parent].parent;
  else
  {
    //when both are equal size assuming y is bigger
    subsets[x_parent].parent = subsets[y_parent].parent;
    subsets[y_parent].rank++;
  }
}


public void kruskalMST() 
{
  PriorityQueue<Edge> pq = new PriorityQueue<>(allEdges.size(), Comparator.comparingInt(o->o.weight));

  //add all edges to pq, sorted on weight by custom comparator
  for(int i=0; i < allEdges.size(); i++)
    pq.add(allEdges.get(i));

  //create parent
  int[] parent = new int[vertices];

  makeSet(parent);

  //To print mst
  ArrayList<Edge> mst = new ArrayList<>();

  //process (vertices-1) no. of edges, because an MST has V-1 edges for V vertices
  int index = 0;
  while(index < vertices - 1)
  {
    Edge edge = pq.remove();
    //check if adding this edge creates a cycle i.e.
    //1. find the parent pointers of the source and destination vertices of the edge
    //2. If the parent pointers are the same => both the src and dest vertex belong to the same connected component then adding an edge connecting a subset to the set will result in a cycle
    int x_set = find(parent, edge.source);
    int y_set = find(parent, edge.destination);

    if(x_set==y_set)
    {
      //ignore will create a cycle
    }
    else
    {
      //add it to the final result
      mst.add(edge);
      index++;
      union(parent, x_set, y_set);
    }
  }

  for(Edge e: mst)
  {
    System.out.println(e.toString());
  }
}

```

### Prim's MST
- Greedy approach, starting from an arbitrary vertex, find the cheapest possible connection from tree to another vertex and add vertex to tree
- for undirected graph
- Similar to Djikstra's
- Use when graph is dense with more no. of edges instead of Kruskal's
- Complexity: O((V+E)logE)
- Steps:
  - Maintain two disjoint sets of vertices. One containing the vertices in the growing spanning tree and other not in it
  - Select the cheapest vertex that is connected to the growing spanning tree and is not in the growing spanning tree and add it to the growing spanning tree
  - Check for cycles
- use adjacencyList and priorityQueue

```

static class ResultSet
{
  int parent;
  int weight;
}

public void primMST()
{
  boolean[] mst = new boolean[vertices];
  ResultSet[] resultSet = new ResultSet[vertices];
  int[] key = new int[vertices];


  //initialize all keys to INF
  //initialize resultSet for all vertices
  for(int i=0; i<vertices; i++)
  {
    key[i] = Integer.MAX_VALUE;
    resultSet[i] = new ResultSet();
  }


  //Initialize priorityQueue with custom comparator based on key of the pair in the priorityQueue
  PriorityQueue<Pair<Integer,Integer>> pq = new PriorityQueue<>(vertices, new Comparator<Pair<Integer,Integer>>()
  {
    @Override
    public int compare(Pair<Integer, Integer> p1, Pair<Intger,Integer> p2)
    {
      return p1.getKey() - p2.getKey();
    }
  });

  //create pair for the first starting arbitrary index 0
  //Pair -> cost = 0, index = 0
  //key array storing the cost of path to that index vertice
  key[0] = 0;
  //key is the cost and value is the vertice index
  Pair<Integer, Integer> p1 = new Pair<>(key[0], 0);
  pq.offer(p1);

  resultSet[0] = new ResultSet();
  resultSet[0].parent = -1;

  while(!pq.isEmpty())
  {
    //extract min
    Pair<Integer, Integer> extractedPair = pq.poll();

    //get the vertice index of the extracted pair
    int extractedVertex = extractedPair.getValue();

    //set the extracted vertex as visited
    mst[extractedVertex] = true;

    //get the adjacent edges to the extractedVertex
    ArrayList<Edge> list = adjacencyList[extractedVertex];
    
    for(int i=0; i<list.size(); i++)
    {
      Edge edge = list.get(i);
      //if the destination vertex of the edge is not present in the mst
      if(mst[edge.destination]==false)
      {
        //check if the weight of edge to destination is less than the current cost/weight of path to destination in key array then update it
        if(edge.weight < key[edge.destination])
        {
          //add the new cost to the pq
          Pair<Integer,Integer> p = new Pair<>(edge.weight, edge.destination);
          pq.offer(p);

          //update resultSet for the destination vertex
          resultSet[edge.destination].parent = extractedVertex;
          resultSet[edge.destination].weight = edge.weight;

          //update the key array
          key[edge.destination] = edge.weight;

        }
      }
    }
  }

  printMST(resultSet);

}

```

## Shortest Path Algorithms

### Bellman Ford's Algorithm
- shortest path from a source vertex to all other vertices in a weighted graph
- shortest path contains at most V-1 edges since it can not have a cycle
- can detect a negative cycle and report it
- based on the relaxation principle
- Steps:
  - initially dist. of all vertices from source is INF and source is 0
  - Outer loop runs 1 to V-1 times
  - Inner loop checks for each edge if dist. of source vertex to u + uv < dist of source to v and updates the dist of uv if true
  - check if -ve weight cycle exists
    - After running the bellman-ford algorithm as above to get the shortest path, check if another round around the cycle gives a lower cost than the current cost i.e.
    - loop over all the edges again and check if dist. of source to u + uv < dist of source to v
  - if yes then there exists a -ve cycle in the graph
- Complexity: WC: O(VE)

```

void BellmanFord(Graph graph, int src)
{
  int V = graph.V, E = graph.E;
  int dist[] = new int[V];

  //Initialize distances from src to all other vertices as INF
  for(int i=0; i<V; i++)
    dist[i] = Integer.MAX_VALUE;
  dist[src] = 0;

  //Relax edges
  for(int i=1; i<V; i++)
  {
    for(int j=0; j<E; j++)
    {
      int u = graph.edge[j].src;
      int v = graph.edge[j].dest;
      int weight = graph.edge[j].weight;
      if(dist[u]!=Integer.MAX_VALUE && dist[u] + weight < dist[v])
        dist[v] = dist[u] + weight;
    }
  }

  //check for -ve cycles
  for(int j=0; j< E; j++)
  {
    int u = graph.edge[j].src;
    int v = graph.edge[j].dest;
    int weight = graph.edge[j].weight;
    if(dist[u]!=Integer.MAX_VALUE && dist[u] + weight < dist[v])
      System.out.println("Graph has -ve cycle");
  }

}

```

### Djikstra's Algorithm
- greedy algorithm
- shortest path for source to all other vertices for weighted undirected graph
- can also be used to find shortest path from src to dest by stopping once we find shortest path to dest.
- Similar to Prim's: In Prim's we build MST, in Djikstra's we build shortest path tree from the given source node
- Steps:
  - initialize distance of all vertices from source to INF
  - set distance of source vertex as 0
  - make a pair of (distance, vertex) (0,0) and add to pq
  - while there are elements in the pq
    - poll the vertex from the pq
    - mark it as visited
    - for each edge in the adjList, if the destination vertex is not visited and dist. of source + uv < dist. to v, then update the dist. and add it to pq
- Complexity: ElogV

```
public void dijkstra(int sourceVertex){

      boolean[] SPT = new boolean[vertices];
      //distance used to store the distance of vertex from a source
      int [] distance = new int[vertices];

      //Initialize all the distance to infinity
      for (int i = 0; i <vertices ; i++) {
          distance[i] = Integer.MAX_VALUE;
      }

      //Initialize priority queue
      //override the comparator to do the sorting based keys(distance from source vertex)
      PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<>(vertices, new Comparator<Pair<Integer, Integer>>() {
          @Override
          public int compare(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) {
              //sort using distance values
              int key1 = p1.getKey();
              int key2 = p2.getKey();
              return key1-key2;
          }
      });
      //create the pair for for the first index, 0 distance 0 index
      distance[0] = 0;
      Pair<Integer, Integer> p0 = new Pair<>(distance[0],0);
      //add it to pq
      pq.offer(p0);

      //while priority queue is not empty
      while(!pq.isEmpty()){
          //extract the min
          Pair<Integer, Integer> extractedPair = pq.poll();

          //extracted vertex
          int extractedVertex = extractedPair.getValue();
          if(SPT[extractedVertex]==false) {
              SPT[extractedVertex] = true;

              //iterate through all the adjacent vertices and update the keys
              LinkedList<Edge> list = adjacencylist[extractedVertex];
              for (int i = 0; i < list.size(); i++) {
                  Edge edge = list.get(i);
                  int destination = edge.destination;
                  //only if edge destination is not present in mst
                  if (SPT[destination] == false) {
                      ///check if distance needs an update or not
                      //means check total weight from source to vertex_V is less than
                      //the current distance value, if yes then update the distance
                      int newKey =  distance[extractedVertex] + edge.weight ;
                      int currentKey = distance[destination];
                      if(currentKey>newKey){
                          Pair<Integer, Integer> p = new Pair<>(newKey, destination);
                          pq.offer(p);
                          distance[destination] = newKey;
                      }
                  }
              }
          }
      }
      //print Shortest Path Tree
      printDijkstra(distance, sourceVertex);
  }
```


### Floyd Warshall's Algorithm
- shortest path b/w all pairs of vertices in a graph where each edge in the graph can have either a +ve/-ve weight
- Complexity: O(V^3)
- Steps:
  - intialize shortest path b/w any two pairs as INF
  - for each pair of nodes add each node as intermediate node and minimize the distance b/w them

  ```
    void floydWarshall(int graph[][]) 
    { 
        int dist[][] = new int[V][V]; 
        int i, j, k; 
  
       //Initialize the dist b/w pairs
        for (i = 0; i < V; i++) 
            for (j = 0; j < V; j++) 
                dist[i][j] = Integer.MAX_VALUE; 
  
        // Add all vertices one by one to the set of intermediate vertices
        for (k = 0; k < V; k++) 
        { 
            // Pick all vertices as source one by one 
            for (i = 0; i < V; i++) 
            { 
                // Pick all vertices as dest. for the above picked source 
                for (j = 0; j < V; j++) 
                { 
                    // If vertex k is on the shortest path from 
                    // i to j, then update the value of dist[i][j] 
                    if (dist[i][k] + dist[k][j] < dist[i][j]) 
                        dist[i][j] = dist[i][k] + dist[k][j]; 
                } 
            } 
        } 
  
        // Print the shortest distance matrix 
        printSolution(dist); 
    } 

  ```

## Topological Sort
- Is an ordering of vertices in a directed graph such that if there is an edge directed from vi towards vj then vi comes before vj
- In order to have a topological sort ordering of vertices, the directed graph must not have a cycle
- the graph must be a DAG
-  Complexity: O(V+E)
- Use a stack to maintain the topological sort order

```

DFS(int[][] adj, Stack<Integer> s, boolean[] visited, int v)
{
  visited[v] = true;
  for(int e: adj[v])
  {
    if(!visited[e])
      DFS(adj, s, visited, v);
  }
  s.push(v);
}

topologicalSort(int[][] adj, int v)
{
  boolean[] visited = new boolean[v];
  Stack<Integer> s = new Stack<>();

  for(int i=0; i<v; i++)
  {
    if(!visited[i])
      DFS(adj, s, visited, i);
  }

  printTopologicalSort(s);

}

```

## Hamiltonian Path
- A path in a directed/undirected graph that visits each vertex exactly once
- Problem to find whether a graph contains a hamiltonian path is NP-complete => problem to find hamiltonian path in a graph is also NP-complete
- One way of getting around is -> to instead verify
  - check all possible permutations of paths in a graph, which path visits each node only once
  - that path is the hamiltonian path
  - complexity O(N*N!)
- DFS and backtracking:
  - Do a DFS starting from each vertex and label the vertices IN_STACK if it is visted and some adj vertices not visited or otherwise NOT_IN_STACK if it is not visited.
  - If at any point the no. of vertices in the IN_STACk is same as total no. of vertices then hamiltonian path is found.
  - complexity - O(N!)

## Ford-Fulkerson Algorithm
- Given: Source node, sink node, each edge has a capacity
  - for any non-source, non-sink node, input flow = output flow
  - for any edge E, flow(E) >= 0, flow(E) <= Capacity(E)
  - Total flow out of source node = total flow into sink node
  - Flow(u,v) = -Flow(v, u)
- To Find: Maximum flow between source and sink
- Input: graph, source, sink
- Proof:
  - initialize a residual graph same as original graph
  - 

```
public int findMaxFlow(int source, int sink) {
    //residual graph
    int[][] residualGraph = new int[vertices][vertices];

    //initialize residual graph same as original graph
    for (int i = 0; i <vertices ; i++) {
        for (int j = 0; j <vertices ; j++) {
            residualGraph[i][j] = graph[i][j];
        }
    }

    //initialize parent [] to store the path Source to destination
    int [] parent = new int[vertices];

    int max_flow = 0; //initialize the max flow

    while(isPathExist_BFS(residualGraph, source, sink, parent)){
        //if here means still path exist from source to destination

        //parent [] will have the path from source to destination
        //find the capacity which can be passed though the path (in parent[])

        int flow_capacity = Integer.MAX_VALUE;

        int t = sink;
        while(t!=source){
            int s = parent[t];
            flow_capacity = Math.min(flow_capacity, residualGraph[s][t]);
            t = s;
        }

        //update the residual graph
        //reduce the capacity on fwd edge by flow_capacity
        //add the capacity on back edge by flow_capacity
        t = sink;
        while(t!=source){
            int s = parent[t];
            residualGraph[s][t]-=flow_capacity;
            residualGraph[t][s]+=flow_capacity;
            t = s;
        }

        //add flow_capacity to max value
        max_flow+=flow_capacity;
    }
    return max_flow;
}

public boolean isPathExist_BFS(int [][] residualGraph, int src, int dest, int [] parent){
    boolean pathFound = false;

    //create visited array [] to
    //keep track of visited vertices
    boolean [] visited = new boolean[vertices];

    //Create a queue for BFS
    Queue<Integer> queue = new LinkedList<>();

    //insert the source vertex, mark it visited
    queue.add(src);
    parent[src] = -1;
    visited[src] = true;

    while(queue.isEmpty()==false){
        int u = queue.poll();

        //visit all the adjacent vertices
        for (int v = 0; v <vertices ; v++) {
            //if vertex is not already visited and u-v edge weight >0
            if(visited[v]==false && residualGraph[u][v]>0) {
                queue.add(v);
                parent[v] = u;
                visited[v] = true;
            }
        }
    }
    //check if dest is reached during BFS
    pathFound = visited[dest];
    return pathFound;
  }
}
```

## Types of problems and applications:
- Detect cycle in a undirected graph
  - Topological Sort
  - Union Find
  - DFS
- Detect cycle in a directed graph
  - Topological Sort with two arrays to track visited and if present on recursion stack
  ```
  public boolean isCycle() {
      boolean visited[] = new boolean[vertices];
      boolean recursiveArr[] = new boolean[vertices];

      //do DFS from each node
      for (int i = 0; i < vertices; i++) {
          if (isCycleUtil(i, visited, recursiveArr))
              return true;
      }
      return false;
  }

  public boolean isCycleUtil(int vertex, boolean[] visited, boolean[] recursiveArr) {
            visited[vertex] = true;
            recursiveArr[vertex] = true;

            //recursive call to all the adjacent vertices
            for (int i = 0; i < adjList[vertex].size(); i++) {
                //if not already visited
                int adjVertex = adjList[vertex].get(i);
                if (!visited[adjVertex] && isCycleUtil(adjVertex, visited, recursiveArr)) {
                    return true;
                } else if (recursiveArr[adjVertex])
                    return true;
            }
            //if reached here means cycle has not found in DFS from this vertex
            //reset
            recursiveArr[vertex] = false;
            return false;
        }
  ```

  ### Sources
- https://medium.com/@codingfreak/graph-data-structure-interview-questions-and-practice-problems-22d5cd488855
- https://www.hackerearth.com/practice/algorithms/graphs/articulation-points-and-bridges/tutorial/
- https://algorithms.tutorialhorizon.com/


