# WAPDA Town Sports

Player registration, tournaments, and live scoring for Football, Cricket,
Badminton, and Volleyball — built as a self-contained web app (single HTML
file) with offline support and installable-app behavior (PWA).

## Files in this folder

- `index.html` — the entire app (UI, styling, and logic in one file, with
  all sport background photos embedded directly inside it)
- `manifest.json` — makes the app installable ("Add to Home Screen") with
  its own name, icon, and colors
- `service-worker.js` — caches the app so it still opens without internet
  after the first visit
- `icon-192.png`, `icon-512.png` — app icons used for the home-screen icon
  and the Android app icon

## 0. Set up the shared database (required — do this first)

This app now stores all data in a shared **Supabase** database instead of
each visitor's own browser, so every device sees the same players and
tournaments. Before deploying:

1. Run the SQL in `supabase-schema.sql` (included in this folder) inside
   your Supabase project: **SQL Editor → New query → paste the whole file
   → Run**. This creates the `players` and `tournaments` tables with the
   right security rules.
2. The app is already configured with this project's URL and publishable
   key (in `index.html`), so no further setup is needed unless you create
   a different Supabase project later — in which case, update the
   `SUPABASE_URL` and `SUPABASE_KEY` constants near the top of the
   `<script>` section in `index.html`.

## 1. Deploy on GitHub Pages (free hosting)

1. Create a new GitHub repository (e.g. `wapda-town-sports`).
2. Upload all the files in this folder to the root of that repository
   (drag-and-drop on github.com works, or use `git add . && git commit -m
   "Initial deploy" && git push`).
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment", set **Source** to "Deploy from a branch",
   choose the `main` branch and `/ (root)` folder, then **Save**.
5. GitHub will give you a live URL after a minute or two, typically:
   `https://<your-username>.github.io/<repo-name>/`
6. Open that link — your app is now live and shareable with anyone.

Every registration, tournament, and score entered by a visitor is saved in
**that visitor's own browser** (there's no shared server database), so each
person managing the complex should bookmark the page on their own device.
Use the in-app **Export JSON** buttons to back up or move data between
devices.

## 2. Turn it into an installable app (no APK needed)

Once it's live on GitHub Pages, anyone can install it like a real app
straight from Chrome on Android:

1. Open the GitHub Pages link in Chrome on an Android phone.
2. Tap the **⋮** menu → **Add to Home screen** (or Chrome may prompt this
   automatically).
3. It installs with its own icon and opens full-screen, no browser bar —
   indistinguishable from a normal installed app for most everyday use.

This works because of `manifest.json` and `service-worker.js` — no extra
steps needed.

## 3. Generate an actual .apk file

Building a real Android `.apk` requires the Android SDK and Gradle build
tools, which aren't available in this environment, so that step needs to
happen on your end. The most reliable free way to do it:

1. Go to **[https://www.pwabuilder.com](https://www.pwabuilder.com)**.
2. Paste in your GitHub Pages URL (from step 1) and click **Start**.
3. PWABuilder will read your `manifest.json` automatically and show a
   readiness score for Android/iOS/Windows packaging.
4. Click the **Android** package option, choose the default settings (or
   adjust the package name, e.g. `com.wapdatown.sports`), and click
   **Generate**.
5. Download the resulting `.apk` (or `.aab` for the Play Store) — it's a
   real installable Android app wrapping your site, generated and signed
   for you.
6. To install it on a phone, transfer the `.apk` file to the device and
   open it (you may need to allow "install from unknown sources" in
   Android settings if you're not going through the Play Store).

If you ever want to publish it on the Google Play Store, PWABuilder also
produces the `.aab` bundle format Play Store submissions require, along
with a checklist for the required store listing assets.

## Notes

- All data (players, tournaments, scores) now lives in your **Supabase**
  database, shared live across every device and browser — not stored
  locally anymore. Anyone with the link can register players and update
  scores, since there's no login system built in yet.
- The sport background photos are embedded directly inside `index.html`
  as base64 data, so there's nothing else to upload for them to show up.
- `index.html` is a little over 600 KB because of the embedded photos —
  well within GitHub's limits and loads quickly on any normal connection.
- If you ever want to restrict *editing* to staff only (e.g. a shared
  password to unlock scoring/registration), that's a reasonable next step
  — just ask.
