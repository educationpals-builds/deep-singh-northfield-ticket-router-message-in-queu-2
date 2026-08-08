# Measurements: Northfield ticket router — message in, queue out

What counts as observable evidence for each of the seven trick tasks when auditing the Northfield ticket router.

---

## Standard line

> A two-problem message opens two tickets.

---

## Observable criteria by task

### p1_bundle — Multi-job detection

**Observable:** Count of tickets created vs. count of distinct problems in the input message.

**Test message:**
> Where's my order? Also the promo code never applied.

**Pass criteria:** Router creates exactly 2 tickets (one for order status, one for promo code issue).

**Fail criteria:** Router creates 1 ticket containing both problems, or routes to a single queue.

---

### p2_messy_harmless — Noisy but single-intent

**Observable:** Ticket count remains 1 when extra words do not indicate a second job.

**Test message:**
> Refund for wrong size — not a shipping question.

**Pass criteria:** Router creates exactly 1 ticket routed to refunds queue. The clarifying phrase ("not a shipping question") does not trigger a second ticket.

**Fail criteria:** Router creates 2 tickets or routes to shipping queue.

---

### p3_mind_reader — Implicit context assumption

**Observable:** Presence or absence of assumed context in the routed ticket metadata.

**Test message:**
> It broke again after you fixed it yesterday.

**Pass criteria:** Router either requests clarification or routes to a general triage queue without assuming product type.

**Fail criteria:** Router assumes a specific product category or repair type without evidence in the message.

---

### p4_small_quotable — Source line requirement

**Observable:** Ticket record includes a quoted source line from the original message.

**Test message:**
> Cancel the subscription but keep the open return.

**Pass criteria:** Routed ticket quotes the original message text verbatim in a source field.

**Fail criteria:** Ticket contains only a paraphrase or category label with no quoted source.

---

### p5_hidden_library — Undocumented routing logic

**Observable:** Routing decision can be traced to a documented rule.

**Test message:**
> Billing charged twice; chat said shipping had the tracking.

**Pass criteria:** The queue assignment matches a documented routing rule that covers billing + shipping cross-reference.

**Fail criteria:** Ticket routes to a queue with no documented rule explaining why billing-plus-shipping goes there.

---

### p6_goldfish — Context retention across turns

**Observable:** Same route for related follow-up messages within a session.

**Test message (follow-up):**
> It broke again after you fixed it yesterday.

**Pass criteria:** If this message follows a prior ticket about the same issue, router links or routes to the same queue.

**Fail criteria:** Router treats the follow-up as a brand-new issue with no link to prior context.

---

### p7_your_own (yt-identity) — Identity confusion

**Observable:** Router does not conflate customer identity with issue type.

**Test message:**
> Billing charged twice; chat said shipping had the tracking.

**Pass criteria:** Router keeps billing issue and shipping reference as separate data points; does not assume the customer is a shipping department employee.

**Fail criteria:** Router misattributes the message sender's role or merges unrelated identity signals into routing logic.

---

## Source

Last week's live queue export (10 messages).

---

## Sample messages (verbatim from learner)

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
