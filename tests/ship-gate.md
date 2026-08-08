# Ship Gate — Northfield ticket router

Go-live rule for the Northfield ticket router — message in, queue out.

---

## Standard

> A two-problem message opens two tickets.

---

## Hold style

**gs-b** — Soft hold: ship proceeds, but flag remaining Slips for manual review before full rollout.

---

## Block threshold

**2 slips** — When the board shows 2 or more Slips rows, the soft hold activates.

---

## Re-run trigger

**rr-a** — Re-run this board whenever the prompt changes.

---

## Current board summary (7 tasks)

| Task | Verdict |
|------|---------|
| p1_bundle | Slips |
| p2_messy_harmless | Caught |
| p3_mind_reader | Hold |
| p4_small_quotable | Slips |
| p5_hidden_library | Slips |
| p6_goldfish | Caught |
| p7_your_own (yt-identity) | Hold |

**Slips count this run:** 3 (p1_bundle, p4_small_quotable, p5_hidden_library)

---

## Go-live rule

The board shows **3 Slips rows**, which exceeds the block threshold of **2**.

**Soft hold is active.**

Ship may proceed, but the following Slips rows require manual review and owner assignment before full rollout:

| Slips row | Defense that flips it | Owner |
|-----------|----------------------|-------|
| p1_bundle | Force a split when there are two jobs (split_bundles — currently ON) | Support ops lead |
| p4_small_quotable | Require a quoted source line (name_source — currently ON) | Support ops lead |
| p5_hidden_library | Require a quoted source line (name_source — currently ON) | Support ops lead |

---

## Defenses enabled

- **split_bundles**: ON — Force a split when there are two jobs
- **name_source**: ON — Require a quoted source line

## Defenses not enabled

- **rewrite_mind_read**: OFF — Ban mind-reading verbs

---

## Decision

With **gs-b** (soft hold) and **3 Slips** exceeding the **2-slip threshold**:

1. Ship may proceed with flagged review
2. Assign each Slips row to an owner for manual triage
3. Re-run this board when the prompt changes (rr-a)

The Northfield ticket router can go live under soft hold, provided the three Slips rows above are tracked and reviewed before full rollout.
