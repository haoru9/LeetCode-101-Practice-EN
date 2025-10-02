# Chapter 1 — Greedy Basics (1.1 · 1.2 · 1.3)

## 1.1 What the Algorithm Is (Greedy Rationale)
- **Definition**: Build the solution step by step, choosing a **locally optimal** (and **safe**) option at each step so that the final result is **globally optimal**.
- **When it works**
  - **Greedy-choice property**: some optimal solution starts with the greedy decision.
  - **Optimal substructure (no backtracking)**: after a greedy step, the remaining problem has the same form.
  - **Monotone progress**: taking the “best now” never harms future choices.
  - **Totally ordered candidates**: you can rank options (e.g., earliest finish, smallest cost).
- **How to justify**
  - **Exchange argument** or **stay-ahead (prefix dominance)**; for graph variants, **cut property**.
  - Keep a short “safety sketch” (2–5 lines) explaining why the greedy rule cannot lose.
- **Mindset**
  - Prefer the simplest rule that preserves feasibility and pushes a clear invariant forward.
  - If you cannot articulate the rule + invariant + safety, switch to DP/search.

---

## 1.2 Assignment-Type Problems (Pairing / Distribution)
- **Core rule**: after sorting, always match the **smallest unmet demand** with the **smallest sufficient resource** (two-pointer style).  
  *Intuition*: this leaves the most room for the remaining pairs.
- **Local constraints on neighbors** (e.g., ratings on a line): satisfy one direction, then the other (**two directional passes**) to enforce left/right inequalities without backtracking.
- **Typical invariants**
  - Chosen pairs are feasible and **do not block** better future matches.
  - After each step, the **unmatched** portion is the “hardest” part left and remains solvable by the same rule.
- **Common pitfalls**
  - Sorting by the wrong key (e.g., not aligning demand and capacity orders).
  - Using a “ratio” rule where it does not apply (0/1 knapsack is **not** greedy-friendly).

---

## 1.3 Interval-Type Problems (Scheduling / Deletion)
- **Maximize non-overlap ≡ Minimize removals**.
- **Canonical rule**: **sort by earliest finish time** and keep an interval **iff** it does **not overlap** the last kept one.  
  *Intuition / stay-ahead*: the smallest end time maximizes the feasible tail for future intervals.
- **Event-sweep companion**: represent starts as +1 and ends as −1, scan in order to track concurrency (e.g., rooms/capacity). This is still greedy in spirit (process the next earliest boundary).
- **Typical invariants**
  - The kept set is always non-overlapping (feasible).
  - Among all sets of the same size at a given step, the greedy set has the **earliest possible end**, hence “stays ahead”.
- **Common pitfalls**
  - Sorting by start time or by length often breaks optimality for “max non-overlap”.
  - Forgetting strictness: clarify whether touching at endpoints counts as overlap in the problem statement.
