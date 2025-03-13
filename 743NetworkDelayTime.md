# Network Delay Time - Solution Explanation

## Problem Statement
You are given a **directed weighted graph** with `n` nodes (labeled `1` to `n`). The edges are represented as `times[i] = (u, v, w)`, meaning there is a **directed edge** from `u` to `v` with a travel time `w`.

You need to find the **minimum time** for all nodes to receive a signal sent from a given starting node `k`. If some nodes **cannot** receive the signal, return `-1`.

---

## Approach
The problem can be solved using **Dijkstra’s Algorithm**, which is efficient for finding the shortest path in a **weighted directed graph**.

### Steps to Solve the Problem:
1. **Build an Adjacency List:** Convert the `times` list into a graph representation.
2. **Use Dijkstra’s Algorithm:** Use a **min-heap (priority queue)** to always expand the node with the shortest known distance.
3. **Track distances:** Maintain a dictionary to store the shortest time taken to reach each node.
4. **Return the maximum shortest distance:** The final result is the longest shortest distance among all reachable nodes.

---

## Adjacency List Representation
Instead of using an adjacency matrix (which takes O(N²) space), we use an **adjacency list** to store only the **existing edges** efficiently.

```python
from collections import defaultdict

graph = defaultdict(list)
for u, v, w in times:
    graph[u].append((v, w))  # u → (v, weight w)
```

### Example:
If `times = [[2,1,1], [2,3,1], [3,4,1]]`, the adjacency list will be:
```python
{
    2: [(1, 1), (3, 1)],
    3: [(4, 1)]
}
```

---

## Implementation
```python
from typing import List
import heapq  # Importing heapq for priority queue (min-heap)

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # Step 1: Create an adjacency list to store the graph
        graph = {}  # Dictionary to store edges in {node: [(neighbor, time), ...]} format
        for u, v, w in times:  # Iterate through all edges
            if u not in graph:
                graph[u] = []  # Initialize the list if node is not in the dictionary
            graph[u].append((v, w))  # Add the directed edge (destination, weight)
        
        # Step 2: Use a priority queue (min-heap) to process nodes with the shortest time first
        pq = [(0, k)]  # Heap stores tuples (time to reach, node), starting with (0, k)
        dist = {}  # Dictionary to store the shortest time to each node
        
        # Step 3: Implement Dijkstra's Algorithm
        while pq:  # Continue while the heap is not empty
            time, node = heapq.heappop(pq)  # Get the node with the shortest time
            
            if node in dist:
                continue  # If the node is already visited, skip it (shortest time already found)
            
            dist[node] = time  # Store the shortest time to reach this node
            
            if node in graph:  # Process the neighbors of the current node
                for neighbor, weight in graph[node]:  # Iterate through connected nodes
                    if neighbor not in dist:  # If neighbor is not visited
                        heapq.heappush(pq, (time + weight, neighbor))  # Push updated time into heap
        
        # Step 4: Check if all nodes are reachable
        if len(dist) == n:  # If we reached all nodes, return the max time taken
            return max(dist.values())  # The maximum time among all shortest paths
        return -1  # If some nodes are unreachable, return -1
```

---

## Time Complexity Analysis
### **Building the Graph**
- We iterate over `times` once: **O(E)**, where `E` is the number of edges.

### **Dijkstra’s Algorithm**
- Each node is processed **once**: O(N) in the worst case.
- Each edge is **relaxed** once: O(E log N) (log N comes from using a heap).

### **Total Complexity**
**O(E log N)** (Efficient for large graphs)

---

## Space Complexity Analysis
- **Adjacency List:** O(N + E)
- **Min-Heap:** O(N)
- **Distance Dictionary:** O(N)
- **Total Space:** **O(N + E)**

---

## Edge Cases Considered
1. **Single Node Graph** → No edges, return `0`.
2. **Disconnected Nodes** → If some nodes are not reachable, return `-1`.
3. **Multiple Paths** → Algorithm ensures the shortest path is chosen.
4. **Large `n` (100) & `E` (6000)** → Handled efficiently using Dijkstra’s Algorithm.

---

## Summary
✅ **Dijkstra’s Algorithm** is optimal for this problem.  
✅ **Adjacency List** ensures efficient graph representation.  
✅ **Min-Heap (Priority Queue)** helps process shortest paths first.  
✅ **O(E log N) complexity** ensures it runs efficiently for large inputs.  

---

### 🚀 **Final Thoughts**
This problem is a classic example of **shortest path algorithms in graphs**, and learning **Dijkstra’s Algorithm** is crucial for solving similar problems in real-world applications like **network routing and traffic systems**.

