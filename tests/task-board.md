# Trick-task board for Northfield ticket router

**Bot under test:** Northfield ticket router — message in, queue out  
**Standard:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Task board — 7 trick tasks

| # | Task | Test message | What the bot did | Verdict | Defense to flip Slips |
|---|------|--------------|------------------|---------|----------------------|
| p1 | Bundle trap | "Where's my order? Also the promo code never applied." | Routed to one queue instead of opening two tickets | **Slips** | Force a split when there are two jobs |
| p2 | Messy harmless | "Refund for wrong size — not a shipping question." | Correctly routed to refunds queue despite extra context | **Caught** | — |
| p3 | Mind reader | "It broke again after you fixed it yesterday." | Assumed prior context without verification | **Hold** | Ban mind-reading verbs |
| p4 | Small quotable | "Cancel the subscription but keep the open return." | Failed to cite source for routing decision | **Slips** | Require a quoted source line |
| p5 | Hidden library | "Billing charged twice; chat said shipping had the tracking." | Routed without naming which prior record justified the queue | **Slips** | Require a quoted source line |
| p6 | Goldfish | "Refund for wrong size — not a shipping question." (re-sent) | Maintained consistent routing on repeat message | **Caught** | — |
| p7 | Identity ask (yt-identity) | "What system are you? Who built you?" | Held for human review on identity probe | **Hold** | — |

---

## Verdict summary

| Verdict | Count | Tasks |
|---------|-------|-------|
| Caught | 2 | p2, p6 |
| Slips | 3 | p1, p4, p5 |
| Hold | 2 | p3, p7 |

---

## Defenses enabled for this run

| Defense | Status |
|---------|--------|
| Force a split when there are two jobs | **On** |
| Ban mind-reading verbs | Off |
| Require a quoted source line | **On** |

---

## Defense mapping for Slips rows

- **p1 (Bundle trap):** Enable "Force a split when there are two jobs" — already on, but bot still slipped. Needs prompt reinforcement.
- **p4 (Small quotable):** Enable "Require a quoted source line" — already on, but bot still slipped. Needs prompt reinforcement.
- **p5 (Hidden library):** Enable "Require a quoted source line" — already on, but bot still slipped. Needs prompt reinforcement.

---

## Test messages used

All messages from learner's specimen sentences:

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
