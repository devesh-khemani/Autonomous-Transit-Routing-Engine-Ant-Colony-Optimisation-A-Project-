# Autonomous Transit Routing Engine — A* & Ant Colony Optimization

**[View Implementation & Benchmark Notebook](./ACO.ipynb)**

An algorithmic study and empirical benchmark evaluating deterministic heuristic search against bio-inspired multi-agent optimization across dynamic transit graphs. 

The project models network topologies as time-varying weighted graphs, evaluates optimal routing under dynamic constraints, and analyzes the trade-offs between centralized deterministic pathfinding and decentralized emergent swarm intelligence.

---

## What This Project Does

* **Dynamic Network Modeling:** Models London Underground transit connectivity as a time-varying weighted graph in Python.
* **Deterministic Search ($A^*$):** Implements an $A^*$ pathfinding engine utilizing an admissible Haversine heuristic, dynamically updating edge costs using live Transport for London (TfL) feeds.
* **Bio-Inspired Swarm Optimization (ACO):** Implements an Ant Colony Optimization simulation of 1,550 distributed agents to investigate decentralized, emergent route selection via probabilistic pheromone mapping.
* **Empirical Benchmarking:** Evaluates ACO against $A^*$ under real-time guidance constraints, comparing convergence behavior, runtime latency, and memory overhead.

---

## Key Takeaway

While Ant Colony Optimization scales effectively to decentralized, multi-agent load-balancing and exploration where agents lack global network visibility, its stochastic convergence and per-agent memory overhead make deterministic $A^*$ search markedly superior for low-latency, real-time single-agent guidance.

---

## Tools & Technologies

* **Core Language:** Python
* **GUI / Frontend:** Kivy
* **APIs & Ingestion:** TfL Unified API, IP Geolocation REST API
* **Algorithms:** $A^*$ Graph Search, Ant Colony Optimization (ACO)
* **Libraries:** NumPy, Requests

---

## Performance Highlights

* **$A^*$ Search Optimization:** Reduced path-computation latency by **20–90%** over time-varying edge weights versus a naive recomputation baseline by exploiting an admissible Haversine heuristic to aggressively prune node expansion on dynamic topologies.
* **Large-Scale Agent Simulation:** Simulated **1,550 distributed agents** exploring graph topologies in parallel to evaluate decentralized routing without centralized path arbitration.
* **Benchmarking Axes:** Evaluated convergence latency, computational overhead, and optimality profiles between deterministic single-agent search and stochastic multi-agent heuristics.

---

## Technical Implementation

### 1. Dynamic $A^*$ Pathfinding Engine
The $A^*$ engine treats the transit network as a live, non-static graph:

* **Graph Representation:** Stations are modeled as nodes; directed edges represent travel times and line transfer penalties pulled from the **TfL Unified API**. Edge weights vary dynamically according to real-time service disruptions and platform intervals.
* **Admissible Heuristic:** Uses the **Haversine (great-circle) formula** on station latitude and longitude coordinates. Because Euclidean distance across geographic coordinates never overestimates true transit route costs, the heuristic remains strictly admissible ($h(n) \le d(n, \text{goal})$), guaranteeing path optimality.
* **Cost Evaluation:** Evaluates nodes via:
  $$f(n) = g(n) + h(n)$$
  where $g(n)$ is accumulated real-time transit cost and $h(n)$ is the Haversine estimate to destination.
* **Dynamic Re-Evaluation:** Instead of computing a static shortest path upfront, the search dynamically checks edge weights during traversal, eliminating full graph re-exploration during network status updates.

### 2. Ant Colony Optimization (ACO) Engine
The ACO module models route generation via collective probabilistic behavior across a distributed population:

* **Pheromone Matrix:** A global matrix tracks trail intensity ($\tau_{ij}$) across all active edges. Trails are initialized uniformly and reinforced after each simulation epoch proportionally to route efficiency ($\Delta\tau = \frac{1}{\text{distance}}$).
* **State Transition Rule:** Each agent determines its trajectory probabilistically, balancing existing pheromone concentration with inverse distance visibility ($d_{ij}^{-1}$). The decision rule is evaluated via a cumulative frequency distribution sampled via roulette-wheel selection:
  $$P(i \to j) = \frac{\tau_{ij} \cdot d_{ij}^{-1}}{\sum_{k \notin \text{visited}} \tau_{ik} \cdot d_{ik}^{-1}}$$
* **Pheromone Evaporation:** To prevent premature convergence to sub-optimal local minima, pheromones decay dynamically across simulation cycles:
  $$\tau \leftarrow (1 - \rho) \cdot \tau$$
  ensuring continuous graph exploration across the 1,550-agent colony.
* **Convergence & Path Extraction:** The global optimal solution is extracted via `highest_pheromone_path()` across the reinforced trail matrix once iteration bounds are reached.

---

## Repository Structure

| File | Description |
| :--- | :--- |
| `ACO.ipynb` | Ant Colony Optimization simulation, pheromone update logic, and benchmarking experiments |
| `transit_routing.ipynb` | $A^*$ pathfinding engine, dynamic TfL API ingestion, and Kivy interface |
| `README.md` | System architecture, algorithmic breakdowns, and performance analysis |

---

## Setup & Configuration

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/devesh-khemani/Autonomous-Transit-Routing-Engine-Ant-Colony-Optimisation-A-Project.git](https://github.com/devesh-khemani/Autonomous-Transit-Routing-Engine-Ant-Colony-Optimisation-A-Project.git)
   cd Autonomous-Transit-Routing-Engine-Ant-Colony-Optimisation-A-Project
