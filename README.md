# Molefe Records Collection

A single-file web app for browsing, playing, and enriching a family vinyl collection — 171 records across the found crates and Neville's Collection.

Everything lives in `index.html`: no build step, no framework. Open it locally to browse, or host it (see below) to unlock sign-ins.

## Features

- **Home** — daily pick hero (Comfort / Discovery modes), recently played, never played, and favourites shelves
- **Catalogue** — grid, list, and 3D **crate-flip** views; multi-select filters with removable chips; `/` to search, `←`/`→` to flip through records
- **Records** — generated vinyl-sleeve placeholders when art is missing, AI liner notes (facts, listen-for, context), Spotify album embeds, tracklists with credits and lyrics
- **Now Playing** — a spinning platter with a tonearm that drops on play, plus a Spotify companion embed
- **Discover** — recommendations computed from your own listening, including AI-drawn connections between records
- **Sessions** — listening diary with a shareable *Year in the Groove* stats card
- **Wheel** — spin to pick tonight's record

## Backend: Microsoft 365

The collection is stored in a SharePoint list (`MolefeRecords`) accessed through Microsoft Graph. Signed-out visitors browse a read-only offline copy.

One-time setup:

1. **Register an Entra app**: [entra.microsoft.com](https://entra.microsoft.com) → App registrations → *New registration* (single tenant).
2. Under **Authentication → Add a platform → Single-page application**, add the app's hosted URL as the redirect URI.
3. Under **API permissions**, add delegated `Sites.ReadWrite.All` (keep `User.Read`) and grant admin consent.
4. Paste the **Application (client) ID** into the app's Admin panel (⚙ → Microsoft Entra App Client ID). Optionally set a SharePoint site URL (defaults to the tenant root site).
5. Settings → Integrations → **Sign in**. On first sign-in the app creates the list and migrates the collection automatically (from Supabase if reachable, otherwise from built-in data).

## Integrations

| Integration | Needs | Configured in |
|---|---|---|
| Spotify embeds & deep links | Nothing — works out of the box | — |
| Spotify playlist export + bulk matching | A free [Spotify app](https://developer.spotify.com/dashboard) Client ID (SPA redirect = hosted URL) | Admin panel → Spotify Client ID |
| AI liner notes (Claude) | An [Anthropic API key](https://platform.claude.com/) | Admin panel → Anthropic API Key |
| Last.fm / Discogs track data | Free API keys | Admin panel |

## Hosting

Sign-ins (Microsoft and Spotify) require an https URL — the app can't complete OAuth from a `file://` page.

This repo ships a GitHub Pages workflow (`.github/workflows/deploy-pages.yml`). To go live:

1. Merge to `main`.
2. Repo **Settings → Pages → Source: GitHub Actions**.
3. The site deploys to `https://<user>.github.io/Vinyl-Records-App/` on every push to `main`. Use that URL as the redirect URI for both the Entra and Spotify apps.

## Admin

The app is browsable by anyone; editing is gated behind the in-app admin login (🔒 in the header). Admin unlocks editing, bulk fetch & enrich (art, bios, tracklists, credits, lyrics, Spotify matching, AI liner notes), verification badges, and data tools.
