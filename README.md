# Autonomous Transit Routing Engine — A* & Ant Colony Optimization

**[View Implementation & Benchmark Notebook](transit_routing.ipynb)**

**Note on this repository:** This project was written in 2023–24 as a personal
project, developed alongside independent research into swarm robotics from my EPQ.
As an early project, the code prioritises getting the underlying algorithms working
correctly over optimisation or production-level structure - some sections
(particularly the ACO implementation) are unoptimised and could be significantly
refactored for performance.

## What this project does
- Models London's Underground network as a dynamic weighted graph in Python
- Implements an A* pathfinding algorithm with an admissible Euclidean heuristic,
  using live TfL API data feeds to route over time-varying edge weights
- Implements a bio-inspired Ant Colony Optimization (ACO) algorithm simulating
  1,550 distributed agents, exploring decentralized multi-agent route planning via
  probabilistic pheromone mapping
- Benchmarks ACO against A* for embedded/real-time routing constraints, comparing
  convergence behaviour, memory overhead, and latency

## Key takeaway
While ACO scales well to decentralized, multi-agent routing scenarios, its
stochastic convergence and memory overhead make deterministic A* the better choice
for real-time, low-latency single-agent guidance.

## Tools & Technologies
Python · Kivy (GUI) · TfL API

---
*Written as an early personal project — shared here for transparency on the
research and algorithmic approach rather than as an example of production code
quality.*
