# Echo Chamber Protocol — Example Run
## Course: Arrays Dungeon (Beginner → Intermediate → Boss)

**Run Start**
- Ego: 100%  
- XP (persist): 0

---

### Beginner Stage (basic array ops)
- Objective: implement `sum(array)` and `max(array)`; tests check edge cases (empty array, negative numbers).
- Fail #1 — off-by-one in loop → Ego 95% | XP +1  
- Fail #2 — didn't handle empty array → Ego 90% | XP +2  
- Success — fixed edge cases → Pass Beginner

_Status_: Ego 90% | XP 2

---

### Intermediate Stage (manipulations & in-place transforms)
- Objective: implement `rotateRight(array, k)` and `removeDuplicates(array)` (no extra arrays allowed).
- Fail #3 — rotate logic reversed → Ego 85% | XP +3  
- Fail #4 — removeDuplicates used extra array (forbidden) → Ego 80% | XP +4  
- Fail #5 — index math bug → Ego 75% | XP +5  
- Fail #6 — subtle edge case (k > n) missed → Ego 70% | XP +6  
- **Turbocharge Activated ⚡** (fast fails + thoughtful retries)  
- Fail #7 — test suite reveals boundary case → Ego 70% | XP +8 (turbo double XP)  
- Fail #8 — micro-optimization attempt fails → Ego 70% | XP +10  
- Success — Intermediate cleared (turbo still active)

_Status_: Ego 70% | XP 10 | Turbocharge active (40m left)

---

### Boss Stage (Array Boss: sliding-window + two-pointer madness)
- Objective: implement `maxSubarraySumK(array, k)` with O(n) runtime and pass 50 hidden tests including random large inputs.
- Note: Boss stage = unlimited attempts, no ego penalty for fails; inactivity or leaving run for >24h resets run.
- Attempt #1 → failed on negative-only input → XP +11  
- Attempt #2 → TLE on large input (wrong complexity) → XP +12  
- Attempt #3 → off-by-one on window boundaries → XP +13  
- Attempt #4 → handle single-element k edge → XP +14  
- Attempt #5 → success! Boss defeated 🎉

_Run End_  
- Ego: 70% (didn't hit ego death)  
- XP Earned this run: 14  
- Status: Course Cleared → Next course unlocked (Loops Dungeon)  
- Persistent XP total: +14

