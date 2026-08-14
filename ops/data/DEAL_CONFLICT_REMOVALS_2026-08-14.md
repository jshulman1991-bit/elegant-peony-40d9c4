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

---

## Round 2 — adversarial review found 4 MORE, all in Dave's lanes (removed, batch 1293)

The first pass built identity from `mel_pool_fresh.json` only, never loading Dave's or
Miguel's pools. 395 of 1,071 enrollees (37%) therefore had no email/company and could not
be joined to deals at all. Four confirmed conflicts were sitting in that blind spot:

| pid | Name | Lane | Deal | Stage | Deal owner | Last activity |
|---|---|---|---|---|---|---|
| 7529 | Kate L. Anderson | 118 Dave | Baker Tilly US LLP | Proposal & Negotiation | **Mel** | 2026-08-12 |
| 2342 | Katie Egan | 118 Dave | ArentFox Schiff LLP | Workshop RSVP | **Mel** | — |
| 1361 | Stacy Rush | 118 Dave | Pathstone Membership, **$33,000** | Solution | Dave | 2026-08-12 |
| 1220 | Emily Barbour | 120 Dave | Balentine IWAC Membership | Solution | Dave | 2026-07-16 |

**The cross-rep test had been run in one direction only.** The first pass concluded "zero
Dave-owned deals under Mel's enrollment", which was true but the wrong question. The real
exposure runs the other way: **Mel-owned deals under Dave's enrollment** (Kate Anderson,
Katie Egan). Kate Anderson is a colleague of John Castrucci on the *same* Baker Tilly deal
the audit had already flagged — it held the deal and still missed her.

**Running total removed: 23** (19 in batch 1292 + 4 in batch 1293). Ownership untouched.

## Coverage — honest state
Identity resolvable per lane (live enrollees): #117 92%, #118 86%, #116 50%,
**#119 21%, #120 4%, #121 4%**. The prospect lanes were enrolled from a source outside the
reengage pool files, so ~384 people still cannot be joined to deals by email or company.
A follow-up harvest of Outreach `account.name` for those prospects is in progress.
Treat the 23 as a floor, not a ceiling, until that lands.

Also unverifiable: `lane_signal_summary` reports 259 enrollees with engagedScore > 0 but
only 61 could be named — roughly 200 engaged prospects remain unidentified.
HubSpot's own reply fields (`hs_email_last_reply_date`, `sales_email_replied`) are empty on
all 423 enriched rows, so every conversation signal came from Outreach.
