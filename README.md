# timeline

A two-person planning timeline. The app is a single HTML file; shared state lives in this repo as `state.json`. Each of you pastes a GitHub Personal Access Token into the app once, and from then on every edit auto-commits to the repo.

## How it works

- **Reads**: anyone who can see the repo can load the timeline.
- **Writes**: each save is a commit to `state.json` on `main`. The commit message is `update timeline <timestamp>`.
- **Conflicts**: if someone else saved between your last load and your save, the app notices, pulls the latest, and tells you "Out of date — pulling". Make your edit again.
- **Offline**: the last successfully loaded state is cached in your browser. If GitHub is unreachable you can still view it; saves will fail until you're back online.

## One-time setup (do this for each of you)

### 1. Open the app

- Either visit the GitHub Pages URL (once Julia enables it — see below), **or**
- Download `index.html` from the repo and open it locally in your browser.

### 2. Create a Personal Access Token (PAT)

1. Go to <https://github.com/settings/tokens?type=beta> (fine-grained tokens).
2. Click **Generate new token**.
3. Name: `timeline`. Expiration: whatever you're comfortable with (90 days is a reasonable default; you'll be prompted to regenerate).
4. **Repository access** → Only select repositories → `jalulia/timeline`.
5. **Permissions** → Repository permissions → **Contents: Read and write**.
6. Generate, copy the token (starts with `github_pat_...`), and **keep the tab open until you've pasted it into the app** — you can't view it again.

### 3. Paste it into the app

- Click the **Token** link in the top-right toolbar.
- Paste the token and hit OK.
- The token is stored only in this browser's `localStorage`. It's not sent anywhere except to `api.github.com`.

### 4. Pull and go

- Click **Sync** to pull the latest state.
- Edit. Every change debounces and auto-commits after ~1 second.
- Hit **Sync** anytime you want to pull the other person's recent edits.

## For Julia only — inviting Matt

1. Go to <https://github.com/jalulia/timeline/settings/access>.
2. Click **Add people**, enter Matt's GitHub username, choose **Write** role.
3. Send Matt the link to this README so he can do his own token setup.

## (Optional) Enable GitHub Pages so you can both open a URL instead of a file

1. <https://github.com/jalulia/timeline/settings/pages>
2. Source: **Deploy from a branch** → Branch: `main` → Folder: `/ (root)` → Save.
3. After a minute, the app will be live at <https://jalulia.github.io/timeline/>.

*Note: GitHub Pages is public even from a private repo only if you're on a paid plan. If you make the repo private and want a URL, either upgrade or just open `index.html` locally.*

## Files

- `index.html` — the app. Single file, no build step.
- `state.json` — created on first save. The source of truth for timeline data.
- `README.md` — this file.

## When things go wrong

| Symptom | Fix |
|---|---|
| Save indicator says **"Need token"** | Click **Token**, paste your PAT. |
| Save indicator says **"Auth failed"** | Token expired or lacks `Contents: write`. Regenerate it. |
| Save indicator says **"Out of date — pulling"** | Normal — the other person saved. Your change didn't go through; re-apply it. |
| Save indicator says **"Offline"** | No network or rate-limited. Unauthenticated reads are capped at 60/hour per IP. |
| You want to wipe the repo state and start fresh | Click **Reset** in the toolbar; it'll commit the seed as the new state. |
