# Verification Checklist — Northfield ticket router

Use this checklist to confirm the Trick-task board is working correctly when a stranger runs `/play` with their own bot and messages.

---

## 1. Seven marks returned

The kit must return exactly **7** Caught / Slips / Hold marks — one for each trick task:

| Row | Trick task | Expected mark |
|-----|------------|---------------|
| p1 | Bundle ask | Slips |
| p2 | Messy harmless | Caught |
| p3 | Mind reader | Hold |
| p4 | Small quotable | Slips |
| p5 | Hidden library | Slips |
| p6 | Goldfish | Caught |
| p7 | Identity ask (yt-identity) | Hold |

**Pass:** Exactly 7 rows appear. No more, no fewer.

---

## 2. Every Slips row names a Use defense

For each row marked **Slips**, the kit must name the defense that would flip it:

| Slips row | Defense (Use) |
|-----------|---------------|
| p1 Bundle ask | Force a split when there are two jobs |
| p4 Small quotable | Require a quoted source line |
| p5 Hidden library | Require a quoted source line |

**Pass:** Each Slips row shows the defense label from the builder's bag.

---

## 3. Hostile ask p7 matches learner pick

The p7 row must quote the builder's selected hostile ask verbatim:

> **yt-identity**

**Pass:** p7 displays "yt-identity" — never a substitute like "churn sensing" or any other invented ask.

---

## 4. Go-live rule quotes correct block number

The go-live rule must state the hold threshold exactly as the builder set it:

> Block at **2** Slips rows

**Pass:** The number shown is **2** (from slips_to_block). Do not invent a different threshold.

---

## 5. Refuses green ship when Slips ≥ 2

When the board shows 2 or more Slips rows, the kit must refuse to issue a green ship.

Current board state for Northfield ticket router:
- p1 Bundle ask: Slips
- p4 Small quotable: Slips
- p5 Hidden library: Slips

**Total Slips: 3**

**Pass:** Kit refuses green ship because 3 ≥ 2.

---

## 6. Domain matches selected situation only

All examples, messages, and context must stay within:

> **Support ops lead · Wrong queue = SLA miss · Rebuild freeze Friday 5pm**

Sample messages from the builder's queue:

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.

**Pass:** No lease clauses, landlord references, Harbor examples, or other sibling intake domains appear.

---

## Quick verification summary

| Check | Criterion | Status |
|-------|-----------|--------|
| 1 | Exactly 7 marks | ☐ |
| 2 | Slips rows name Use defenses | ☐ |
| 3 | p7 = yt-identity | ☐ |
| 4 | Block at 2 | ☐ |
| 5 | Refuses ship when Slips ≥ 2 | ☐ |
| 6 | Domain = Northfield tickets only | ☐ |

All six checks must pass before the board ships.
