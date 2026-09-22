# ACMS Match Control — Admin Portal v4.48

Production Admin repository package for GitHub Pages.

## Production URLs
- Admin: https://qourts.github.io/ACMS_Admin/admin.html
- Player: https://qourts.github.io/ACMS_InterHospital/

## Supabase
The Admin HTML contains the browser-safe Supabase project URL and publishable key. The private ACMS publish key is intentionally NOT committed.

After deploying to GitHub Pages, open Admin > Settings > Public Player Portal Sync and paste the rotated private `acms_...` publish key on the authorized tournament computer, then click **Save Sync Settings** and **Test & Publish**.

Never commit the private ACMS publish key, an `sb_secret_...` key, or the database password.


## v4.48 — Click / Tap to Add Schedule Match
- Click or tap any truly empty Schedule matrix cell to open **Add Match to Schedule**.
- Restore an existing unscheduled match into that court/time slot.
- Or create an additional official preliminary match by selecting Division, Bracket, Team A and Team B.
- Duplicate pairings, same-time team conflicts, occupied slots, and closed courts are blocked.
- Manual preliminary matches persist across browser reloads/backups and publish through the existing Supabase public-state sync.
- When a division filter hides a match, the cell now shows that it is occupied instead of appearing empty.
- On the mobile Schedule view, each time group gets a touch-friendly **+ Add match** action when a court is available.
