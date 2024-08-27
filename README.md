# LEETCODE-Graphs-1514
Let's go through a dry run of the `maxProbability` function from the `Solution` class. The goal of this function is to find the maximum probability of reaching a target node (`end`) starting from a source node (`start`) in a graph represented by `n` nodes and `edges` where each edge has a success probability.

### Function Signature
```java
public double maxProbability(int n, int[][] edges, double[] succProb, int start, int end)
```

### Input Parameters
- `n`: Number of nodes in the graph.
- `edges`: A 2D array where each element is an edge represented by two integers `[u, v]` indicating an edge from node `u` to node `v`.
- `succProb`: An array where each element represents the success probability of the corresponding edge in `edges`.
- `start`: The starting node.
- `end`: The target node.

### Key Variables
- `graph`: An adjacency list where each node points to a list of pairs, with each pair consisting of a neighboring node and the probability of reaching that neighbor.
- `maxHeap`: A max-heap (priority queue) that stores pairs of probability and node. It allows us to always process the most promising (highest probability) path first.
- `seen`: A boolean array to track if a node has already been processed.

### Algorithm Steps
1. **Initialize the graph and priority queue**:
   - Create an adjacency list `graph` to represent the graph structure.
   - Initialize a max-heap `maxHeap` to prioritize nodes with higher probabilities.
   - Initialize the `seen` array to keep track of visited nodes.
   
2. **Build the graph**:
   - For each edge in `edges`, update the adjacency list `graph` with the nodes and corresponding success probabilities from `succProb`.

3. **Max-Heap based Dijkstra's-like algorithm**:
   - Start from the `start` node with an initial probability of 1.0.
   - Use a while loop to process the nodes from the max-heap until it is empty.
   - Extract the node with the maximum probability.
   - If the extracted node is the `end` node, return the probability.
   - If the node has already been seen, skip processing it.
   - Mark the node as seen.
   - For each neighbor of the current node, calculate the new probability by multiplying the current path probability with the edge's success probability.
   - Add the new probability and the neighbor node to the max-heap if the neighbor has not been seen.

4. **Return 0 if the `end` node is not reachable**:
   - If the heap is exhausted and the `end` node has not been reached, return 0.

### Dry Run Example
Let's assume the following input:
- `n = 3`
- `edges = [[0, 1], [1, 2], [0, 2]]`
- `succProb = [0.5, 0.5, 0.2]`
- `start = 0`
- `end = 2`

#### Step-by-Step Execution

1. **Initialization**:
   - `graph` is initialized as an array of lists of size `n`.
   - `maxHeap` starts with `[(1.0, 0)]`.
   - `seen` is `[false, false, false]`.

2. **Build the graph**:
   - `graph[0]` = `[(1, 0.5), (2, 0.2)]`
   - `graph[1]` = `[(0, 0.5), (2, 0.5)]`
   - `graph[2]` = `[(1, 0.5), (0, 0.2)]`

3. **Processing nodes in max-heap**:
   - **First iteration**:
     - `maxHeap`: `[(1.0, 0)]` 
     - Pop `(1.0, 0)` from `maxHeap`.
     - `u = 0`, `prob = 1.0`.
     - `seen[0] = true`.
     - Process neighbors of node 0:
       - Node 1 with `edgeProb = 0.5`: Push `(1.0 * 0.5, 1)` to `maxHeap` => `[(0.5, 1)]`.
       - Node 2 with `edgeProb = 0.2`: Push `(1.0 * 0.2, 2)` to `maxHeap` => `[(0.5, 1), (0.2, 2)]`.
   - **Second iteration**:
     - `maxHeap`: `[(0.5, 1), (0.2, 2)]`
     - Pop `(0.5, 1)` from `maxHeap`.
     - `u = 1`, `prob = 0.5`.
     - `seen[1] = true`.
     - Process neighbors of node 1:
       - Node 0 is already seen, skip.
       - Node 2 with `edgeProb = 0.5`: Push `(0.5 * 0.5, 2)` to `maxHeap` => `[(0.25, 2), (0.2, 2)]`.
   - **Third iteration**:
     - `maxHeap`: `[(0.25, 2), (0.2, 2)]`
     - Pop `(0.25, 2)` from `maxHeap`.
     - `u = 2`, `prob = 0.25`.
     - Since `u == end`, return `prob = 0.25`.

### Output
The maximum probability of reaching node `2` from node `0` is `0.25`.
