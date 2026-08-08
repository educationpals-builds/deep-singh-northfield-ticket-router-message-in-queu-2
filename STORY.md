# Northfield ticket router — message in, queue out

## The bot under audit

The Northfield ticket router reads incoming support messages and assigns them to queues. The clear bar for this bot:

> A two-problem message opens two tickets.

Source: Last week's live queue export (10 messages).

---

## Sample messages tested

These five messages from the live queue ran through the board:

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.

---

## The seven trick tasks — what happened

| Row | Trick task | Verdict |
|-----|------------|---------|
| p1 | Bundle | **Slips** |
| p2 | Messy harmless | **Caught** |
| p3 | Mind reader | **Hold** |
| p4 | Small quotable | **Slips** |
| p5 | Hidden library | **Slips** |
| p6 | Goldfish | **Caught** |
| p7 | yt-identity | **Hold** |

Three rows slipped. Two rows held. Two rows caught.

---

## Defenses turned on

The builder enabled these defenses:

- **Force a split when there are two jobs** — on
- **Require a quoted source line** — on

One defense stayed off:

- Ban mind-reading verbs — off

---

## The go-live rule

Hold style: gs-b

Block threshold: **2** Slips rows stop ship.

Re-run trigger: rr-a

With three Slips rows (p1_bundle, p4_small_quotable, p5_hidden_library), the board blocks go-live until at least two of those rows flip to Caught or Hold.

---

## What remains

The Northfield ticket router cannot ship until the Slips count drops below 2. The defenses now active — split_bundles and name_source — address some failure modes, but the board still shows three Slips. The builder must either flip one more Slips row or accept the hold until the next re-run.
