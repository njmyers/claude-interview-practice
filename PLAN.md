# Prep plan

_Created 2026-10-09 from `/prep-plan` intake and a 20-question diagnostic._

## Target

Staff backend or full-stack engineer at big-tech or mid-size product companies, interviewing in TypeScript.
Loop format unknown; assume coding, system design, and behavioral. ~17 weeks at ~1 hour/day.

## Scores

Overall ≈ 39%. Structural gaps: algorithm patterns (heaps, graph traversal, sliding window,
topological sort), linked structures, and log-n complexity. System design intuition is ahead of
algorithms.

| Category | Score | Specific misses |
|---|---|---|
| Algorithms | ~18% | **Meeting rooms:** missed sort + min-heap of end times (O(n log n)); claimed O(1) map. **Islands:** didn't name DFS/BFS or visited marking or "count search starts"; built a graph the grid already is (complexity correct). **Longest substring:** map per start is O(n²), not O(n); missed the sliding window with last-seen index. **Build order:** didn't recognize topological sort; "impossible" is a cycle, not two trees; no in-degree (Kahn's) or DFS cycle detection. |
| Data structures | ~30% | **Running median:** missed two heaps (O(log n) insert, O(1) median). **LRU:** missed a hash map pointing to nodes in a doubly linked list; sorting timestamps isn't O(1). **Hash map internals:** missed hash → array index, chaining vs open addressing, and load-factor resize → amortized O(1). |
| Complexity | ~37% | **Structural: log-n costs missed three times.** Binary search said O(n/2), not O(log n). Sorting said O(n), not O(n log n). The doubling loop sums to about n (O(n)), not O(n²). No space complexity given; the reason to sort is O(1) extra space, not simpler code. Naive fib O(2ⁿ) and `unshift` loop O(n²) correct, but no reasoning given. |
| System design | ~54% | **Strong:** saga + idempotency key; expand/contract migration. **URL shortener:** hashed keys shard evenly; the real strain is the 100:1 read ratio → cache/CDN; partitioning by user ID breaks lookups by short code; truncated-MD5 collisions not addressed. **Rate limiter:** count requests per window, not in-flight; missed atomic INCR, the algorithm choice (fixed/sliding window, token bucket), and fail-open vs fail-closed when Redis is down. **Checkout:** missed the outbox pattern, step ordering, and that the email can't be undone. **Migration:** missed backfill throttling and that splitting names is lossy. |
| JS runtime | ~47% | **Blocking event loop + streaming fix:** excellent. **Event-loop order:** answered `A G B C E F D`; correct is `A E G C D F B`. An async fn runs synchronously up to its first `await`, and all microtasks drain before the next macrotask. **`var` in a loop:** prints `3 3 3`, not `0 0 0`; there's one shared binding, and callbacks run after the loop. |
| Practical judgment | ~47% | **Estimation:** arithmetic exact (500 GB/day, ~182 TB/yr), but a single Postgres holding 45–90 TB is unrealistic, so weeks, not months; missed partitioned/columnar storage and retention for append-only events. **Code review:** SQL injection correct; missed N+1 (2N sequential queries vs `IN`/`JOIN`). **Latency triage:** missed scoping first (which endpoints/hosts, traffic, dependency latency), saturation (pools, CPU, GC), non-deploy changes (cron, flags, config), and mitigating before root-causing. |

## Priorities

1. **Complexity and core data structures** — weeks 1–2
   - **What:** log n, the cost of sorting, amortized cost; heap, linked list, hash map internals.
   - **Resources:** this repo's problems; NeetCode "Big-O" primer.
   - **Reps:** build `min-heap`, `priority-queue`, `binary-search`, `first-bad-version`,
     `reverse-linked-list`, and `lld/lru-cache` from scratch. For each, state the cost of every
     operation and explain why.

2. **Core algorithm patterns** — weeks 3–11 (biggest gap)
   - **Resources:** `CHECKLIST.md`, `SLIDING_WINDOW.md`, `DP.md`, `BACKTRACKING.md`; NeetCode 150 roadmap.
   - **Order:**
     - Graph traversal: `number-of-islands`, `max-area-of-island`, `rotting-oranges`,
       `clone-graph`, `course-schedule` (topological sort), `pacific-atlantic`.
     - Sliding window and two pointers.
     - Intervals and heaps: `merge-intervals`, meeting rooms.
     - Trees: `validate-bst`, `lowest-common-ancestor`, `binary-tree-level-order`.
     - Binary search on the answer: `koko-bananas`.
     - 1D DP: `climbing-stairs`, `house-robber`, `coin-change`.
   - **Rep target:** write BFS and DFS from scratch 5 times, then 8 graph problems from easy;
     after that, 4–6 problems per pattern.

3. **Staff-level system design** — weeks 6–16, alongside priority 2
   - **Resources:** `PRIMITIVES.md`, `BUILDING_BLOCKS.md`, `SCALE_BREAKPOINTS.md`;
     *Designing Data-Intensive Applications*; Alex Xu, *System Design Interview* Vol 1.
   - **Reps:** 10 full designs, each covering estimation, the read/write ratio, failure modes,
     and how to evolve it. Start by redoing the URL shortener and rate limiter.

4. **JS runtime** — 2–3 sessions in week 2
   - **What:** event-loop order (sync → microtasks → macrotasks; `await`), closures and `var`/`let`.
   - **Resource:** Jake Archibald, "In The Loop" (talk).
   - **Rep:** predict the output of 10 ordering snippets, then run each one.

5. **Practical judgment** — one session/week throughout
   - **What:** incident triage order, review red flags (N+1, unbounded queries), back-of-envelope limits.
   - **Rep:** practice these as part of the system design sessions.

6. **Behavioral** — weeks 12–16
   - **What:** 6–8 staff-scope stories: cross-team influence, technical direction, mentoring, conflict.
   - **Note:** not in the diagnostic, but staff loops weigh it heavily.

## Skip list

- **Hard DP** (interval, bitmask, 2D-on-strings): rare at staff level; time pays off better in system design.
- **Advanced structures** (segment trees, Fenwick trees, union-find beyond basics) and **string
  algorithms** (KMP, Rabin–Karp): low probability for this target.
- **Bit tricks, geometry, competitive-programming puzzles:** not what backend loops test.
- **Frontend trivia** (React internals, CSS, browser rendering): the target is backend/full-stack,
  and full-stack loops lean on the backend.
- **Language trivia beyond the event loop and closures:** product companies test judgment, not trivia.

## Timeline (~7 h/week)

| Weeks | Focus |
|---|---|
| 1–2 | Complexity; build core data structures; JS runtime |
| 3–5 | Graph traversal and topological sort |
| 6–7 | Sliding window, two pointers, intervals and heaps; 2 system design sessions/week |
| 8–9 | Trees, binary search on the answer, linked lists; 2 system design sessions/week |
| 10–11 | 1D DP, basic backtracking; 2 system design sessions/week |
| 12–14 | System design heavy (4/week); 1 mixed `/interview` rep/week; draft behavioral stories |
| 15–16 | Full mock loops: `/interview` for coding plus a timed design session |
| 17 | Review logged weak spots; taper |

**Action item:** on the first recruiter call, ask what the loop looks like, then re-run `/prep-plan` to revise.

## First rep

`algo/min-heap.practice.ts`: build a min-heap from scratch with `/practice`. It goes straight at
the running-median, meeting-rooms, and log-n gaps.
