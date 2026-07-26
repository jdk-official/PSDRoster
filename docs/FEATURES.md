# PSDRoster Feature Scaffold

PSDRoster is moving from a transfer roster into an alliance operations board. The first build should keep the current roster and JSONBin merge-on-save model intact while adding new planning modules beside it.

## Alliance Activity Ledger

Purpose: Track who is actually contributing each week, not just who has the biggest power number.

Features:
- Weekly ledger by commander and week start date.
- Event columns for VS, Desert Storm, Marshal, Zombie Siege, donations, help activity, Enemy Buster, and manual officer notes.
- Status chips: reliable, available, away, needs nudge, no-show risk.
- Screenshot/OCR import routes that land in a review grid before shared save.
- Follow-up queue for missing activity answers or repeated event misses.

Suggested data shape:
- activityWeeks: [{ weekStart, members: [{ name, vsScore, stormStatus, marshal, zombieSiege, donations, helpCount, enemyBuster, availability, reliability, notes, updated }] }]

## Alliance Squad Power Record

Purpose: Turn power screenshots into a growth and readiness history.

Features:
- Dated snapshots for THP, Squad 1, Squad 2, Squad 3, squad types, T11, HQ, profession, and kills.
- Growth deltas since previous snapshot.
- Missing/stale data queue.
- Tank/Air/Missile balance view.
- Top rally leads and filler pool by squad type.

Suggested data shape:
- squadSnapshots: [{ capturedAt, source, members: [{ name, thp, squad1, squad2, squad3, squadType, squad2Type, squad3Type, t11, hqLevel, profLevel, kills }] }]

## Desert Storm Plan Creator

Purpose: Give R4/R5 a practical pre-war board for starters, substitutes, roles, and building assignments.

Features:
- Availability RSVP per commander.
- Auto-draft 20 starters plus substitutes using squad power, T11, activity, and officer trust flags.
- Balanced Team A / Team B split.
- Role assignment: rally lead, filler, scout, crate runner, hospital guard, flex.
- Building phase plan: Hospitals, Research Center, Information Center, Arsenal, Mercenary Factory, Nuclear Silo, Oil Wells.
- Copy/export plan for alliance chat.

Suggested data shape:
- stormPlans: [{ id, title, eventAt, status, starters, substitutes, assignments, phaseNotes, createdAt, updatedAt }]

## Weekly Alliance Report

Purpose: Produce a reset-day leadership summary with action items.

Features:
- Officer-ready text report.
- Roster totals, power totals, T11 count, squad data coverage.
- Top contributors and top growth once snapshots exist.
- Missing data and missing activity queues.
- Recommended Desert Storm starters/subs.
- Export as text first, then CSV/image later.

Suggested data shape:
- weeklyReports: [{ weekStart, generatedAt, summary, actionItems, topMembers, missingData, stormRecommendations }]

## Scaffold Feature

Purpose: Make the conversion safe and incremental.

Features:
- Top-level tabs for Roster, Activity Ledger, Squad Power, Desert Storm, Weekly Report, and Scaffold.
- Display-only generated summaries from the current roster before new writes are added.
- Add new top-level store arrays without changing existing roster units.
- Extend merge-on-save for each new array separately.
- Keep automation boundaries safe: screenshot in, plan out, human clicks in-game.

Build order:
1. Ship display scaffold and copyable generated Storm/report output.
2. Add local editable drafts for activityWeeks and stormPlans.
3. Add JSONBin merge support for new arrays.
4. Add OCR imports for activity and squad snapshots.
5. Add export/share surfaces for Storm plans and weekly reports.