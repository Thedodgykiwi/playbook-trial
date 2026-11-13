# Playbook Trial

A practice app for testing the new-app setup.

**Purpose: a test bench for the playbook starter.** New starter features
(menu, theme, sign-in…) are tried here on Dev first, then go into
`app-playbook/templates/starter/`. It stays on **Dev only**: don't suggest
promoting it to Alpha, Staging or Production unless the user asks.
Plain HTML/CSS/JS, no build step, hosted on GitHub Pages.

**Choice card, no issues here:** for any change to the starter's pieces, show the
AskUserQuestion card with "Mock-up first" / "Build it now" (playbook `issues.md`).

## Playbook

General processes, design patterns and gotchas live in
https://github.com/Thedodgykiwi/app-playbook. Before setting up anything it
covers (new app, stages, deploys, Firebase, icons, design), attach that repo
to the session and read its README and the relevant guide.

**Keep the playbook growing.** Whenever work on this app teaches something
that would help *any* app (a gotcha, a missing or wrong step, a setup
process, a design or wording pattern that worked), update the right guide
in the playbook in the same session, commit it to the playbook's `main`, and
tell the user what was added. App-specific facts stay in this file instead.

## Stages and branches

| Branch    | Stage      | URL                                              |
|-----------|------------|--------------------------------------------------|
| `dev`     | Dev        | https://thedodgykiwi.github.io/playbook-trial/dev/             |
| `alpha`   | Alpha      | https://thedodgykiwi.github.io/playbook-trial/alpha/           |
| `staging` | Staging    | https://thedodgykiwi.github.io/playbook-trial/staging/         |
| `main`    | Production | https://thedodgykiwi.github.io/playbook-trial/                 |

Any push to one of these branches redeploys the site (`.github/workflows/deploy.yml`).

## Rules

- **All new work happens on `dev`.** Start with `git checkout dev && git pull`.
- **Never commit or push directly to `alpha`, `staging` or `main`.** Changes
  reach them only through the `promote` skill, one stage at a time.
- **Production (`main`) needs the user's explicit "yes"** for each release.
- **Rollbacks** (undo the last Production release) only with the promote
  skill's `rollback` command, after the user's "yes" and their reason.
- Hotfixes only when the user asks: branch from `main`, release, merge back into `dev`.

## Issues (bugs, requests, tweaks)

The repo's GitHub Issues are the to-do list (playbook `issues.md`).
**"Log it" means record only:** create the issue, don't change code. Fix
later on `dev` with `Issue #<n>` (not "Fixes") in the commit message; the
Production release closes the issue automatically. No client names or private details in
issues (public repo = public issues).

## Concept first (for bigger ideas)

Before a new feature or noticeable change to how the app looks or works,
offer a concept mock-up once (Claude artifact, 2–3 options, not in the repo).
Not for small tweaks or fixes. Details: playbook `working-with-the-user.md`.

## Working on a change

1. Make the change on `dev`.
2. Preview: `python3 -m http.server 8080` from the repo root (banner says "LOCAL").
3. Commit with a clear message, push to `dev`, confirm the deploy succeeded.
4. Ask the user to try it on the Dev URL. Promote only when they're happy.

## Working with the user

- Plain English; briefly define technical terms.
- Screenshot visual changes before pushing.
- Ambiguous → make a sensible choice and say so. Ask first for anything hard to undo.
- Report honestly what was and wasn't tested.
- End with status: what changed, committed/pushed, where to see it.

## Quality checklist (before pushing)

- No errors in the browser console.
- Existing features still work: the page loads without errors.
- Works at ~375px wide and on desktop.
- Labelled buttons/inputs, good contrast, keyboard works.
- One feature or fix per commit, clear message.

## Guardrails

- **No secrets or personal data in the repo** (it's public).
- **Ask before adding** libraries, third-party services, tracking/analytics,
  or anything that costs money or needs an account.
- **Don't delete or reset user data** without asking; migrate formats.
- Plain HTML/CSS/JS with no build step unless the user agrees otherwise.
- Never edit `stage.js` by hand for deploys.

## Notes

- Browser storage is shared by all stages (one web address): key it per stage.
- Firebase project `playbook-trial-app`,
  web app "Playbook Trial (web)"; Firestore `(default)` in `eur3` (Europe, can't
  move), Standard, free tier, delete protection on. Google sign-in on;
  authorised domains include `thedodgykiwi.github.io` and `localhost`.
- `firestore.rules` currently denies everything (no data stored yet). The first
  feature that saves data adds owner-only rules plus tests, then deploys them
  with `npx -y firebase-tools@latest deploy --only firestore:rules` before the app.

## Sign-in and cloud data (optional, off until set up)

`account.js` provides Google sign-in and Firestore access, dormant while
`firebase-config.js` is `null`. To switch it on, follow the playbook's
`firebase-auth-and-firestore.md` (Firebase project, database location, rules
for this app's data with tests, deployed before the app, emulator testing).
The app listens for `account:change` and saves its own data per stage.
