# The Torchbearer

**Student Name:** Brandon Garate
**Student ID:** 130364309
**Course:** CS 460 – Algorithms | Spring 2026

> This README is your project documentation. Write it the way a developer would document
> their design decisions , bullet points, brief justifications, and concrete examples where
> required. You are not writing an essay. You are explaining what you built and why you built
> it that way. Delete all blockquotes like this one before submitting.

---

## Part 1: Problem Analysis

- **Why a single shortest-path run from S is not enough:**

  Performing only one run from the entrance will only give us the shortest distances from `S` to every node. It will never visit every relic, since it will never commit to an order. The total fuel also depends on which relic-to-relic stretch we will actually be chaining together.

- **What decision remains after all inter-location costs are known:**

  Pairwise costs only say how expensive each stretch is, we will still need to choose an order through all the relics, which minimizes the sum of the stretches.

- **Why this requires a search over orders (one sentence):**

  Each real solution is still characterized by an order, where the relics are first collected. Different orders will most likely hold a different total cost, which means that we must compare lots of these orders in order to find the optimal order, and not by a singular run of a shortest-path algo.

---

## Part 2: Precomputation Design

### Part 2a: Source Selection

- **Spawn:** cheapest paths from the entrance.
- **Relic (each):** cheapest paths from any relic you might leave next.

| Source Node Type | Why it is a source |
|---|---|
| Spawn | Planner needs every shortest cost leaving `S`. |
| Relic chamber | Same, for each chamber you can occupy between objectives. |

### Part 2b: Distance Storage

| Property | Your answer |
|---|---|
| Data structure name | Python `dict` (hash map) |
| What the keys represent | A graph node `v` (destination) |
| What the values represent | Min cost `source → v` from this Dijkstra run |
| Lookup time complexity | `O(1)` expected per lookup |
| Why O(1) lookup is possible | Dict lookup is hashing on the node key |

### Part 2c: Precomputation Complexity

- **Number of Dijkstra runs:** one per `select_sources` output node (≤ `k + 1` for `k = |M|`).
- **Cost per run:** `O(m log n)` for `n = |V|`, `m = |E|` (binary heap Dijkstra).
- **Total complexity:** `O((k + 1) · m log n)` = `O(k m log n)`.
- **Justification (one line):** each run is a full Dijkstra from a different source; counts multiply.

---

## Part 3: Algorithm Correctness

### Part 3a: What the Invariant Means

- **For nodes already finalized (in S):**
  `dist[u]` is the cheapest total cost from the source to `u`. no cheaper stretch exists.

- **For nodes not yet finalized (not in S):**
  `dist[u]` is the cheapest total cost found so far, from the paths which **interior** vertices all lie in the final set. 

### Part 3b: Why Each Phase Holds

- **Initialization — why the invariant holds before iteration 1:**
  If you start with the source at distance `0` and everything else is valued at `infinity`, there are "no cheaper path" options execpt the source. 

- **Maintenance — why finalizing the min-dist node is always correct:**
  If you pick `u` with the smallest possible distance outside the final set, any other path to `u` will need to first reach some "non-finalized" `w` through a first step with a non negative costs. This way the path is at least as long as `dist[w] ≥ dist[u]`. 


- **Termination — what the invariant guarantees when the algorithm ends:**
  Every node that ever gets finalized has its true shortest distance from the source. Anything else remains as `infinity`

### Part 3c: Why This Matters for the Route Planner

A wrong `dist` value would misprice every corridor leg in `dist_table`, so the search could pick a suboptimal relic order or wrongly think a route is impossible.

---

## Part 4: Search Design

### Why Greedy Fails

> State the failure mode. Then give a concrete counter-example using specific node names
> or costs (you may use the illustration example from the spec). Three to five bullets.

- **The failure mode:** _Your answer here._
- **Counter-example setup:** _Your answer here._
- **What greedy picks:** _Your answer here._
- **What optimal picks:** _Your answer here._
- **Why greedy loses:** _Your answer here._

### What the Algorithm Must Explore

> One bullet. Must use the word "order."

- _Your answer here._

---

## Part 5: State and Search Space

### Part 5a: State Representation

> Document the three components of your search state as a table.
> Variable names here must match exactly what you use in torchbearer.py.

| Component | Variable name in code | Data type | Description |
|---|---|---|---|
| Current location | | | |
| Relics already collected | | | |
| Fuel cost so far | | | |

### Part 5b: Data Structure for Visited Relics

> Fill in the table.

| Property | Your answer |
|---|---|
| Data structure chosen | |
| Operation: check if relic already collected | Time complexity: |
| Operation: mark a relic as collected | Time complexity: |
| Operation: unmark a relic (backtrack) | Time complexity: |
| Why this structure fits | |

### Part 5c: Worst-Case Search Space

> Two bullets.

- **Worst-case number of orders considered:** _Your answer (in terms of k)._
- **Why:** _One-line justification._

---

## Part 6: Pruning

### Part 6a: Best-So-Far Tracking

> Three bullets.

- **What is tracked:** _Your answer here._
- **When it is used:** _Your answer here._
- **What it allows the algorithm to skip:** _Your answer here._

### Part 6b: Lower Bound Estimation

> Three bullets.

- **What information is available at the current state:** _Your answer here._
- **What the lower bound accounts for:** _Your answer here._
- **Why it never overestimates:** _Your answer here._

### Part 6c: Pruning Correctness

> One to two bullets. Explain why pruning is safe.

- _Your answer here._

---

## References

> Bullet list. If none beyond lecture notes, write that.

- _Your references here._
