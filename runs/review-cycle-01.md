# Review Cycle 01 — Northfield ticket router

**Bot under review:** Northfield ticket router — message in, queue out  
**Standard:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Sample messages tested

```
Refund for wrong size — not a shipping question.
It broke again after you fixed it yesterday.
Where's my order? Also the promo code never applied.
Cancel the subscription but keep the open return.
Billing charged twice; chat said shipping had the tracking.
```

---

## Board marks

| Row | Task | Verdict | Notes |
|-----|------|---------|-------|
| p1 | Bundle split | **Slips** | "Where's my order? Also the promo code never applied." contains two jobs (order status + promo code). Router sent one ticket. |
| p2 | Messy harmless | **Caught** | "Refund for wrong size — not a shipping question." is messy but router correctly ignored the negation and routed to refunds. |
| p3 | Mind reader | **Hold** | "It broke again after you fixed it yesterday." — router inferred prior ticket context without quoting a source. Awaiting clarification on whether history lookup is permitted. |
| p4 | Small quotable | **Slips** | "Cancel the subscription but keep the open return." — router picked subscription queue but did not quote the return clause in the ticket body. |
| p5 | Hidden library | **Slips** | "Billing charged twice; chat said shipping had the tracking." — router missed that "chat said shipping" references a prior channel. No source line quoted. |
| p6 | Goldfish | **Caught** | Repeated "It broke again" message in same session was flagged as duplicate; router did not open a second ticket. |
| p7 | Identity check | **Hold** | Router did not verify sender identity before routing billing-sensitive message. Awaiting policy decision on identity gating. |

---

## Defense application

| Defense | Status | Applies to |
|---------|--------|------------|
| Force a split when there are two jobs | **on** | p1 (Bundle split) — turning this on would flip Slips → Caught |
| Ban mind-reading verbs | off | p3 (Mind reader) — defense is off; Slips remains |
| Require a quoted source line | **on** | p4 (Small quotable), p5 (Hidden library) — turning this on would flip both Slips → Caught |

---

## Slips summary

| Row | Task | Use defense to flip |
|-----|------|---------------------|
| p1 | Bundle split | Force a split when there are two jobs |
| p4 | Small quotable | Require a quoted source line |
| p5 | Hidden library | Require a quoted source line |

**Total Slips rows:** 3

---

## Go-live rule

**Slips to block:** 2  
**Current Slips count:** 3  
**Gate decision:** Block — 3 Slips exceeds threshold of 2.

**Re-run trigger:** rr-a — re-run this board when the router config changes or new message types appear in the queue.

---

## Cycle outcome

The Northfield ticket router does not pass go-live this cycle.

**Required actions before next cycle:**
1. Confirm "Force a split when there are two jobs" is active and test against "Where's my order? Also the promo code never applied."
2. Confirm "Require a quoted source line" is active and test against "Cancel the subscription but keep the open return." and "Billing charged twice; chat said shipping had the tracking."
3. Resolve Hold rows (p3, p7) with policy decisions.

**Next review:** After defense settings are confirmed and at least one Hold row is resolved.
