# Facebook Comment Exporter

A Tampermonkey userscript that scrapes all Facebook comments — including nested replies, emoji, stickers, and @mentions — and exports to **Excel** or **JSON**.

---

## Installation

**Step 1:** Install [Tampermonkey](https://tampermonkey.net/) for Chrome/Edge/Firefox.

**Step 2:** Click the button below to install the script:

[![Install](https://img.shields.io/badge/Install-Facebook%20Comment%20Exporter-1877f2?style=for-the-badge)](https://github.com/Amorydev/Comments-Exporter-FB/raw/refs/heads/main/facebook-comment-scraper.user.js)

**Step 3:** Tampermonkey will prompt for confirmation → click **Install**.

---

## Usage

1. Open any Facebook post with comments
2. A **floating panel** appears in the top-right corner — drag it anywhere on screen
3. Configure options:
   - **Max Comments**: limit the number of comments (0 = unlimited)
   - **Export Format**: Excel (.xlsx) or JSON
   - **Fields**: select which data fields to include in the export
4. Click **Start Scraping**
5. The script automatically:
   - Scrolls and loads all comments
   - Expands all replies (including deeply nested ones)
   - Highlights each scraped comment with a colored border
   - Shows a live elapsed time counter
6. The export file downloads automatically when done

---

## Exported Data

| Field | Description | Default |
|-------|-------------|---------|
| `author` | Commenter's display name | ✅ |
| `text` | Comment content (includes emoji and sticker labels) | ✅ (required) |
| `timestamp` | Time the comment was posted | ✅ |
| `likes` | Number of reactions | ✅ |
| `depth` | Nesting level (0=main, 1=reply, 2=reply to reply...) | ✅ |
| `replyToAuthor` | Name of the person being replied to | ✅ |
| `profileUrl` | Link to commenter's Facebook profile | ✅ |
| `tagName` | Names @mentioned in the comment | ✅ |
| `isReply` | Whether the comment is a reply | ☐ |
| `parentId` | ID of the parent comment | ☐ |
| `profileImage` | Profile picture URL | ☐ |
| `isMedia` | Whether the comment is a sticker/GIF/image | ☐ |
| `stickerUrl` | URL of the sticker or GIF | ☐ |
| `hasUnloadedReplies` | Whether replies are still not loaded | ☐ |

---

## Features

- **Multi-language** — detects comments in Vietnamese, English, French, Spanish, German, Korean, Japanese
- **MutationObserver** — captures every article the moment it enters the DOM, even if Facebook removes it from the viewport (virtual scroll)
- **Emoji & Sticker support** — scrapes emoji-only comments (`😂😂`) and sticker-only comments
- **Short comments** — captures 1–5 character comments like "Ok", "+" that were previously missed
- **Auto-retry** — if extraction fails on the first pass, the article is retried on the next scroll pass
- **Progressive scraping** — scrolls step-by-step and collects comments continuously, not just at the end
- **Excel export** — includes a Thread column with visual hierarchy (`┌ Main`, `└─ Reply`)
- **Draggable panel** — move the UI to any position on screen
- **Live timer** — displays elapsed scraping time in mm:ss
- **Color-coded borders** — scraped comments are outlined by depth: red=main, orange=reply 1, yellow=reply 2, green=reply 3+

---

## Debugging

Enable verbose logging by running this in the browser console (F12):

```javascript
window.DEPTH_DEBUG = true
```

Shows detailed logs for depth detection, author extraction, and DOM structure analysis.

---

## Version History

### v1.3 (Current)
- ✅ Fixed emoji-only comments (`😂😂😂`) — Facebook renders emoji as `<img alt>`, causing `textContent` to be empty
- ✅ Fixed randomly missed comments — article is only marked as processed after a successful extraction, allowing retries
- ✅ Added `tagName` field (@mentioned users) and `stickerUrl` field
- ✅ Draggable floating panel
- ✅ Live elapsed time counter (mm:ss)

### v1.2
- ✅ MutationObserver — captures articles as they enter the DOM, fixes virtual scroll unloading
- ✅ 3–5x faster — reduced all sleep() timings, `instant` scroll instead of `smooth`
- ✅ Excel (.xlsx) export via SheetJS
- ✅ Field selector and format picker in the UI panel
- ✅ Deduplication by DOM element reference (replaces unstable outerHTML approach)
- ✅ Fixed short comments (`Ok`, `+`) being skipped due to `> 5` character threshold
- ✅ Added Vietnamese keywords: "Trả lời", "Bình luận", "Phản hồi"

### v1.1
- ✅ Auto-download JSON on completion — no confirmation dialog
- ✅ Progressive scraping — scroll and collect simultaneously

### v1.0
- 🎉 Initial release
- ✅ Multi-level nested reply support
- ✅ Color-coded comment depth visualization
- ✅ 5 depth detection strategies

---

## License

MIT — open source, free to use and contribute.
