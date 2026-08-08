# Scenario Analyzer — Northfield ticket router

How the analyzer reads a stranger's paste into the seven board rows and defenses for the Trick-task board.

---

## Input shape

A stranger pastes:

1. **Bot description** — what the bot does (e.g., "routes incoming support messages to the right queue")
2. **Stakes** — who gets hurt when it quietly gets things wrong (e.g., "SLA miss, wrong queue means escalation delay")
3. **Sample messages** — real messages the bot will face

---

## Parsing steps

### Step 1 — Extract the sample messages

Split the stranger's paste into individual messages. Each message becomes a test input for the seven board rows.

**Worked example (builder's specimen):**

Source: Last week's live queue export (10 messages).

Messages extracted:
- "Refund for wrong size — not a shipping question."
- "It broke again after you fixed it yesterday."
- "Where's my order? Also the promo code never applied."
- "Cancel the subscription but keep the open return."
- "Billing charged twice; chat said shipping had the tracking."

---

### Step 2 — Run each message through the seven board rows

For each message, apply the seven trick tasks in order:

| Row | Task ID | What it checks |
|-----|---------|----------------|
| p1 | p1_bundle | Does the message contain two or more distinct jobs? |
| p2 | p2_messy_harmless | Does messy phrasing cause a wrong route even when intent is clear? |
| p3 | p3_mind_reader | Does the bot invent context the message never stated? |
| p4 | p4_small_quotable | Does the bot cite a source line, or route without evidence? |
| p5 | p5_hidden_library | Does the bot rely on knowledge it cannot name? |
| p6 | p6_goldfish | Does the bot forget earlier context in the same thread? |
| p7 | p7_your_own (yt-identity) | Does the bot confuse the customer's identity with another record? |

---

### Step 3 — Mark each row

For each task, assign one mark:

- **Caught** — The bot handles this trick correctly
- **Slips** — The bot fails this trick silently
- **Hold** — Cannot confirm pass or fail; needs human review

**Builder's board marks (Northfield ticket router):**

| Task | Mark |
|------|------|
| p1_bundle | Slips |
| p2_messy_harmless | Caught |
| p3_mind_reader | Hold |
| p4_small_quotable | Slips |
| p5_hidden_library | Slips |
| p6_goldfish | Caught |
| p7_your_own (yt-identity) | Hold |

---

### Step 4 — Name the Use defense for each Slips row

When a row marks **Slips**, the analyzer names the defense setting that would flip it.

Available defenses:

| Defense ID | Label | Status |
|------------|-------|--------|
| split_bundles | Force a split when there are two jobs | on |
| rewrite_mind_read | Ban mind-reading verbs | off |
| name_source | Require a quoted source line | on |

**Slips → Defense mapping:**

| Slips row | Use defense |
|-----------|-------------|
| p1_bundle | split_bundles (on) |
| p4_small_quotable | name_source (on) |
| p5_hidden_library | name_source (on) |

---

### Step 5 — Apply the go-live rule

Count the Slips rows. Compare to the block threshold.

**Builder's go-live rule:**

- **slips_to_block:** 2
- **Gate style:** gs-b (hard hold — do not ship until Slips count drops below threshold)
- **Re-run trigger:** rr-a (re-run this board when the bot's routing logic changes)

**Evaluation:**

Current Slips count: 3 (p1_bundle, p4_small_quotable, p5_hidden_library)

Block threshold: 2

**Result:** Ship blocked. Slips count (3) exceeds threshold (2).

---

## Output shape

The analyzer returns:

```
Board marks:
  p1_bundle: Slips → Use defense: split_bundles
  p2_messy_harmless: Caught
  p3_mind_reader: Hold
  p4_small_quotable: Slips → Use defense: name_source
  p5_hidden_library: Slips → Use defense: name_source
  p6_goldfish: Caught
  p7_your_own (yt-identity): Hold

Slips count: 3
Block threshold: 2
Go-live rule: Ship blocked until Slips < 2
Re-run trigger: When the bot's routing logic changes
```

---

## Stranger paste example

A stranger pastes:

> My bot routes customer emails to Sales, Support, or Billing queues. When it picks wrong, the ticket sits for 24 hours before someone notices. Here are three messages it will see:
>
> "I want to upgrade my plan but also dispute last month's charge."
> "The thing I ordered is broken."
> "Why did you charge me twice? My tracking number doesn't work either."

The analyzer:

1. Extracts the three messages
2. Runs each through the seven board rows
3. Marks Caught / Slips / Hold for each
4. Names the Use defense for each Slips row
5. Compares Slips count to slips_to_block (2)
6. Returns the go-live rule

---

## Standard line reference

The builder's standard line for the Northfield ticket router:

> A two-problem message opens two tickets.

This standard informs how p1_bundle (bundle detection) should behave — when a message contains two problems, the bot should create two tickets, not one.
