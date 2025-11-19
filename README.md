# CI2025_lab3

Shortest Path Algorithm Comparison: Bellman-Ford vs. A* Search

1. Overview

This project implements and compares two fundamental shortest path algorithms—Bellman-Ford (BF) and the A* Search algorithm—across various graph structures to evaluate their performance in terms of time efficiency and optimality.

The graphs are generated randomly with controllable parameters, including graph size, edge density, the presence of negative edge weights, and noise level. The Bellman-Ford algorithm serves as the ground truth (baseline) for the shortest path cost, while the A* algorithm uses an admissible Euclidean distance heuristic to guide its search and improve performance.

The primary goal is to demonstrate:

A*'s optimality when using an admissible heuristic.

A*'s superior time efficiency, especially on large, dense graphs.

Both algorithms' ability to handle non-negative weights, with BF uniquely capable of handling negative weights (as long as no negative cycles exist on the shortest path).

2. Methodology and Code Explanation

Graph Generation (create_problem)

The graph generation function creates a weighted, directed graph (represented by a cost matrix) and assigns 2D coordinates to each node.

Cost Calculation: The base cost between two nodes is determined by the Euclidean distance between their coordinates, scaled by 1,000.

Density: Controls the probability that an edge exists between two nodes. Low density creates sparse graphs.

Noise Level: A small random value is added to the edges, creating non-uniform edge weights.

Negative Values: When enabled, the base random weights are scaled into the range [-1, 1], allowing for negative edge costs.

Heuristic: The node coordinates are crucial for calculating the admissible Euclidean distance heuristic h(n) used by A*.

Algorithms

Bellman-Ford (BF) - Baseline:

Uses the networkx implementation.

Guaranteed to find the optimal shortest path even with negative edge weights.

Detects negative cycles (which result in an unbounded cost).

A* Search - Optimized:

A custom implementation using a priority queue (min-heap).

Uses the cost-to-reach function g(n) plus the Euclidean distance heuristic h(n) to calculate the priority f(n) = g(n) + h(n).

Admissibility: Since the Euclidean distance is a straight-line distance, it is guaranteed to be less than or equal to the true path cost, making it an admissible heuristic. This ensures A* finds the optimal path.

Testing Strategy

The code iterates through all combinations of the defined parameters. For each configuration:

A graph is generated.

A random (Start, Goal) node pair is sampled until a pair is found for which Bellman-Ford can locate a path with a finite cost (i.e., not an infinite path and not a path compromised by a negative cycle).

Both Bellman-Ford and A* are run on the same pair.

The cost, execution time, and path of both algorithms are recorded.


3. Analysis of Results

The following observations are based on the final summary table generated from the execution:

3.1. Optimality

Conclusion: In all scenarios where a path was successfully found (i.e., BF returned a finite cost), the A* Cost exactly matched the Bellman-Ford Cost.

Interpretation: This confirms the theoretical guarantee that when an admissible heuristic (like the Euclidean distance used here) is employed, the A* search algorithm is optimal, meaning it always finds the lowest-cost path.


3.2. Key Comparison:

For most of the Graphs:  Times were nearly identical

3.3. Negative Weights and Path Sampling Failures

Success with Negative Weights: A* successfully found the optimal path in all scenarios where negative weights were present but no negative cycle existed on the shortest path. This is expected because the Euclidean distance heuristic remains admissible and the underlying graph structure is still valid for A* to terminate optimally.

Path Sampling Failure: Multiple scenarios, predominantly those combining low density=0.2 and negative_values=True, resulted in "Path sampling failed."

Interpretation: This is a byproduct of the test setup. Low density means fewer edges, making a connected path between two random nodes less likely. The inclusion of negative values does not cause the failure, but the sparse graph structure made it hard for the random sampler to find a guaranteed connected path pair within the allowed attempts.

4. Generated Data File

The complete, granular data for all test scenarios is available in:
shortest_path_comparison_summary.csv

Disclaimer:
I used Gemini to help me in code structure, documentation formatting and to optimize some parts of my implementation. Some ideas were mine and some were Gemini suggested improvements.