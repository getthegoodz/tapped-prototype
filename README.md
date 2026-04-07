# Goodz Tapped — Prototype Project

## What is this
Exploring a better landed experience when you tap a Goodz NFC card. Currently: boring "You tapped a Goodz" → "Listen Now" button. Goal: make it feel like opening a gift.

## Current flow (baseline)
1. NFC tap → browser opens `getthegoodz.com/tapped?playlist_url=...&code=...`
2. Page shows: logo, "YOU TAPPED A GOODZ", "Listen Now" button (links to Spotify playlist), "Change playlist" link
3. Nothing else — no context, no music, no personality

## Constraints
- NFC triggers a browser URL open, not an app launch
- Spotify playlist URLs only (for now)
- Can't embed Spotify app directly, but can embed Spotify web player (iframe)
- Can use Spotify Web API to fetch playlist metadata (requires backend or CORS proxy)

## Proto setup
- Local: `Goodz/subprojects/tapped-prototype/`
- Testing: `https://tapped-prototype.vercel.app` (Vercel, mikerosenthal account)
- GitHub: `https://github.com/getthegoodz/tapped-prototype` (public)
- Workflow: push to `main` → auto-deploys to `tapped-prototype.vercel.app`
- Test URL: `https://tapped-prototype.vercel.app/?playlist_url=https://open.spotify.com/playlist/37i9dQZF1DXcBWIGoYBM5M&code=test`

## Key files
- `SKILL.md` — this file
- `BRAINSTORM.md` — idea dump
- `PROTOTYPES/` — individual prototype HTML pages
