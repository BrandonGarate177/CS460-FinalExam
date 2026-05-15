# Development Log – The Torchbearer

**Student Name:** Brandon Garate 
**Student ID:** 130364309

---

## Entry 1 – [May 13, 3pm]: Initial Plan


Math: 
    - One Dijkstra from `S` is only fixing for distance from `S`. It does NOT select an order to visit all relics. 
    - After precomputation we will still need to search over permutations / the orders. 

Implementation: 
    - `run_dijkstra` should be created first. 
    - `select_sources` && `precompute_distances`. 
    - `find_optical_route` / `_explore`, these should state where we are, which relics are done, and the cost so far. 
        the goal is that all relics are collected, and then pay costs to T. 

How are we supposed to preform the precalculation? How do we determine which nodes we need to run Dijkstra FROM so that every lef the search needs exists in our table? 






---

## Entry 2 – [May 13, evening]: Part 2 design assumptions

We treated the exit as a canditate Dijkstra source so we would have a row for "leaving T", but the planner didn't need that. We only really need the cheapest cost TO the exit from which ever position we currently located. 

I kept `select_sources` as the spawn, we also had each relic rely on `dist_table[u][exit_node]` for the final stretch. 

We also almost wrote Part 2.b abotu the nested `dist_table` but I caught the handout mapping aligned 2b with the single sourced `dist` given through running `run_dijkstra`

---

## Entry 3 – [May 14, afternoon]: back after a day

Last time i was on here i knocked out part 3 (readme + the `dijkstra_invariant_check()` string so it actually matches). 

Been a full day since, lining up what's left: 

part 4 / `explain_search()`, then the meaty parts 5-7 with the actual search + pruning + `solve` 


---

## Entry 4 – [Date]: Post-Implementation Reflection

> Required. Written after your implementation is complete. Describe what you would
> change or improve given more time.

_Your entry here._

---

## Final Entry – [Date]: Time Estimate

> Required. Estimate minutes spent per part. Honesty is expected; accuracy is not graded.

| Part | Estimated Hours |
|---|---|
| Part 1: Problem Analysis | |
| Part 2: Precomputation Design | |
| Part 3: Algorithm Correctness | |
| Part 4: Search Design | |
| Part 5: State and Search Space | |
| Part 6: Pruning | |
| Part 7: Implementation | |
| README and DEVLOG writing | |
| **Total** | |
