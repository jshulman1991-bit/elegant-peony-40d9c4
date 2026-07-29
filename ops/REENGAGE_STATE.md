# FOX Reengage & Nurture — Operating State (Single Source of Truth)

> **Read this first.** Any AI or human working the reengage engine syncs from this file
> and the data in `ops/data/`. If you change canonical state (sequences, rules, lists),
> update this file in the same commit. Last updated: **2026-07-29** by Claude (Opus).

---

## 1. Mission

Reengage FOX's dropped members and stalled prospects through rep-owned Outreach
sequences, fed from HubSpot, enriched/verified through Clay — 30 sends/day/rep,
warmest-first, without ever touching a recently-dropped member.

## 2. Canonical systems & IDs

| System | Identity | Notes |
|---|---|---|
| HubSpot portal | `49700948` | CRM source of truth for contacts/companies |
| Outreach | FOX org | Sequences + sends. Full API access as of 2026-07-29 |
| Clay workspace | `Family Office Exchange` (id `332549`) | Enrichment + email waterfall. No subroutines configured |
| Repo (centralized folder) | `jshulman1991-bit/elegant-peony-40d9c4`, branch `claude/reengage-nurture-strategy-arch-m2jed9` | Architecture brief + this ops state |
| Power Automate | live as of 2026-07-29 | Automation layer — flows must reference THIS file's canonical IDs |

**Rep owner IDs (HubSpot):** Mel Simms `81561781` · Dave Blide `79929567` · Miguel López de Silanes `79929566`

## 3. Canonical Outreach sequences (the ONLY approved targets)

| Rep | Sequence | Outreach ID |
|---|---|---|
| Mel Simms | `FOX_2026_REENGAGE_FOWAF_FO_US_Mel` | **107** |
| Dave Blide | `FOX_2026_REENGAGE_FOWAF_Advisor_MFO_US_Dave` | **108** |
| Miguel López de Silanes | `FOX_2026_REENGAGE_FOWAF_INT_Miguel` | **109** |

- 2026-07-29 verification: the ~22 duplicate FOWAF sequences flagged in
  `ops/data/FOX_Outreach_Sequence_Cleanup.csv` are **no longer present** (archived). ✅
- New sequences observed 2026-07-29, NOT yet in the canon (decide + update here):
  - `#114 Mel Denver Dropped Member Reengage Invite - FOWAF` (Mel; contains **auto_email** steps — first automation in the family)
  - `#113 Mel Los Angeles Regional Reengage` (owner Jake)

### ⚠️ 2026-07-29 (evening): NEW segment-differentiated architecture live — #116–121 (owner Jake)
| ID | Sequence | Intended cohort (inferred from name) | Steps |
|---|---|---|---|
| 116 | `FOX_REENG_2026_RECENT_Mel` | Mel dropped-recent | 1 manual + 4 **auto** |
| 117 | `FOX_REENG_2026_LONG_Mel` | Mel dropped mid/deep | 1 manual + 5 **auto** |
| 118 | `FOX_REENG_2026_LONG_Dave` | Dave dropped mid/deep | 1 manual + 4 **auto** |
| 119 | `FOX_PROSPECT_REENG_2026_Mel` | Mel cold-inbound / dead-referral | 1 manual + 6 **auto** |
| 120 | `FOX_PROSPECT_REENG_2026_Dave` | Dave cold-inbound / dead-referral | 1 manual + 5 **auto** |
| 121 | `FOX_PROSPECT_REENG_2026_Miguel` | Miguel all-INT prospects | 1 manual + 5 **auto** |

**Enrollment audit 2026-07-29** (`ops/data/FOX_Outreach_Enrollment_Audit_2026-07-29.json`):
222 enrollments since Jul 27 — #117: 142 (75% on approved lists) · #119: 58 (**only 9% on
approved lists** — cohort source unknown, reconcile before more sends) · #116: 16 (38%) ·
6 inbound (expected). **Action taken:** Wendy Gartenberg (ANOMALIES: Never-a-Member
mis-tagged as dropped) was enrolled in #117 at an auto_email step — sequence state 7274
destroyed 2026-07-29, verified removed. No Current Members found enrolled.
**These sequences carry auto_email steps — enrollment = sending.**
OPEN DECISION: #116–121 vs #107–109 as canon. Until decided, the send-list CSVs keep
mapping to #107/108/109 (manual, safe); do NOT dual-enroll a contact in both families.
Enrollees must come FROM the AVAILABLE lists (enriched + verified + hold-checked) —
#119's 53 off-list enrollees bypassed that pipeline.
- Do **not** create new reengage sequences without recording them here.

## 4. Non-negotiable rules

1. **6-month drop hold.** A dropped member may not be contacted until ≥ 6 months after
   drop date (`COMPANY.membership_expiration_date` or `COMPANY.hs_v2_date_exited_customer`).
   **The relationship manager (RM) has final call** on releasing any held member.
   Verified 2026-07-24: 0 held members in any send list (newest drop in lists: 2026-01-14).
   Advisor-side (Dave) treatment is less strict per Jake; FO/INT (Mel/Miguel) strict.
2. **Eligibility gates** (all lists already filtered): no engine-excluded, no opt-outs,
   no quarantined, no prior hard bounce, no `reengagement_attempts_q >= 3`,
   no family members of current members, valid email syntax, no `[ZZZ-ARCHIVE]`/not-a-fit companies,
   no Current Members / Never-a-Member records.
3. **Priority order** (lists are pre-sorted): segment warmth
   (dropped-recent → cold-inbound → dead-referral → dropped-mid → dropped-deep)
   → **family tier (Wealth Owner → Rising Gen → other)** → prospect score desc →
   titled-first → last name. Reps work top-down, 30/day. Segments recomputed from real
   Dynamics drop dates where present. Personal domains rank normally (SFO principals use them).
4. **Personas (8):** Family Principal / Wealth Owner · FO Executive · FO Investment/Finance ·
   FO Operations/Admin · **Investment Advisor · Wealth Advisor · Specialist Advisor**
   (advisor split is mandatory — "Specialist" = every advisor that isn't the first two) ·
   Sponsor / Partner.
5. **Member count** must stay live/accurate in HubSpot (`membership_status` on COMPANY).

## 5. Send lists (deliverables) — `ops/data/`

Rebuilt 2026-07-29 from the Dynamics sync (`family_member`, `is_primary_contact`, SFO/MFO,
company drop dates). New columns: Family (Dynamics), Primary Contact, Office Type, Drop Date.

| File | Rep | Rows | Wealth Owner / Rising Gen | Target sequence | Runway @30/day |
|---|---|---|---|---|---|
| `FOX_Reengage_AVAILABLE_Mel_Simms.csv` | Mel | 1,558 | 347 / 520 | #107 | 51 days |
| `FOX_Reengage_AVAILABLE_Dave_Blide.csv` | Dave | 818 | 18 / 13 | #108 | 27 days |
| `FOX_Reengage_AVAILABLE_Miguel_Lopez_de_Silanes.csv` | Miguel | 253 | 38 / 2 | #109 | **8 days — refill needed** |

6-month hold re-audited against Dynamics drop dates (cutoff 2026-01-29): **0 breaches**;
`FOX_Reengage_HOLD_under6mo.csv` is the standing RM-gated hold file (currently empty).
`FOX_Reengage_ANOMALIES.csv`: 46 excluded (Current Members / Never-a-Member in dropped
segments — surfaced by the Dynamics sync; do not reengage without review).
Columns include `Email Status` + `Clay Suggested Email` (verification), persona, segment,
sequence mapping, HubSpot ID/URL. **First wave = top 150 rows of each file.**

### Email verification state (first wave, 450 contacts) — COMPLETE 2026-07-29
| Status | Count | Meaning |
|---|---|---|
| Verified ✓ | 96 | Clay independently confirmed the address |
| Review — newer address found | 13 | Clay found a DIFFERENT address → see `FOX_Reengage_EMAIL_REVIEW.csv`; swap before sending |
| Personal domain (stable) | 41 | gmail/icloud etc. — not corp-verifiable, low staleness risk |
| Unconfirmed (HubSpot hygiene only) | 300 | Clay couldn't source; protected by bounce/opt-out gates only |

Everything below row 150 per list = `Tail — not yet verified`.

### Clay ledger (paid searches — results retrieved 2026-07-29, never resubmit)
Chunks 1–3 taskIds (RECOVERED ✅):
`mcp-task_0tip654op575wVqvmxz` `mcp-task_0tip6578ri878d8XmY5` `mcp-task_0tip65gzRY8efqCVhYV`
`mcp-task_0tip65hsSCPno3psPxa` `mcp-task_0tip65urp8y2smgSted` `mcp-task_0tip65xVo2H9Nm5KVxk`
`mcp-task_0tip666EyNqTXQJZUsK` `mcp-task_0tip666hyNkathordHE` `mcp-task_0tip66hFqjvpKuP8eSM`
`mcp-task_0tip66sh6pxx6SSQ8Ti` `mcp-task_0tip66vS9hZQZPk9dt6` `mcp-task_0tip66ihrgoMEnwn99R`

## 6. HubSpot pending imports

- `ops/data/FOX_Reengage_HubSpot_Persona_IMPORT.csv` (854 rows) — apply after creating
  the 8 `hs_persona` options in HubSpot Settings.
- `engine_email_validation_status` is **unpopulated portal-wide**; write-back of Clay
  verification results is an open item (see §8).

## 7. Contract for every AI working this engine

1. **Sync from this file first**; treat `ops/data/` CSVs as the canonical lists.
2. **Never** add prospects to any sequence other than #107/#108/#109 without updating §3.
3. **Never** contact a <6-month drop. RM has final call on exceptions.
4. **Log Clay spends** in §5's ledger (taskIds) so no search is ever paid twice.
5. Changes to lists/rules/sequences ⇒ update this file **in the same commit/PR**.
6. Power Automate flows must read sequence IDs + field names from here, not hardcode copies.

## 8. Open items

- [x] Recover Clay chunks 1–3 → fold into lists → 0 "Pending" (done 2026-07-29)
- [ ] Human review of the 13 corrected addresses (`FOX_Reengage_EMAIL_REVIEW.csv`) → swap in HubSpot
      (3 are job moves — McDonald→AWM Associates, Arora→My Three Rocks, Greene→Sterling Seacrest
      Pritchard, Brutten→Brixton — confirm still in-persona before sending)
- [ ] Decide canon status of new sequences #113/#114 (esp. #114's auto_email steps)
- [ ] Create `hs_persona` options in HubSpot → run persona import (854)
- [ ] Write Clay verification results to `engine_email_validation_status` in HubSpot
- [ ] Extend persona classification to full pool (~1,800 remaining)
- [ ] Live member-count monitor + 6-month countdown automation (candidate for Power Automate)
- [ ] Verify tail (rows 151+) before reps reach dropped-mid/deep territory
