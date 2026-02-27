# Technical Documentation: Graph-Based Path Planning

## Project: ACC Self-Driving Car Competition

### Module: Path Planning

**Lead:** Favour Olaoti

---

## 1. Objective

The primary goal of this project was to develop a dynamic navigation system capable of determining the most efficient route between any two points (A, the current node, and B, the destination node) on the competition map using the provided node sequence map.

---

## 2. Methodology: BFS Path Discovery

Instead of hard-coding a single path, the environment was treated as a graph \( G(V, E) \) where:

- **V (Vertices):** The coordinates extracted from the node sequence.
- **E (Edges):** The possible transitions between nodes based on track connectivity.

### 2.1. Implementation of Breadth-First Search (BFS)

To find the optimal path from the starting node to the target, we implemented a BFS algorithm. This approach ensures that the car finds the shortest path in an unweighted graph environment.

#### The Process:

1. **Adjacency Mapping:** The node sequence from the map was converted into an adjacency list to represent which nodes are physically connected.
2. **Queue-Based Exploration:** Starting at point A, the algorithm explores all neighboring nodes at the current "depth" before moving to the next level.
3. **Backtracking:** Once point B is reached, the algorithm backtracks through the "parent" pointers to reconstruct the exact sequence of nodes for the car to follow.

---

## 3. Path Optimization & Minimization

### 3.1. Optimal Path Selection (Shortest Path Logic)

The core advantage of using BFS in our path planning module is its inherent ability to find the shortest path in an unweighted graph. Our implementation ensures that the QCar does not just find any route to the target, but the one with the least number of nodes.

#### How We Implemented Node Minimization:

1. **Level-Order Traversal:** The algorithm explores all nodes at distance \( d \) from the start before moving to \( d+1 \). This guarantees that the first time the target node B is reached, it is via the shortest possible sequence.
2. **Parent Tracking:** We maintained a parent dictionary. Once the BFS reached the goal, we reconstructed the path by backtracking. This produced a list of the minimum required nodes to reach the destination.
3. **Redundancy Elimination:** By selecting the path with the fewest nodes, we reduced the computational load on the lateral controller, as it has fewer "waypoints" to process, leading to smoother transitions between segments.

### 3.2. Efficiency in the Node Sequence

By extracting the node sequence directly from the QLabs map data, we converted a 3D environment into a 2D adjacency matrix. The BFS then operates on this matrix to return an optimized list:

- **Input:** Full map node sequence (22 nodes).
- **Output:** A refined, minimal subset of nodes representing the most direct route from A to B.

---

## 4. Technical Impact on the QCar

- **Reduced Steering Jitter:** Because the BFS provides the "least amount of nodes," the car doesn't try to micro-adjust to unnecessary intermediate points.
- **Predictable Trajectory:** The "Shortest Path" logic allows the team to predict exactly which turn the car will take at an intersection, which is vital for collision avoidance.

---

## 5. Conclusion

By implementing a BFS-based path planner, we moved beyond simple line-following. The QCar now possesses the "intelligence" to navigate from any point A to any point B on the map. This approach is scalable and robust, allowing for real-time re-routing if the target destination changes during the competition.

--- 

This documentation provides a structured overview of the path planning module developed for the ACC self-driving car competition, highlighting the methodology, implementation details, and the technical impact of the BFS algorithm on the QCar's navigation capabilities.