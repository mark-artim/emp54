# emp54.com marketing site — RETIRED

**This repository does not serve anything. Editing the pages here changes nothing that
anyone can see.**

It held the static marketing site that Vercel served at `emp54.com` until the DNS move on
**2026-08-21**. Since then everything — the marketing page and the application — is served by
the Django app on Railway.

## Where the live landing page actually is

Repo: **`mark-artim/vue-basic`** (locally `projects/vue-basic`).

- `django-backend/emp54_django/urls.py` — `home_view` serves the marketing page at `/`.
  It used to redirect to `/login/`; that was correct while Vercel owned the root, and became
  wrong the moment Railway started answering it — prospective customers were handed an
  employee login form instead of the page explaining what emp54 sells.
- `django-backend/core/views.py` — `landing_page`, the view behind both `/` and `/welcome/`.
- Read that repo's `CLAUDE.md`, section **Deployment**, before touching anything to do with
  domains, DNS or hosting. The Squarespace apex-forwarding rule in particular has a trap
  ("Maintain paths") that silently breaks every deep link when set wrong.

## The one thing here that still matters

`/ship54.html` was a real URL on the live site and links to it exist in the wild. It is not
served from this repo any more — the Django app redirects it to `/welcome/#suite`, and
`django-backend/core/tests_legacy_redirects.py` asserts that target still resolves. **If you
retire more URLs, add them there, not here.**

## Why the files here look convincing but are not live

`index.html` links the Client Portal straight to
`https://vue-basic-production-a944.up.railway.app/login/`, and `vercel.json` redirects
`/login` to the same place. Both are pre-DNS-move artefacts: `emp54.com` now *is* the Railway
app, so those hops no longer exist. The page renders fine locally, which is exactly what
makes it a trap — it looks like the site, and changing it does nothing.

Last real change here: **2026-02-05**.

## If you want to be sure nobody edits this by accident

Archive the repository on GitHub (Settings → General → Danger Zone → Archive). That makes it
read-only while keeping the history. Worth confirming first that no Vercel project is still
attached to it — it would not be serving `emp54.com`, but it may still be building.
