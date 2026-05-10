# Supply Chain Network Optimization

Optimization of a four-layer retail supply chain using Multi-Commodity Network Flow (MCNF) with Gurobi. Built for Forever 67, a hypothetical fast-fashion retailer.

## Problem Overview

The supply chain spans four layers: suppliers (A) → regional warehouses (B) → distribution centers (C) → retail stores (D), with 10 suppliers, 5 warehouses, 5 distribution centers, and 20 stores.

The model minimizes total transportation cost plus weighted backorder penalties across all demand nodes, subject to flow balance, capacity, and inventory constraints.

**Baseline result:** Optimal cost ~$12,255 at ~99% service level.

## Actionable Insights

### 1. Network Expansion Analysis

Three levers for supply chain improvement were analyzed and compared:

| Strategy | Best Action | Cost Improvement |
|---|---|---|
| Transport capacity | Increase B4→C2 capacity by 80 | −6.5% |
| Manufacturing capacity | Expand A5 (+18) and A6 (+32) inventory | −2.0% |
| New routes | Open 1 new transport edge (B2→C5) | −6.4% |

Sensitivity analysis sweeps each lever across a budget range, producing shadow prices and allowable ranges to guide investment decisions.

### 2. Disruption Resilience Analysis

Four disruption scenarios were simulated:

- **Inventory shortage** — proportional reduction across all suppliers. The network absorbs moderate shortfalls well due to routing flexibility.
- **Transport capacity disruption** — proportional capacity reduction on all edges. Cost increases sharply beyond ~11% reduction.
- **Edge failure** — single-edge enumeration identifies C4→D6 as a critical single point of failure: its removal alone causes a **+62% cost increase**, far exceeding any other edge.
- **Node failure** — The network absorbsmoderate shortfalls well, but Layer C should be prioritized when targeting node resilience.

### 3. Worst-Case Vulnerability Analysis

Implemented an **adaptive greedy algorithm** for both edge and node failures: at each step, the currently most damaging failure is selected given all prior failures. This produces a tighter worst-case bound than static pre-ranking, revealing that the true resilience envelope degrades faster than naive vulnerability rankings suggest.

## Technical Stack

- **Gurobi / gurobipy** — LP and MIP optimization
- **NetworkX** — network topology and flow visualization  
- **pandas / matplotlib**
- **Python**
