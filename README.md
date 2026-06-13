# ultimate-uploader-V100
Cross-post videos to YouTube, Instagram, TikTok and Bluesky — auto-format, watermark removal, auto-pull, scheduler, and mirror mode.
Ultimate Uploader

A single-file terminal app for cross-posting short-form video to YouTube, Instagram, TikTok, and Bluesky — with auto-formatting, watermark removal, source watching, scheduling, and a "post once, publish everywhere" mirror mode.

It runs as a TUI (text user interface) in your terminal.


Features


Multi-platform publishing — push one video to YouTube + Instagram + TikTok + Bluesky in a single run.
Per-platform captions — one caption with {platform} and date/emoji variables; each platform gets its own rendered copy.
Cross-platform dedup — a video is never double-posted to the same platform (hash-based history).
Auto-format / resize — reshape each clip to the right aspect ratio per platform (9:16 for Reels/Shorts/TikTok, 1:1 for IG feed, 16:9 for normal YouTube), pad-with-blur or crop-to-fill.
Watermark removal — corner logo blur (delogo) or edge crop before reposting.
Auto-pull / source watchers — give it a channel/playlist/profile URL and it auto-downloads new uploads into your workspace.
Content calendar / scheduler — queue posts for a specific date/time; a background scheduler fires them automatically.
Mirror mode — watch your own profiles; when you post to one platform, the clip is auto-cross-posted to the others (source excluded).
Per-platform account management — credentials stored in your OS keychain, never in the code.


The 12 tabs

Cross-Post · TikTok · Auto-Pull · Schedule · Bluesky · Mirror · IG Download · IG Auto Post · YT Single · YT Channel · YT Batch · YT Auto Post


Requirements


Python 3.8+
ffmpeg — required for auto-format and watermark removal (the app skips those features gracefully if it's missing)
Google Chrome + a matching chromedriver — required only for TikTok posting (browser-driven)
Python packages (the app tries to auto-install these on first launch):
yt-dlp, textual, requests, google-api-python-client, google-auth-oauthlib, google-auth-httplib2, instagrapi, Pillow, moviepy, keyring, pyotp, and (lazily, on first use) tiktok-uploader, atproto.


Installing ffmpeg


macOS: brew install ffmpeg
Debian/Ubuntu: sudo apt install ffmpeg
Windows: download from ffmpeg.org and add it to your PATH



Install & run

bashgit clone https://github.com/YOURNAME/ultimate-uploader-V100.git
cd ultimate-uploader-V100
python3 ultimate_uploader.py

On first launch it will attempt to install its Python dependencies automatically. If that fails, install them manually with pip and re-run.


ConfigurationAccounts (set up inside the app)


YouTube — authenticate via the OAuth flow in the YT Auto Post tab.
Instagram — add accounts in the IG tabs (credentials stored in your OS keychain).
TikTok — add an account in the TikTok tab by pasting your sessionid cookie, or drop a Netscape cookies.txt in the tiktok/ folder of your workspace.
Bluesky — add a handle + an app password (create one at Settings → Privacy and Security → App Passwords on bsky.app) in the Bluesky tab.


All account credentials live in your OS keychain or a local workspace folder — never in this repository.


How it works

The workflow for a posted clip is:

source video  →  remove watermark  →  auto-format per platform  →  publish (+ dedup)


Auto-Pull and Mirror feed new videos into your workspace's videos/ folder.
Cross-Post and Schedule publish them to the platforms you choose.
A cached, formatted copy is reused across platforms that share an aspect ratio, so each clip is only re-encoded once.



Important notes & limitations


Unofficial platform access: Instagram (via instagrapi) and TikTok (via a browser-driven cookie session) do not use official public posting APIs. They can break when those platforms change, and automating them may be against those platforms' Terms of Service — use responsibly and at your own risk. YouTube uses the official Data API; Bluesky uses the official AT Protocol.
TikTok cookies expire — if uploads start failing with an auth error, re-export the cookie file / re-add the session.
Bluesky as a source for Mirror mode is limited (yt-dlp support for pulling from a bsky profile is weak); it works best as a publishing destination.
First live runs: the transforms, scheduler, dedup, and UI are well-tested, but the actual posting calls depend on your live accounts — expect to shake out platform-specific quirks on your first real posts.



Security


If this repository is public, do not commit any real secrets. Keep credentials in environment variables or your OS keychain.
A .gitignore (Python template) is recommended so caches, virtual environments, and any local secrets files are never committed.
