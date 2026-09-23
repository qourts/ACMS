# ACMS Admin v4.55 — Navy First Paint + Normalized Win Rate → PQ

- Navy/Sky theme is now loaded before all legacy CSS, eliminating the green sidebar flash on first paint.
- Wildcard logic remains: Normalize match count → Normalized Win Rate → Normalized PQ → TD decision if still tied.

# ACMS Match Control — Admin Portal v4.55 — Normalized Win Rate → PQ Wildcard

Production Admin repository package for GitHub Pages.

## Production URLs
- Admin: https://qourts.github.io/ACMS_Admin/admin.html
- Player: https://qourts.github.io/ACMS_InterHospital/

## Supabase
The Admin HTML contains the browser-safe Supabase project URL and publishable key. The private ACMS publish key is intentionally NOT committed.

After deploying to GitHub Pages, open **Admin > Settings > Public Player Portal Sync** and paste the rotated private `acms_...` publish key on the authorized tournament computer, then click **Save Sync Settings** and **Test & Publish**.

Never commit the private ACMS publish key, an `sb_secret_...` key, or the database password.


## v4.55 — Fair wildcard normalization for three-pool divisions
For divisions with **3 pools/brackets**, semifinal qualification now uses this ACMS-specific wildcard rule:

> **Each Pool #1 automatically qualifies. The wildcard is selected from the Pool #2 teams after normalizing everyone to the same number of counted pool matches. Ranking is Normalized Win Rate first, then Normalized PQ.**

Normalization and ranking behavior:
- The portal finds the smallest pool size in the division and uses it as the common comparison field.
- A Pool #2 candidate from a larger pool has the match result against the lowest-ranked opponent excluded.
- If a pool is larger by more than one team, results against as many bottom-ranked opponents as necessary are excluded until every wildcard candidate is evaluated over the same number of opponents.
- The portal recalculates wildcard-only **W/L, Win Rate, PE, PC and PQ = PE ÷ (PE + PC)** from the retained matches.
- Highest **Normalized Win Rate** becomes the leading wildcard candidate.
- If Normalized Win Rate is tied, **Normalized PQ** breaks the tie.
- If both Normalized Win Rate and Normalized PQ are exactly tied, automatic selection stops and a Tournament Director decision is required.
- Official standings, official W/L records, official PE/PC and official PQ are never modified by this normalization.
- The Playoffs qualification panel displays the official record, normalized record, normalized Win Rate, excluded opponent/result, PE/PC and Normalized PQ so the calculation can be audited during the event.

Example retained in validation:
- A five-team pool #2 candidate with official **3–1, PE 37, PC 21, PQ .6379** has an **11–1** result versus the #5 team excluded when compared with a four-team pool.
- Wildcard comparison becomes **2–1, Win Rate 66.67%, PE 26, PC 20, Normalized PQ .5652** across three counted matches.
- A normalized **2–1** candidate ranks ahead of a normalized **1–2** candidate even if the 1–2 candidate has the higher PQ.

## v4.52 — Smart Add Match workflow
- Clicking/tapping a truly empty Schedule cell opens an operator-first **Add Match to Schedule** modal with the selected time block and court already fixed.
- The workflow is now **Match Type → Division → Category → Bracket/Official Slot → Team A → Team B**.
- Match Type options are **Pool Match, Semi-Final, Bronze Match, and Gold Match**.
- Division/category/bracket selections progressively filter the eligible teams and official knockout slots.
- Pool Match can restore an existing unscheduled pairing instead of creating a duplicate.
- Semi-Final, Bronze and Gold use the tournament's existing official playoff records rather than creating duplicate knockout slots.
- The modal performs an impact check before applying the schedule change.
- Hard integrity conflicts are blocked, including occupied target cells, closed courts, same-team selection, duplicate active pool pairings, ineligible team context, and same-time player conflicts.
- Operational overrides that are possible but unusual are shown as explicit warnings and require confirmation before they are applied.

## v4.52 — Automated Twice-to-Beat semifinals
ACMS currently uses this tournament-specific rule:

> **Twice-to-Beat applies only in the Semifinal Round. A semifinalist receives the Twice-to-Beat advantage only when that team completed its Pool Matches with zero losses.**

Automation behavior:
- Eligibility is calculated from the completed pool record, not from seed/rank alone.
- A semifinal with no undefeated participant remains a standard single-match semifinal.
- If exactly one semifinalist is undefeated, that team needs **1 semifinal-series win** to advance; the other team must beat it **twice**.
- If the undefeated team wins Game 1, the semifinal series resolves immediately.
- If the non-advantaged team wins Game 1, the portal automatically creates the required **Twice-to-Beat Decider** as an official unscheduled match.
- The decider is intentionally **not auto-assigned to a court/time**. An operator places it in an available Schedule cell so the system does not silently create court, rest-time, or player conflicts.
- If both semifinalists completed pool play undefeated, both receive the protection and the semifinal series becomes first to two wins.
- Gold and Bronze participants are generated from the resolved **semifinal series**, not merely the result of Game 1.
- Twice-to-Beat metadata and generated decider records persist through browser reloads and backups.

## v4.51 — Production hardening retained
- Blocks schedule moves/swaps that would create hard same-time player conflicts.
- Prevents swaps from moving another match into a closed source court.
- Treats `Timeout` as an active court-occupying state when closing courts.
- Preserves Called / On Deck reservations when Start, Reset, Remove or Forfeit validation/confirmation does not complete.
- Prunes stale Called / On Deck reservations before authoritative state saves.
- Keeps Start Match disabled when a called match becomes blocked by court/player/rest constraints.
- Corrects Preflight so legitimate TBD playoff slots awaiting qualification are not reported as broken assignments.
- Removes the hard-coded 20-minute match-duration assumption from Preflight recovery warnings; live rest enforcement remains based on actual completion timestamps.
- Checks known participants on partially populated knockout slots for same-time scheduling conflicts.
- Removes the duplicate live-clock DOM ID and updates every rendered operations clock instance consistently.

## v4.51 — Navy/Sky first paint retained
- Navy/Sky is declared on the root HTML element before first paint.
- The legacy green navigation fallback has been retired from desktop/mobile structural sidebar rules.
- The Navy/Sky runtime guard remains only as defensive protection against accidental theme-attribute mutation.

## Earlier scheduling updates retained
- v4.48 introduced click/tap empty-cell scheduling and unscheduled-match restoration.
- v4.49 introduced stage-aware scheduling for Prelims, Semifinals, Bronze and Gold plus stage-aware scoring.
- All existing scoring, rest-time, Callable, Start/End Match, court-state, persistence and Player Portal publishing behavior is retained unless explicitly changed above.
