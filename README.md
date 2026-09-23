# ACMS Match Control — Admin Portal v4.52 — Smart Match Scheduling

Production Admin repository package for GitHub Pages.

## Production URLs
- Admin: https://qourts.github.io/ACMS_Admin/admin.html
- Player: https://qourts.github.io/ACMS_InterHospital/

## Supabase
The Admin HTML contains the browser-safe Supabase project URL and publishable key. The private ACMS publish key is intentionally NOT committed.

After deploying to GitHub Pages, open **Admin > Settings > Public Player Portal Sync** and paste the rotated private `acms_...` publish key on the authorized tournament computer, then click **Save Sync Settings** and **Test & Publish**.

Never commit the private ACMS publish key, an `sb_secret_...` key, or the database password.

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
