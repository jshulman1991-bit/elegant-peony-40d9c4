# Active-deal / live-conversation removals — 2026-08-14

Rule (Jake): remove anyone in an **open deal with activity in the last 3 months**
(cutoff 2026-05-14). **Ownership was NOT changed** — sequence removal only.
Applied across all six reengage lanes, not just Mel's.

## Removed: 19 prospects (batch 1292, verified 0 active states remain)

| Reason | N | Notes |
|---|---|---|
| Live conversation — contact has replied | 14 | The severe class: most were already at an `auto_email` step, i.e. Touch 1 had gone out and automated follow-ups were firing at people who had replied |
| Open deal, contact is on the deal, active <3mo | 2 | Gregory Schuler (MRM Capital), Ed Lewis (Kensington Investment) |
| Colleague at same company on an active deal <3mo | 2 | Kenneth Oaks (KGBO Holdings), Stephanie Luedke (Neuberger Berman) |
| Cross-rep deal conflict | 3 | John McDonald (deal owned by Miguel, enrolled under Mel); John Castrucci (deal owned by Mel, enrolled under Miguel); Mark Cosens (deal owned by another user) |

By lane: #117 = 11, #119 = 3, #121 = 3, #120 = 1, #116 = 0.

## Deliberately KEPT (rule says 3 months)
Four contacts sit at companies with an OPEN deal whose last activity is older than
3 months — Richard Bachmann, Giovanna Carter, Zach Matula (Enterprise Products) and
David Lubar (Lubar & Co.). Under the stated rule these stay. Flip the cutoff and they go.

## Outside scope, flagged not actioned
- **David Janco** — has replied and is ACTIVE in sequence **#99** (`DROPPED * NON-MEMBER +
  Rep Mel Simms // PFCS 2026 EM#2`), which is not a reengage lane. Same harm, different
  campaign — someone should pull him there.
- 97 SEV3 "watch" contacts (logged activity in 90 days, no open deal) were left enrolled.

## New standing gate — add to canon
No batch ships without a deal-conflict check. A contact is ineligible if they, or a
colleague at the same company, are on an open deal (`hs_is_closed = false`) with activity
in the last 3 months; or if they show a reply/engagement signal in Outreach.
This check did NOT exist during the original build and must run before every enrollment.

## Method note
`numActiveSequences = 0` and sequence-state history do NOT reveal deal conflicts or
one-off rep email. Deals must be queried directly from HubSpot and joined on contact id,
email domain, and normalized company name (excluding free webmail domains and generic
tokens like 'family'/'capital'/'group').
