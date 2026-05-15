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


#### **The failure mode:** 
- A greedy algorithm will always move to the nearest unvisited relic. It optimizes for the most next step, ignoring any upstream calculations 

#### **Counter-example setup:** 

- S -> R1 = 1, S -> R2 =3;    
- R1 -> R2 = 100, R1 -> T = 1; R2 -> R1 = 1, R2 -> T = 100.

Two relics {R1  , R2} exit T 


#### What greedy picks:
When we start at `S`, greedy picks R1 first (+1), then it is forced to go R1 -> R2 (+100) then R2 -> T (+100) 

TOTAL = 201 

#### What optimal picks:
We start at `R2` first S -> R2 (+3). R2 -> R1 (+1), then R1 -> T (+1)

TOTAL = 5

#### Why greedy loses:

The first turn gives the option for a move with a cost of only 1, but it strands our torchbearer to only expensive legs. 

### What the Algorithm Must Explore


- The algorithm should be able to checl out every possible order, where the relic chambers are visted, because only comparing complete orderings could we know how to minimize fuel costs. 

---

## Part 5: State and Search Space

### Part 5a: State Representation


| Component | Variable name in code | Data type | Description |
|---|---|---|---|
| Current location | `current_loc` | node (hashable) | The dungeon node the Torchbearer currently occupies |
| Relics not yet collected | `relics_remaining` | `set` | Set of relic nodes still to be visited |
| Cost so far | `cost_so_far` | `float` | Total cost burned on the route so far |

### Part 5b: Data Structure for Visited Relics

| Property | Your answer |
|---|---|
| Data structure chosen | Python `set` |
| Operation: check if relic already collected | O(1) — `relic not in relics_remaining` |
| Operation: mark a relic as collected | O(1) — `relics_remaining.remove(relic)` |
| Operation: unmark a relic  | O(1) — `relics_remaining.add(relic)` |
| Why this structure fits | All three mutation operations are O(1); `add` makes backtracking trivial without copying the collection |

### Part 5c: Worst-Case Search Space

- **Worst-case number of orders considered:** 
k! (k factorial),  where k =  |M|.

- **Why:** 
At the first turn there are `k` moves of next relic, then k − 1, ..., then 1; multiplying gives k!.

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
