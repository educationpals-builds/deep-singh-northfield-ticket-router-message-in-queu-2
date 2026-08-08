# Trick-task board

A stranger describes the bot they're about to trust—what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The kit runs seven trick tasks against those messages, marks each **Caught / Slips / Hold**, names the defense that would flip each Slips row, and returns a go-live rule with the block threshold and re-run trigger.

---

## Worked example

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Sample messages:**
- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.

**Source:** Last week's live queue export (10 messages).

---

## The seven trick tasks

| Row | Trick task | Verdict | Example from sample messages |
|-----|------------|---------|------------------------------|
| p1 | Bundle ask | **Slips** | "Where's my order? Also the promo code never applied." — two jobs, router may file as one ticket |
| p2 | Messy but harmless | **Caught** | "Refund for wrong size — not a shipping question." — messy phrasing, router still routes correctly |
| p3 | Mind-reader ask | **Hold** | "It broke again after you fixed it yesterday." — requires context the router can't see |
| p4 | Small quotable | **Slips** | "Cancel the subscription but keep the open return." — subtle second job buried in one line |
| p5 | Hidden library | **Slips** | "Billing charged twice; chat said shipping had the tracking." — references prior conversation not in message |
| p6 | Goldfish ask | **Caught** | Repeated simple routing request — router handles without drift |
| p7 | Identity ask (your trick task) | **Hold** | Message asks router to confirm its own identity or role — outside routing scope |

---

## Defenses that catch Slips

These defenses are set to **Use**:

| Defense | What it does |
|---------|--------------|
| Force a split when there are two jobs | Catches p1 (Bundle ask) and p4 (Small quotable) — ensures multi-problem messages open multiple tickets |
| Require a quoted source line | Catches p5 (Hidden library) — forces router to cite where it found the routing signal |

Defenses set to **Skip**:
- Ban mind-reading verbs (off)

---

## Go-live rule

**Block at:** 2 Slips rows

**Hold style:** gs-b

**Re-run trigger:** rr-a

The board blocks ship when Slips count reaches **2** or more. With p1, p4, and p5 currently marked Slips (3 rows), ship is blocked until defenses flip at least two of those rows to Caught.

---

## One-paste rebuild

```
Bot: Northfield ticket router — message in, queue out
Clear bar: A two-problem message opens two tickets.
Messages:
- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.
Source: Last week's live queue export (10 messages).
```

Paste this block to rebuild the board against the same bot and messages.

---

## For strangers

Point this board at your own bot:
1. Describe what your bot does and who gets hurt when it quietly fails
2. Paste a few real messages it will face
3. The kit runs all seven trick tasks, marks each Caught / Slips / Hold
4. Every Slips row names the defense that would flip it
5. You get a go-live rule with your block threshold

Your board, your messages, your go-live decision.

<!-- educationpals-build-verified -->
