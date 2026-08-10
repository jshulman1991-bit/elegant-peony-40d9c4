# Mel reengage enrollment — executed 2026-08-10

Batch source: `ops/data/mel_enrollment_batch_FINAL.csv` (423 rows, three adversarial review rounds).
Enrolled by this session via Outreach `sequence_add_prospects`. All lanes verified ACTIVE
(not among the 23 paused sequences) and Touch 1 is `manual_email` — **no email sends without
Mel actioning the task**. All states attribute to user id 8 (Mel Simms), not the sequence owner.

| Lane | Sequence | Enrolled | Outreach batch ids |
|---|---|---|---|
| 116 | FOX_REENG_2026_RECENT_Mel | 2 | 1271 |
| 117 | FOX_REENG_2026_LONG_Mel | 408 | 1273, 1275, 1276, 1277, 1278 |
| 119 | FOX_PROSPECT_REENG_2026_Mel | 10 | 1272 (12 added, 2 removed) |
| | **Total** | **420** | 0 failures across all batches |

Post-enrollment lane totals: #116 = 17 states, #117 = 555 (158 active + 378 pending),
#119 = 104. The pending pool is Outreach's 30/day throttle metering activation — intended.

**Runway: 420 contacts / 30 per day = 14 business days.**

## Held back from the batch (3)
Flagged by review as having engagement timestamps but no visible outbound mail — could be a
call or an inbound reply that `emails_search` cannot see. Held for rep confirmation rather
than enrolled:
- 2329 Theresa Altobelli (enrolled to 119 then removed, state 8089)
- 4229 Kirby Rosplock (enrolled to 119 then removed, state 8094)
- 5532 Olivier Perier (dropped from the 117 chunk before submission)

## Governor conflict — needs a ruling
Canon specifies a **25/rep/week** governor. 30/day = 150/week, 6x that. Jake directed
30/day combined on 2026-08-10. Enrollment is staged (pending), and the manual Touch 1 means
actual send volume is whatever Mel works — but the canon figure should be reconciled.

## Enrollment mechanics worth reusing
- Batches over ~100 return `state: "confirming"` and **do not apply**. Either re-submit with
  `confirmedBatchId` + `confirmedCount`, or pass `skipConfirmation: true`.
- Newly added prospects land in `pending`, not `active`; query both or totals look wrong.
- Verify attribution after enrolling: check the state's `user` field is the rep, not the
  sequence owner (these sequences are owned by Jake but must send as Mel).
