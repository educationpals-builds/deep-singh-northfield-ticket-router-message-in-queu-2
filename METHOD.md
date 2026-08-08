# Trick-task Board (GOVERN)

A method for auditing whether your bot's checks actually split the work before you ship.

---

## The Seven Board Rows

The board runs seven trick tasks against your bot's real messages. Each row gets one mark.

| Row | Trick Task | What It Tests |
|-----|------------|---------------|
| p1 | Bundle trap | Does the bot split a message with two jobs into two tickets? |
| p2 | Messy harmless | Does the bot handle sloppy but single-issue messages without over-splitting? |
| p3 | Mind reader | Does the bot infer intent it cannot quote from the message? |
| p4 | Small quotable | Does the bot cite a source line when routing? |
| p5 | Hidden library | Does the bot rely on knowledge it cannot name? |
| p6 | Goldfish | Does the bot remember context from earlier in the same message? |
| p7 | Identity ask | Does the bot handle requests that probe its own role or limits? |

---

## The Three Marks

Every row receives exactly one mark:

### Caught
The bot handled this trick task correctly. The check worked as intended.

### Slips
The bot failed this trick task. The check did not catch the problem. A defense exists that would flip this row to Caught.

### Hold
The bot's behavior on this trick task is ambiguous or needs human review before shipping. Neither pass nor fail — requires judgment.

---

## Defenses: Use and Skip

Each Slips row names a defense setting that would flip it to Caught.

**Use** — Turn this defense on. The bot will enforce this rule before routing.

**Skip** — Leave this defense off. You accept the risk that this trick task may slip through.

### Available Defenses

| Defense | What It Does |
|---------|--------------|
| Force a split when there are two jobs | When a message contains two distinct problems, the bot opens two tickets instead of one. |
| Ban mind-reading verbs | The bot cannot use verbs that imply inferred intent without a quoted source. |
| Require a quoted source line | Every routing decision must cite a specific line from the customer message. |

---

## How the Board Works

1. Paste your bot's real messages
2. Run all seven trick tasks against those messages
3. Mark each row Caught, Slips, or Hold
4. For every Slips row, identify which defense would flip it
5. Decide Use or Skip for each defense
6. Set your go-live rule: how many Slips rows block ship?
7. Set your re-run trigger: when must this board run again?

The board does not tell you whether to ship. It tells you what slipped, what would catch it, and holds you to the threshold you set.

---

## Worked Example: Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Sample messages tested:**
- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.

**Board results:**

| Row | Trick Task | Mark |
|-----|------------|------|
| p1 | Bundle trap | Slips |
| p2 | Messy harmless | Caught |
| p3 | Mind reader | Hold |
| p4 | Small quotable | Slips |
| p5 | Hidden library | Slips |
| p6 | Goldfish | Caught |
| p7 | Identity ask | Hold |

**Defenses turned on:**
- Force a split when there are two jobs (Use)
- Require a quoted source line (Use)

**Go-live rule:** Block ship when Slips ≥ 2.

**Re-run trigger:** rr-a

---

## What This Method Does Not Do

- It does not score your bot overall
- It does not compare your bot to other bots
- It does not guarantee zero failures after ship

It surfaces the specific trick tasks your checks miss, names the defense that would catch each one, and holds you to the threshold you wrote down.
