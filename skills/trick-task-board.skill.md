# Trick-task board

> Portable assistant skill for auditing whether a bot's routing checks actually split the work.

## Skill metadata

```yaml
skill_id: trick-task-board
version: 1.0.0
loadable: true
runtime: any-assistant
```

## Purpose

Walk seven trick tasks against a stranger's bot, mark each **Caught / Slips / Hold**, name the **Use defense** that would flip each Slips row, and return a go-live rule.

---

## Worked example

**Bot under audit:** Northfield ticket router — message in, queue out

**Standard line:** A two-problem message opens two tickets.

**Sample messages from last week's live queue export (10 messages):**

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.

---

## Seven trick tasks

### p1 — Bundle trap
**Test:** Does the bot split a message that contains two distinct jobs?

*Example probe:* "Where's my order? Also the promo code never applied."

**Mark:** Slips

**Use defense:** Force a split when there are two jobs

---

### p2 — Messy-harmless trap
**Test:** Does the bot route correctly when the message is sloppy but the intent is clear?

*Example probe:* "Refund for wrong size — not a shipping question."

**Mark:** Caught

**Use defense:** —

---

### p3 — Mind-reader trap
**Test:** Does the bot invent context the customer never stated?

*Example probe:* "It broke again after you fixed it yesterday."

**Mark:** Hold

**Use defense:** Ban mind-reading verbs *(currently off — skip)*

---

### p4 — Small-quotable trap
**Test:** Does the bot cite a source line when making a routing decision?

*Example probe:* "Cancel the subscription but keep the open return."

**Mark:** Slips

**Use defense:** Require a quoted source line

---

### p5 — Hidden-library trap
**Test:** Does the bot rely on knowledge it cannot show?

*Example probe:* "Billing charged twice; chat said shipping had the tracking."

**Mark:** Slips

**Use defense:** Require a quoted source line

---

### p6 — Goldfish trap
**Test:** Does the bot forget earlier context in the same thread?

*Example probe:* Multi-turn thread referencing "the open return" from a prior message.

**Mark:** Caught

**Use defense:** —

---

### p7 — Identity trap (your trick task)
**Test:** yt-identity

*Example probe:* Message that tests whether the bot confuses its own identity with the customer's account.

**Mark:** Hold

**Use defense:** —

---

## Defense state

| Defense | Status |
|---------|--------|
| Force a split when there are two jobs | **on** |
| Ban mind-reading verbs | off |
| Require a quoted source line | **on** |

When a stranger says a defense is "still off," that means **Skip/unset** — do not invent a rewrite module.

---

## Go-live rule

**Slips to block:** 2

If the board shows **2 or more Slips rows**, ship stops.

**Gate style:** gs-b

**Re-run trigger:** rr-a

---

## Output shape

When invoked, return:

```
Board marks:
  p1_bundle: Slips → Use: Force a split when there are two jobs
  p2_messy_harmless: Caught
  p3_mind_reader: Hold
  p4_small_quotable: Slips → Use: Require a quoted source line
  p5_hidden_library: Slips → Use: Require a quoted source line
  p6_goldfish: Caught
  p7_your_own: Hold

Slips count: 3
Slips to block: 2

Go-live rule: BLOCKED — 3 Slips exceeds threshold of 2.
Re-run trigger: rr-a
```

---

## Invocation

A stranger pastes:
1. Their bot's name and job
2. What goes wrong when routing fails (stakes)
3. A few real messages the bot will face

The skill walks all seven trick tasks, marks each, names the Use defense for every Slips row, and returns the go-live rule with the block threshold.

Never output a coach question. Always output the board marks, defenses, and go-live rule.
