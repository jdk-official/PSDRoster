# Firebase Realtime Database setup (~5 minutes)

The app's shared store now targets Firebase RTDB. The old JSONBin bin exhausted
its lifetime request quota on 2026-07-26 (every request returns 403); its final
state is preserved in `backups/psd-ops-backup-2026-07-26.json`.

Until the steps below are done, the app runs in local-only mode ("changes save
to this device only").

## Why Firebase RTDB

- REST API is a near drop-in for the old JSONBin flow: `GET`/`PUT` on
  `<dbUrl>/store.json`.
- Free (Spark) plan has **no request-count cap** — limits are 1 GB storage and
  10 GB/month transfer. Our store is ~40 KB, so ~250k page loads a month fit.
- Reads are strongly consistent, and ETag + `if-match` gives real
  compare-and-set — the stale-read data-loss problem JSONBin had is gone for
  good (the app now re-merges and retries when a conditional PUT loses).

## Steps (you — needs a Google account)

1. Go to https://console.firebase.google.com → **Add project**.
   - Name: `psd-roster` (anything works). Google Analytics: **disable**.
2. In the left menu: **Build → Realtime Database → Create database**.
   - Location: `europe-west1` (Belgium — closest to the UK).
   - Start in **locked mode** (we'll set rules next).
3. On the database's **Rules** tab, replace the rules with:

   ```json
   {
     "rules": {
       "store": { ".read": true, ".write": true },
       "$other": { ".read": false, ".write": false }
     }
   }
   ```

   and click **Publish**. This is the same trust model as the old setup (the
   JSONBin access key was public in the page too): anyone with the URL can
   write, and the app's password gate is the deterrent. It can be tightened
   later with Firebase Auth without changing the data layout.
4. On the **Data** tab, copy the database URL shown at the top — it looks like
   `https://psd-roster-default-rtdb.europe-west1.firebasedatabase.app`.
5. Paste it into `index.html` → the `FIREBASE.dbUrl` constant (no trailing
   slash) — or just hand the URL to Claude, who will wire it, seed the store
   from the backup (including the edit-password hash), and verify reads,
   writes, and the 412 conflict path live.

## Seeding (Claude, once the URL exists)

```bash
curl -X PUT '<dbUrl>/store.json' -H 'Content-Type: application/json' -d @<backup-with-editHash>.json
```

The repo backup has `editHash` stripped; the original export (with the hash)
is in `~/Downloads`, or the hash can be recovered from any device's
localStorage (`up7-state`).

## Note on scripts/

The one-off maintenance scripts under `scripts/` still point at the dead
JSONBin bin. They are historical; any future batch work should target
`<dbUrl>/store.json` with the same GET → merge → conditional PUT pattern the
app now uses.
