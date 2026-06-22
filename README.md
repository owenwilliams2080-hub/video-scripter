# Video Scripter

A tiny single-page app for planning Build's marketing videos. Each idea has a
**Concept / Hook / Body / CTA** and a status (idea → scripting → filmed → posted).

**Live app:** https://owenwilliams2080-hub.github.io/video-scripter/

---

## How it works (plain English)

- The whole app is one file: `index.html`. GitHub Pages serves it as a website.
- Your ideas are stored in **Supabase** (a hosted database), so they sync across
  every device you sign in on — laptop, phone, whatever.
- You sign in with your **email** (a one-tap magic link). No password.
- `ideas.json` is a list of starter ideas. The first time you sign in, they get
  copied into your account. After that they're yours to edit.

## Two ways ideas get in

1. **You add/edit them** in the app → saved to your Supabase account instantly.
2. **Claude adds them** by editing `ideas.json` and pushing → next time you open
   the app, any *new* ideas (ones you've never seen) appear automatically.
   - Editing your own ideas is never overwritten.
   - Deleting a seeded idea is permanent — it won't come back.
   - Note: changing the *text* of an existing idea in `ideas.json` won't update an
     idea already in your account (it only adds brand-new ones, matched by `id`).

## Adding new ideas (for Claude / future me)

Edit `ideas.json`, add an object to the `ideas` array with a **unique `id`**:

```json
{
  "id": "seed-ugc-something-new",
  "title": "UGC — ...",
  "status": "idea",
  "concept": "one-line what it is",
  "hook": "the first 3 seconds",
  "body": "beat-by-beat shot list",
  "cta": "what they should do next"
}
```

Commit and push. Refresh the live app → it shows up.

## Tech notes

- Backend: Supabase project `Workout Tracking App` (reused, fully isolated).
  - Tables: `video_scripter_videos`, `video_scripter_seeded`.
  - Row-level security scopes every row to its owner — nobody else can read your ideas.
  - The Supabase URL + publishable key in `index.html` are safe to be public;
    RLS is what protects the data.
- Hosting: GitHub Pages (static), auto-deploys on push to `master`.
- No build step, no dependencies to install. Supabase JS loads from a CDN.

## Login

Sign-in is a **6-digit code** emailed to you (no links, no redirects). Enter your
email → get a code → type it in. Your session persists per browser.

For the email to contain the code, the Supabase **Magic Link** email template must
include `{{ .Token }}`. Dashboard → **Authentication → Email Templates → Magic Link**.

## Organizing

- **Folders** group the sidebar. Set an idea's folder with the 📁 picker.
- **Drag a folder header** to reorder folders (order saved to your account).
- **Star** an idea (★ in the editor) to float it to the top of its folder.
- Collapse/expand folders by clicking the header chevron (remembered per browser).
