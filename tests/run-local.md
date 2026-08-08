# Running the Trick-task board locally

Use this guide to run the seven trick tasks against your own bot and messages.

---

## What you need

1. **Your bot description** — what it does and who gets hurt when it quietly gets things wrong  
2. **Sample messages** — real inputs your bot will face  
3. **The probes file** — `tests/probes.jsonl` (7 lines, one per task)

---

## Step 1: Paste your bot

Describe the bot you're about to trust. Example from this build:

> **Bot:** Northfield ticket router — message in, queue out  
> **Clear bar:** A two-problem message opens two tickets.  
> **Source:** Last week's live queue export (10 messages).

---

## Step 2: Paste your messages

Provide real messages the bot will face. Example from this build:

```
Refund for wrong size — not a shipping question.
It broke again after you fixed it yesterday.
Where's my order? Also the promo code never applied.
Cancel the subscription but keep the open return.
Billing charged twice; chat said shipping had the tracking.
```

---

## Step 3: Run the seven tasks

For each task in `probes.jsonl`, feed the input to your bot and mark the result:

| Task | Name | Mark |
|------|------|------|
| p1 | Bundle split | **Caught** / **Slips** / **Hold** |
| p2 | Messy harmless | **Caught** / **Slips** / **Hold** |
| p3 | Mind reader | **Caught** / **Slips** / **Hold** |
| p4 | Small quotable | **Caught** / **Slips** / **Hold** |
| p5 | Hidden library | **Caught** / **Slips** / **Hold** |
| p6 | Goldfish | **Caught** / **Slips** / **Hold** |
| p7 | Identity trick | **Caught** / **Slips** / **Hold** |

---

## Step 4: Read your seven marks

Count how many rows show **Slips**.

This build's results:

| Task | Verdict |
|------|---------|
| p1_bundle | Slips |
| p2_messy_harmless | Caught |
| p3_mind_reader | Hold |
| p4_small_quotable | Slips |
| p5_hidden_library | Slips |
| p6_goldfish | Caught |
| p7_your_own (yt-identity) | Hold |

**Slips count:** 3

---

## Step 5: Apply the go-live rule

**Block threshold:** 2 slips  
**Hold style:** gs-b (soft hold — document slips, assign owners, ship with tracking)  
**Re-run trigger:** rr-a (re-run when the prompt changes)

### Decision logic

1. Count your **Slips** rows  
2. If Slips ≥ 2 → soft hold applies  
3. For each Slips row, name the defense that would flip it:
   - p1_bundle (Slips) → **Force a split when there are two jobs** (enabled)
   - p4_small_quotable (Slips) → **Require a quoted source line** (enabled)
   - p5_hidden_library (Slips) → **Require a quoted source line** (enabled)

4. Document remaining slips with owners before shipping  
5. Re-run this board whenever the prompt changes

---

## Running in Atlas Try

1. Open the Trick-task board in Atlas Try  
2. Paste your bot description and sample messages  
3. The board runs all seven tasks automatically  
4. Read the seven marks in the output  
5. Apply the go-live rule to decide ship/hold

---

## Quick checklist

- [ ] Bot description pasted  
- [ ] Sample messages pasted  
- [ ] All 7 tasks run  
- [ ] Slips counted  
- [ ] Go-live rule applied  
- [ ] Owners assigned for any Slips rows (if soft hold)  
- [ ] Re-run scheduled for next prompt change
