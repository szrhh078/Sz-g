# Shahriar TV — Premium IPTV Streaming Platform

**Your Family Entertainment Hub**

A production-ready, Netflix-style IPTV streaming web application built with React 19, TypeScript, Vite, Tailwind CSS v4, Shadcn-style UI, Framer Motion, HLS.js, React Router, and Zustand.

## Getting Started

```bash
npm install
npm run dev      # start dev server
npm run build    # production build (outputs to dist/)
npm run preview  # preview the production build
```

## Features

### IPTV Engine
- Automatically fetches and parses three M3U playlists (Bangla TV, FIFA, Sports) from GitHub.
- Custom M3U parser (`src/lib/m3uParser.ts`) extracts channel name, logo, group/category, and stream URL.
- Automatic CORS-proxy fallback chain (`src/lib/playlistFetcher.ts`) if direct fetch is blocked.
- Channels are deduplicated, grouped, and cached in `localStorage` for 6 hours with stale-while-revalidate behavior.

### Video Player (`src/components/player/VideoPlayer.tsx` + `src/hooks/useHlsPlayer.ts`)
- Built on HLS.js with native HLS fallback for Safari.
- Auto play, auto-reconnect with exponential backoff (up to 5 attempts), buffer monitoring, error detection & retry UI.
- Fullscreen, Picture-in-Picture, Theater Mode, Mini floating player (draggable).
- Quality selector driven by HLS level manifest.
- Full keyboard shortcuts: Space/K play-pause, M mute, F fullscreen, P PiP, T theater mode, arrow keys for volume/channel switching, Esc exit fullscreen.

### Pages
- **Home** — hero carousel, continue watching, pinned, featured, trending, recently watched, favorites, sports/FIFA/Bangla rows.
- **Live TV / All Channels** — grid/list/compact views with category filters.
- **Category pages** — `/category/bangla`, `/category/sports`, `/category/fifa`.
- **Watch page** — full player with related channels sidebar, next/prev navigation.
- **Search** — instant fuzzy search with scoring, recent searches, category shortcuts.
- **Favorites / Pinned / History** — all persisted to localStorage via Zustand `persist`.
- **Settings** — family mode toggle, PWA install prompt, data management.
- **Admin Dashboard** (`/admin`) — playlist refresh per source, stream health checker (concurrent probes), channel statistics, category distribution chart.

### Design
- Dark theme, glassmorphism, gradient branding (`#e11d48` brand red), Sora/Inter typography.
- Framer Motion animations throughout (fade/slide/scale on scroll & route change).
- Fully responsive: mobile bottom nav + drawer sidebar, desktop top nav.

### PWA
- `vite-plugin-pwa` with auto-update service worker.
- Custom app icons (192/512, maskable variants), apple-touch-icon, manifest.
- Runtime caching: NetworkFirst for `.m3u`/`.m3u8` playlists, CacheFirst for images.

## Project Structure

```
src/
  components/
    channel/    — ChannelCard, ChannelGrid, ChannelCarousel
    common/      — Logo, PageHeader, GroupFilter, ViewModeToggle
    layout/      — Header, Sidebar, BottomNav, Layout
    player/      — VideoPlayer, MiniPlayer
    ui/          — shadcn-style primitives (button, dialog, dropdown, etc.)
  hooks/         — useHlsPlayer, useChannelSearch, useStreamHealth
  lib/           — m3uParser, playlistFetcher, utils
  pages/         — HomePage, WatchPage, LiveTVPage, SearchPage, AdminPage, etc.
  store/         — channelStore, userStore, playerStore (Zustand)
  types/         — shared TypeScript types
```

## Notes

- All user data (favorites, history, pins, preferences) is stored locally — nothing leaves the browser except playlist fetches and stream requests.
- "Family Access Mode" toggle in Settings is a foundation for content filtering by category.
- If a CORS error occurs fetching the M3U playlists, the app automatically retries through `corsproxy.io`, `allorigins.win`, and `codetabs.com` proxies in sequence.
