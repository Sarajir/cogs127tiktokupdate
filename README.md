# cogs127tiktokupdate

A static, TikTok-style demo: vertical swipe feed, comments, video replies, and a **full-screen feed that only shows user-uploaded comment videos** (no “For You” algorithm mock).

## Features

- **For You feed**: full-screen cards, like / double-tap like, share, bottom navigation
- **Related feed**: **swipe left** on the main feed to open a second vertical column; **swipe right** to return
- **Comments**: tap 💬 for a bottom sheet with text replies and **📹 video replies** (local file pick)
- **Comment-video fullscreen feed**: with comments open, **swipe left** to open a feed that mirrors the main layout but **only lists video uploads from this post’s comment thread**. Empty state explains how to add videos. **Swipe right** or **Back** returns to the comment sheet
- **Nested comments**: in the comment-video feed, 💬 opens a separate thread for that clip

## Run locally

No build step—serve the folder over HTTP:

```bash
cd douyin
python3 -m http.server 8888
```

Open [http://localhost:8888](http://localhost:8888).

You can also open `index.html` directly; some behavior may be limited under `file://`—a local server is recommended.

## Structure

```
.
├── index.html   # Single-file app (HTML + CSS + JS)
├── README.md
└── .gitignore
```

## Notes

- Single file, no framework
- Video comments use `URL.createObjectURL`; data is session-only and blob URLs break after refresh
- Main vs. “related” feeds use two static mock datasets; the **comment-video feed** only aggregates `type: 'video'` entries in `commentStore` for the current post

## Repository

[https://github.com/Sarajir/cogs127tiktokupdate](https://github.com/Sarajir/cogs127tiktokupdate)

## Disclaimer

Educational / demo only. UI is a loose imitation for learning and is **not** affiliated with ByteDance or the Douyin / TikTok product.
