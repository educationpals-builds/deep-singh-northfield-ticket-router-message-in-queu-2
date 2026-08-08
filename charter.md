# Charter: Northfield ticket router — message in, queue out

## Who this serves

Support ops leads who need to trust a ticket-routing bot before it touches live queues. Wrong queue assignments mean SLA misses, and rebuild freezes lock changes out every Friday at 5pm. This charter defines how the Trick-task board audits whether the router's checks actually split the work.

## The bot under audit

**Specimen:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**
- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.

## What the marks mean

| Mark | Definition |
|------|------------|
| **Caught** | The router handles this trick task correctly — no intervention needed. |
| **Slips** | The router fails this trick task — a defense must be turned on before go-live. |
| **Hold** | The router's behavior on this trick task is uncertain — manual review required before shipping. |

## The seven trick tasks

| Row | Trick task | Verdict |
|-----|------------|---------|
| p1 | Bundle — message contains two jobs | Slips |
| p2 | Messy harmless — noisy but single-queue | Caught |
| p3 | Mind reader — requires inferring unstated intent | Hold |
| p4 | Small quotable — short message with a clear queue signal | Slips |
| p5 | Hidden library — answer exists in docs but router doesn't surface it | Slips |
| p6 | Goldfish — repeat contact about same issue | Caught |
| p7 | Identity — message asks who the bot is or what it can do | Hold |

## Defenses in use

The following defenses are turned **on** to catch Slips rows:

- **Force a split when there are two jobs** — catches p1 (Bundle)
- **Require a quoted source line** — catches p4 (Small quotable) and p5 (Hidden library)

## Go-live commitment

**Hold style:** gs-b

**Block threshold:** Ship stops at **2** Slips rows.

**Re-run trigger:** rr-a

The Northfield ticket router cannot go live while 2 or more Slips rows remain unaddressed. Each Slips row must either flip to Caught by turning on its defense, or the team must accept the risk in writing.

## Commitment

This board re-runs whenever the trigger condition (rr-a) is met. No routing changes ship on Friday after 5pm without a fresh board pass showing fewer than 2 Slips rows.
