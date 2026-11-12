# Watchlist

A page of YouTube Shorts I have emailed to myself over the years, rebuilt
automatically from Gmail every morning.

Videos arrive by email with a subject starting with `Watch`; this repo holds the
generated `index.html` and `feed.xml`. Nothing is stored here except the
resulting page — the generator itself lives outside the repo, in
`~/.local/bin/yt-watchlist`.

- `index.html` — the page (search box filters by title/channel, no build step)
- `feed.xml` — Atom feed of the 50 most recent additions
- `.nojekyll` — serve files as-is on GitHub Pages

## How it is produced

`yt-watchlist` logs into Gmail read-only over IMAP (an app password, not OAuth),
searches `subject:Watch`, extracts YouTube video IDs from each message, caches
title/channel/thumbnail via YouTube's public oEmbed endpoint, renders the static
page, then commits and pushes. Messages are fetched with `BODY.PEEK[]`, so
nothing is ever marked as read.

Run manually:

```bash
yt-watchlist              # collect, render, commit, push
yt-watchlist --dry-run    # list what it found, write nothing
yt-watchlist --no-push    # write files but do not commit
```

Scheduled with the `yt-watchlist.timer` systemd user timer (daily, 07:00).
