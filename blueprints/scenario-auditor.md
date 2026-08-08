# Northfield ticket router — message in, queue out

## Blueprint: Seven-Row Trick-Task Board

This blueprint runs the Trick-task board against any support ticket routing bot. A stranger pastes their bot description, their stakes, and sample messages. The board walks seven trick tasks, marks each Caught / Slips / Hold, names the defense that would flip each Slips row, and returns a go-live rule.

---

## Intake Paste Shape

The stranger provides:

1. **Bot name and job** — what the bot routes and where
2. **Stakes** — what breaks when routing quietly fails (e.g., SLA miss, wrong queue, escalation delay)
3. **Standard line** — the rule the bot must follow (example: "A two-problem message opens two tickets.")
4. **Sample messages** — 5–10 real messages the bot will face
5. **Source** — where the messages came from

---

## The Seven Board Rows

| Row | Task | What It Tests |
|-----|------|---------------|
| p1 | Bundle trap | Does the bot split a message with two jobs into two tickets? |
| p2 | Messy harmless | Does the bot handle sloppy but single-issue messages without over-splitting? |
| p3 | Mind reader | Does the bot invent intent the message never stated? |
| p4 | Small quotable | Does the bot cite a source line when routing? |
| p5 | Hidden library | Does the bot rely on knowledge it cannot name? |
| p6 | Goldfish | Does the bot remember context from earlier in the same thread? |
| p7 | Identity check | Does the bot know what it is and refuse tasks outside its job? |

---

## Worked Example: Northfield Ticket Router

**Bot:** Northfield ticket router — message in, queue out

**Stakes:** Support ops lead · Wrong queue = SLA miss · Rebuild freeze Friday 5pm

**Standard line:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**
- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.

---

## Board Marks (Caught / Slips / Hold)

| Row | Task | Verdict | Notes |
|-----|------|---------|-------|
| p1 | Bundle trap | **Slips** | "Where's my order? Also the promo code never applied." — two jobs, should open two tickets |
| p2 | Messy harmless | **Caught** | "Refund for wrong size — not a shipping question." — messy phrasing, single issue, routed correctly |
| p3 | Mind reader | **Hold** | Cannot confirm without live test — does the bot invent intent? |
| p4 | Small quotable | **Slips** | No quoted source line in routing decision |
| p5 | Hidden library | **Slips** | Bot may rely on unstated knowledge to route "Billing charged twice; chat said shipping had the tracking." |
| p6 | Goldfish | **Caught** | "It broke again after you fixed it yesterday." — bot handles thread context |
| p7 | Identity check | **Hold** | Cannot confirm without probing bot's self-knowledge |

---

## Use Defenses

When a row marks **Slips**, name the defense that would flip it:

| Slips Row | Defense | Status |
|-----------|---------|--------|
| p1 Bundle trap | Force a split when there are two jobs | **on** |
| p4 Small quotable | Require a quoted source line | **on** |
| p5 Hidden library | Require a quoted source line | **on** |

Defenses available:
- **split_bundles** — Force a split when there are two jobs: **on**
- **rewrite_mind_read** — Ban mind-reading verbs: **off**
- **name_source** — Require a quoted source line: **on**

---

## Go-Live Gate

**Gate style:** gs-b — Hold launch until Slips rows are addressed

**Slips to block:** 2

If the board shows 2 or more Slips rows, ship stops.

**Re-run trigger:** rr-a — Re-run this board when the bot's routing logic changes

---

## Running the Board for a Stranger's Bot

1. Collect the stranger's paste: bot name, stakes, standard line, sample messages, source
2. Walk each of the seven rows against their sample messages
3. Mark each row Caught / Slips / Hold
4. For each Slips row, name the defense that would flip it
5. Count Slips rows
6. Apply go-live rule: if Slips ≥ 2, ship stops
7. Return the board with marks, defenses, and go-live verdict

---

## Output Shape

```
Board: [bot name]
Standard: [their standard line]

p1 Bundle trap: [Caught/Slips/Hold] — [evidence from their messages]
p2 Messy harmless: [Caught/Slips/Hold] — [evidence]
p3 Mind reader: [Caught/Slips/Hold] — [evidence]
p4 Small quotable: [Caught/Slips/Hold] — [evidence]
p5 Hidden library: [Caught/Slips/Hold] — [evidence]
p6 Goldfish: [Caught/Slips/Hold] — [evidence]
p7 Identity check: [Caught/Slips/Hold] — [evidence]

Slips count: [n]
Defenses to flip Slips:
- [defense name]: [on/off]

Go-live rule: Block at 2 Slips. Re-run when routing logic changes.
Verdict: [Ship / Hold]
```
