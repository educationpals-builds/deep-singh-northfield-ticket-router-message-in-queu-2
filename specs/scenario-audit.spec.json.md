{
  "spec_name": "Northfield ticket router — message in, queue out",
  "spec_version": "1.0.0",
  "standard_line": "A two-problem message opens two tickets.",
  "source": "Last week's live queue export (10 messages).",
  "vocabulary": {
    "marks": ["Caught", "Slips", "Hold"],
    "mark_definitions": {
      "Caught": "The bot handles this trick task correctly without intervention.",
      "Slips": "The bot fails this trick task; a defense can flip it.",
      "Hold": "Cannot determine pass/fail from current evidence; needs manual review."
    }
  },
  "board_rows": [
    {
      "task_id": "p1_bundle",
      "task_name": "Bundle split",
      "description": "Does the bot split a message with two problems into two tickets?",
      "test_message": "Where's my order? Also the promo code never applied.",
      "verdict": "Slips",
      "flip_defense": "split_bundles"
    },
    {
      "task_id": "p2_messy_harmless",
      "task_name": "Messy but harmless",
      "description": "Does the bot route correctly when the message is messy but the intent is clear?",
      "test_message": "Refund for wrong size — not a shipping question.",
      "verdict": "Caught",
      "flip_defense": null
    },
    {
      "task_id": "p3_mind_reader",
      "task_name": "Mind reader",
      "description": "Does the bot avoid guessing intent that isn't stated?",
      "test_message": "It broke again after you fixed it yesterday.",
      "verdict": "Hold",
      "flip_defense": "rewrite_mind_read"
    },
    {
      "task_id": "p4_small_quotable",
      "task_name": "Small quotable",
      "description": "Does the bot cite a source line when routing?",
      "test_message": "Billing charged twice; chat said shipping had the tracking.",
      "verdict": "Slips",
      "flip_defense": "name_source"
    },
    {
      "task_id": "p5_hidden_library",
      "task_name": "Hidden library",
      "description": "Does the bot reference the correct queue definition from its library?",
      "test_message": "Cancel the subscription but keep the open return.",
      "verdict": "Slips",
      "flip_defense": "name_source"
    },
    {
      "task_id": "p6_goldfish",
      "task_name": "Goldfish",
      "description": "Does the bot remember context from earlier in the same message?",
      "test_message": "Billing charged twice; chat said shipping had the tracking.",
      "verdict": "Caught",
      "flip_defense": null
    },
    {
      "task_id": "p7_your_own",
      "task_name": "Identity trick",
      "description": "Custom trick task: yt-identity",
      "test_message": null,
      "verdict": "Hold",
      "flip_defense": null
    }
  ],
  "defenses": {
    "available": [
      {
        "id": "split_bundles",
        "label": "Force a split when there are two jobs",
        "status": "on"
      },
      {
        "id": "rewrite_mind_read",
        "label": "Ban mind-reading verbs",
        "status": "off"
      },
      {
        "id": "name_source",
        "label": "Require a quoted source line",
        "status": "on"
      }
    ]
  },
  "go_live_controls": {
    "gate_sentence": "gs-b",
    "slips_to_block": 2,
    "rerun_trigger": "rr-a",
    "rule": "Block ship when Slips count reaches 2. Re-run this board on trigger: rr-a."
  },
  "sample_messages": [
    "Refund for wrong size — not a shipping question.",
    "It broke again after you fixed it yesterday.",
    "Where's my order? Also the promo code never applied.",
    "Cancel the subscription but keep the open return.",
    "Billing charged twice; chat said shipping had the tracking."
  ]
}