# Connecting third-party services

This project talks to external services **only over HTTPS from the browser** (and from `curl` when testing). Nothing runs on your own server unless you add it later.

## 1. Already wired in the code (no account)

### Pollinations (AI image → then GIF in browser)

- **What it is:** Public image endpoint. Your `index.html` builds a URL like  
  `https://image.pollinations.ai/prompt/...`
- **API key:** Not required for basic use.
- **What you do:** Serve the site with a local HTTP server and open it in a browser (not `file://`). Example:
  ```bash
  cd douyin
  python3 -m http.server 8888
  ```
  Then open `http://localhost:8888`.
- **CORS:** Responses include `access-control-allow-origin: *`, so the browser can draw the image on a canvas when encoding the GIF (when the request succeeds).
- **Limits:** Free tier can return **429 Too Many Requests** or be slow. Wait and retry, change prompt, or try another network/VPN if blocked.

### jsDelivr (gif.js + worker)

- **What it is:** CDN scripts loaded in `index.html`:
  - `https://cdn.jsdelivr.net/npm/gif.js@0.2.0/dist/gif.js`
  - Worker URL passed to `GIF`: `.../gif.worker.js`
- **API key:** None.
- **What you do:** Same as above—normal internet access. If a corporate firewall blocks `cdn.jsdelivr.net`, GIF encoding may fail; the app then falls back to a still AI image in the comment.

---

## 2. Optional: Supabase (persist comments + videos for everyone)

Pollinations and jsDelivr **do not store your comments**. To let “anyone see anyone’s uploads,” you need a backend. **Supabase** has a generous free tier.

**I cannot log in or create projects for you** (needs your email/password in the browser). You do these steps once:

1. Go to [https://supabase.com](https://supabase.com) → **Sign up** (GitHub login is fine).
2. **New project** → pick region/password → wait until the dashboard is ready.
3. **Settings → API** → copy:
   - **Project URL** (`https://xxxx.supabase.co`)
   - **`anon` `public` key** (safe to put in frontend only if you lock down with RLS; for a class demo you still shouldn’t commit secrets to a public repo—use env or a private config file).

4. **Storage:** Create a bucket, e.g. `comment-videos`, set policies so authenticated or anonymous uploads match what you want (public demo often uses restrictive RLS + signed URLs—plan with your TA).

5. **Database:** Create a table, e.g. `comments` with columns like `post_id`, `body`, `media_url`, `created_at`.

6. In your app: add the Supabase JS client and `insert` / `select` instead of only `commentStore` in memory.

If you want this repo to include **ready-made Supabase SQL + minimal client code**, say so and we can add a follow-up PR (you would still paste your own URL and anon key locally, not in public GitHub).

---

## 3. Quick connectivity checks (terminal)

```bash
# Pollinations (may return 200 or 429 depending on rate limits)
curl -sI "https://image.pollinations.ai/prompt/hello?width=32&height=32&nologo=true" | head -n 5

# jsDelivr worker
curl -sI "https://cdn.jsdelivr.net/npm/gif.js@0.2.0/dist/gif.worker.js" | head -n 5
```

---

## Summary

| Service        | Account? | You already “connect” by…                          |
|----------------|----------|---------------------------------------------------|
| Pollinations   | No       | Using the site over HTTP; URLs are in the code. |
| jsDelivr       | No       | Same; scripts load from CDN.                      |
| Supabase (opt) | Yes      | Create project, copy URL + anon key, add client. |

No extra “connector” software is required on your Mac beyond a browser and, for local testing, `python3 -m http.server`.
