# Transaction Reconciliation Engine

A pandas-based tool that matches transactions between two systems — an internal ledger (System A) and a bank/payment gateway report (System B) — handling merchant naming differences, small amount tolerances, and timestamp skew.

---

## Files

```
├── input_a.json          # System A transactions (internal ledger)
├── input_b.json          # System B transactions (bank/gateway)
├── merchant_map.json     # Canonical merchant name mappings
└── reconciliation.py     # Main script (or run in Colab)
```

**Outputs generated on run:**
```
├── matched_pairs.csv     # Matched A↔B pairs with Δamt and Δtime
├── unmatched_a.csv       # Transactions from A with no match
└── unmatched_b.csv       # Transactions from B with no match
```

---

## How to Run

### Google Colab
1. Upload `input_a.json`, `input_b.json`, and `merchant_map.json` to your session files
2. Paste the script into a notebook cell and run

### Local
```bash
pip install pandas
python reconciliation.py
```

---

## Matching Rules

A pair is matched if all three conditions are met:

| Rule | Condition |
|---|---|
| Merchant | Same after stripping punctuation/spaces, lowercasing, and applying the merchant map |
| Amount | Absolute difference ≤ 0.50 **or** relative difference ≤ 0.1% |
| Timestamp | Difference ≤ 120 seconds (converted to UTC before comparison) |

When multiple valid matches exist for one transaction, the pair with the **smallest time difference** is preferred (then smallest amount difference as a tiebreaker). Each transaction is used in at most one match.

---

## Merchant Normalization

The `merchant_map.json` file maps raw names to canonical ones:

```json
{
  "AMZN MKTPLACE": "Amazon",
  "Amazon Marketplace": "Amazon",
  "STARBUCKS 123": "Starbucks"
}
```

Before the map is applied, both the transaction merchants and the map keys are cleaned the same way — punctuation (`, - . `) removed, lowercased. This means `"AMZN. MKTPLACE"`, `"amzn mktplace"`, and `"AMZN MKTPLACE"` all resolve to `"amazon"` before matching.

Merchants not found in the map keep their cleaned form and can still match across systems if both sides produce the same cleaned string.

---

## Approach

```
Load A and B → Clean merchants → Parse timestamps to UTC
       ↓
Cross-join A × B on merchant (pandas merge)
       ↓
Filter by time window (≤120s) and amount tolerance
       ↓
Sort candidates by (time_diff ASC, amount_diff ASC)
       ↓
Greedy 1-to-1 assignment — claim best pair, mark both used, repeat
       ↓
Remaining rows → unmatched_a / unmatched_b
```

The merge step produces every possible A↔B pair within the same merchant group. Filtering and sorting then reduces this to a ranked candidate list, and the greedy loop picks non-overlapping pairs greedily.

---

## Dependencies

- Python 3.8+
- pandas
- json, re (standard library)
