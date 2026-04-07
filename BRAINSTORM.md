# Brainstorm — Tapped Goodz Landing Experience

## The Problem
When you tap a Goodz card, you get a bare page that says "You tapped a Goodz" with a Listen Now button. It's:
- Aesthetically flat — no sense of discovery or reward
- Cognitively empty — no context about what you're looking at
- A click barrier before music — why make them click to hear?

---

## Big Ideas

### 1. "Unwrap the Goodz" — The Album Art Reveal
**Concept:** Full-bleed playlist cover art as the entire background. On load, the art is hidden/blurred. A subtle animation "unwraps" it as the page loads — like peeling back layers. The playlist name and a play button float over it.

**Why it's compelling:** Makes the tap feel like unwrapping something. Album art is rich, emotional, and immediately contextualizes whose music you're about to hear.

**Technical:** Spotify Web API → `GET /playlists/{id}/images`. Fallback: og:image from the Spotify URL's HTML. CSS blur-up / scale animation.

---

### 2. "First Track" Teaser
**Concept:** Fetch the first track on the playlist. Show the song title, artist, album art. Have a lyric snippet (from a lyrics API or YouTube) animate in below. One tap to play the full playlist.

**Why it's compelling:** Curiosity — you see the first song, you want to hear it. Shows that something real is here, not just a link.

**Technical:** Spotify Web API → `GET /playlists/{id}/tracks` (first item). YouTube oEmbed for video. Musixmatch or LRCLIB for lyrics.

---

### 3. "Now Playing" Mode
**Concept:** The page looks like a phone in a dock — phone-shaped frame, screen shows the Spotify web player embedded. Feels physical, like a clock or a photo frame. Shows "TAPPED A GOODZ" as the label on the dock.

**Why it's compelling:** Transforms a URL into a physical object experience. The dock metaphor is warm and familiar.

**Technical:** Spotify iframe embed. CSS phone frame. Static layout, no JS animation needed.

---

### 4. Lyrics-First Reveal
**Concept:** Pull the first song's lyrics. Display them lyric-by-lyric with a type-writer or karaoke-style animation as the page loads. Below the lyrics: the full Spotify playlist embed. A "Listen along" vibe.

**Why it's compelling:** Lyrics make it feel personal and immediate. If it's a song you love, the lyrics hit you before the music does.

**Technical:** LRCLIB (free, no auth) for timed lyrics. Spotify iframe for playback. CSS karaoke animation.

---

### 5. "The Curator" — Artist + Playlist Combo
**Concept:** Split screen. Left side: artist photo (pulled from Spotify API or og:image). Right side: playlist name, track list preview (first 5 songs), and the Spotify embed. Shows context — who made this, why it matters.

**Why it's compelling:** If this is a fan collecting an artist's Goodz, they want to know whose Goodz they have. The artist identity adds emotional weight.

**Technical:** Spotify Web API for artist image + playlist data. Two-column CSS grid.

---

### 6. Ambient / Visualizer Mode
**Concept:** Abstract background animation using the dominant colors from the playlist cover art (color extraction). Playlist name fades in and out. One persistent "Play" button. Feels like a music visualizer screensaver.

**Why it's compelling:** Beautiful, low-effort, platform-like. Makes the tap feel premium.

**Technical:** ColorThief or vibrant.js to extract palette from cover art. CSS animated background. Spotify iframe.

---

## Supporting Ideas (smaller, composable)

- **Progress bar:** Shows how many people have tapped this card before you (fake or real)
- **"Tap count":** Display how many times this card has been tapped — social proof
- **Season/limited edition badge:** If it's a limited release, show the edition number
- **Quick-save button:** One tap to save playlist to your Spotify account (requires auth)
- **"Send to my speakers"** — Spotify Connect API to push to a device
- **Shareable tap:** Generate a shareable image/card showing what Goodz you tapped

---

## Technical Approach

### Spotify Web API
- Playlist cover image: `GET https://api.spotify.com/v1/playlists/{playlist_id}/images`
- First track + metadata: `GET https://api.spotify.com/v1/playlists/{playlist_id}/tracks`
- Artist info: `GET https://api.spotify.com/v1/artists/{id}`
- All of the above require a Spotify API token (client_credentials flow, no user login needed)

### Spotify Embed
```
<iframe style="border-radius:12px" src="https://open.spotify.com/embed/playlist/{playlist_id}?utm_source=generator" width="100%" height="352" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
```

### Lyrics APIs
- **LRCLIB:** `https://lrclib.net/api/get?artist={}&title={}` — free, no auth, returns timed lyrics
- **Musixmatch:** requires API key, rate limited
- **YouTube oEmbed:** `https://www.youtube.com/oembed?url=https://www.youtube.com/watch?v={id}&format=json` — free, no auth

### Color Extraction
- **ColorThief** (browser JS): pull dominant color from cover image
- **Vibrant.js:** extracts palette from image

### CORS Issue
Spotify Web API doesn't support CORS from browsers directly. Solutions:
1. Server-side API route (Vercel function) — token stays secret, proxy the request
2. Use a CORS proxy (e.g., `corsproxy.io`) — easier but less reliable for production
3. Use Spotify's oEmbed endpoint which IS CORS-friendly for embedding — limited metadata but no auth needed

### Spotify oEmbed (simplest path, no auth)
```
GET https://open.spotify.com/oembed?url=https://open.spotify.com/playlist/{playlist_id}
```
Returns: title, thumbnail_url, html (embed code), provider_name.
No lyrics, no track list, but cover art + embed in one call.

---

## What to prototype first

**Recommended path:**
1. **Prototype A — "Album Art Reveal"** (easiest win: visual impact, minimal API)
2. **Prototype B — "Now Playing Dock"** (physical metaphor, clean and achievable)
3. **Prototype C — "First Track + Lyrics"** (more complex, highest reward)

Start with A: uses Spotify oEmbed (no auth), CSS animation, full-bleed cover art.
