# cogs127tiktokupdate

A static, TikTok-style demo: vertical swipe feed, comments, video replies, **AI prompt → GIF** in comments, and a **full-screen feed** for comment-only media (no “For You” algorithm mock).

## Features

- **For You feed**: full-screen cards, like / double-tap like, share, bottom navigation
- **Related feed**: **swipe left** on the main feed to open a second vertical column; **swipe right** to return
- **Comments**: tap 💬 for a bottom sheet with text replies, **📹 video replies** (local file pick), and **✨ Generate GIF**: describe a scene in the textarea → free [Pollinations](https://pollinations.ai) image → optional local **gif.js** loop → preview → **Post to comments**
- **Comment-media fullscreen feed**: with comments open, **swipe left** to open a feed that mirrors the main layout but **only lists videos and AI GIFs from this post’s comment thread**. **Swipe right** or **Back** returns to the comment sheet
- **Nested comments**: in the comment-media feed, 💬 opens a separate thread for that clip

## Run locally

No build step—serve the folder over HTTP:

```bash
cd douyin
python3 -m http.server 8888
```

Open [http://localhost:8888](http://localhost:8888).

You can also open `index.html` directly; some behavior may be limited under `file://`—a local server is recommended.

**How third-party services are “connected”:** see [CONNECTING.md](./CONNECTING.md) (Pollinations + jsDelivr need no keys; optional Supabase needs your own project).

## Structure

```
.
├── index.html   # Single-file app (HTML + CSS + JS)
├── README.md
└── .gitignore
```

## Notes

- Single file, no framework; **gif.js** is loaded from jsDelivr for client-side GIF encoding
- Video / AI GIF comments live in `commentStore` in the browser: **session-only** unless you add a backend (see earlier discussion)
- Pollinations is a third-party free image API; quality, latency, and availability are **not guaranteed**. If the image loads but the canvas is “tainted” (CORS), the app falls back to a **still AI image** in the comment (badge: “AI still”)
- Main vs. “related” feeds use two static mock datasets; the **comment-media feed** aggregates `type: 'video'` and `type: 'aiGif'` in `commentStore` for the current post

## Repository

[https://github.com/Sarajir/cogs127tiktokupdate](https://github.com/Sarajir/cogs127tiktokupdate)

## Disclaimer

Educational / demo only. UI is a loose imitation for learning and is **not** affiliated with ByteDance or the Douyin / TikTok product.
