# Unit 1

###### Properties of Search Algorithms

**Completeness**: A search algorithm is said to be complete if it guarantees to return a solution if at least any solution exists for any random input.

**Optimality**: If a solution found for an algorithm is guaranteed to be the best solution (lowest path cost) among all other solutions, then such a solution for is said to be an optimal solution.

**Time Complexity**: Time complexity is a measure of time for an algorithm to complete its task.

**Space Complexity**: It is the maximum storage space required at any point during the search, as the complexity of the problem.

###### Types Of Search Algorithims

Based on the search problems we can classify the search algorithms into two types:

1. Uninformed Search Algorithims
2. Informed Search Algorithims

#### Uninformed Search Algorithims

Also called Blind Search Algorithims.

It does not contain any domain knowledge such as closeness, the location of the goal.

It operates in a brute-force way as it only includes information about how to traverse the tree and how to identify leaf and goal nodes.

Commonly used uninformed search algorithms:

1. **Breadth-First Search (BFS):**
    
    - _Description:_ BFS explores the search space level by level, expanding all nodes at the current depth before moving on to the next depth level.
    - _Advantages:_ Completeness (will find a solution if it exists at a finite depth), optimality (finds the shallowest solution), simplicity.
    - _Disadvantages:_ Memory-intensive for large state spaces.

2. **Depth-First Search (DFS):**
    
    - _Description:_ DFS explores the search space by going as deep as possible along one branch before backtracking. It uses a stack to manage the nodes.
    - _Advantages:_ Memory efficiency, can find solutions deep in the search space quickly.
    - _Disadvantages:_ Non-optimality (may not find the shallowest solution), may get stuck in infinite loops for infinite state spaces.

3. **Depth-Limited Search (DLS):**
    
    - _Description:_ DLS is similar to DFS but with a depth limit. It avoids infinite loops by limiting the depth of the search.
    - _Advantages:_ Memory efficiency, avoids infinite loops.
    - _Disadvantages:_ May miss solutions beyond the depth limit.

4. **Iterative Deepening Depth-First Search (IDDFS):**
    
    - _Description:_ IDDFS is a combination of BFS and DFS. It performs a series of DFS with increasing depth limits until a solution is found.
    - _Advantages:_ Retains the memory efficiency of DFS while ensuring optimality.
    - _Disadvantages:_ Some nodes are revisited at different depths.

5. **Uniform-Cost Search (UCS):**
    
    - _Description:_ UCS explores the search space by expanding the node with the lowest path cost. It uses a priority queue based on path cost.
    - _Advantages:_ Optimality, completeness for finite-cost solutions.
    - _Disadvantages:_ May be inefficient if path costs vary widely.

6. **Bidirectional Search:**
    
    - _Description:_ Bidirectional search explores the search space from both the start and goal states simultaneously and meets in the middle.
    - _Advantages:_ Can be more efficient than uninformed searches for some problems.
    - _Disadvantages:_ Requires the ability to traverse the state space backward.

#### Informed Search Algorithims

Also called a Heuristic Search Algorithims.

It uses domain knowledge.

Problem information is available which can guide the search.

Informed search strategies can find a solution more efficiently than an uninformed search strategy

A heuristic is a way which might not always be guaranteed for best solutions but guaranteed to find a good solution in reasonable time.

Informed search can solve much complex problem which could not be solved in another way.

The goal is to efficiently find a solution by prioritizing paths that are likely to lead to the goal.

Some commonly used informed search algorithms:

1. **Best-First Search:**
    
    - _Description:_ Best-First Search selects the node with the lowest heuristic value (estimated cost to the goal) for expansion. It uses a priority queue based on heuristic values.
    - _Advantages:_
	    - Can be efficient in guiding the search toward the goal, especially in the presence of good heuristics.
	    - Best first search can switch between BFS and DFS by gaining the advantages of both the algorithms.
    - _Disadvantages:_ Not optimal.

2. **A* Search (A-star):**
    
    - _Description:_ A* is an extension of Best-First Search that considers both the cost to reach a node (g-cost) and the estimated cost from that node to the goal (h-cost). The priority queue is based on the sum of g-cost and h-cost.
    - _Advantages:_ Optimality (if the heuristic is admissible), completeness.
    - _Disadvantages:_ Requires a consistent and admissible heuristic.

3. **Greedy Best-First Search:**
    
    - _Description:_ Greedy Best-First Search is similar to A* but considers only the heuristic cost (h-cost) when selecting nodes for expansion. It does not consider the cost to reach the current node.
    - _Advantages:_ Can be computationally less expensive than A*.
    - _Disadvantages:_ May not guarantee optimality.

4. __IDA* (Iterative-Deepening A*):**
    
    - _Description:_ IDA* is a memory-efficient variant of A*. It performs iterative deepening depth-first search with a depth limit determined by the f-cost (g-cost + h-cost).
    - _Advantages:_ Memory-efficient, retains optimality.
    - _Disadvantages:_ May revisit nodes.

5. **Hill Climbing:**
    
    - _Description:_ Hill Climbing is a local search algorithm that iteratively moves toward the goal by selecting the neighbor with the lowest heuristic value. It stops when it reaches a local minimum.
    - _Advantages:_ Simple and computationally efficient.
    - _Disadvantages:_ May get stuck in local optima, not guaranteed to find the global optimum.