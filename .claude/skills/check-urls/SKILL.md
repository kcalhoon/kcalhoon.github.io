---
description: Check all URLs in a file and report accessible/failed status.
argument-hint: [file-path]
allowed-tools: Read, Bash
---

# check-urls

Check the accessibility of all URLs in a blog post and report their status.

## Usage

Invoked as: `/check-urls [file-path]`

- If `$ARGUMENTS` is provided, use it as the file path.
- Otherwise, read the file content from the conversation context (user dragged it in).

---

## Step 1 — Read the file

If a file path was given, use the Read tool to load it. Otherwise use the content already in the chat.

---

## Step 2 — Extract all URLs

Extract every URL from the file that appears in:
- Markdown links: `[text](https://...)`
- Attribute links: `](https://...){:target=...}`
- Bare `https://` URLs in text

Deduplicate the list. Preserve the order they appear in the file.

---

## Step 3 — Test all URLs with curl

Use a single Bash call to test all URLs at once using curl. This is faster and more reliable than WebFetch.

```bash
urls=(
  "https://url-one"
  "https://url-two"
  ...
)

for url in "${urls[@]}"; do
  status=$(curl -o /dev/null -s -w "%{http_code}" -L --max-time 10 -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "$url")
  echo "$status $url"
done
```

---

## Step 4 — Classify results

Classify each URL by its HTTP status:

| Status | Classification |
|---|---|
| 200, 201 | `OK` — accessible |
| 301, 302, 303, 307, 308 | `OK` — redirects (followed by curl `-L`) |
| 403 | `BOT-BLOCK` — site blocks scrapers; likely works in browser |
| 404, 410 | `FAIL` — page not found or removed |
| 000 | `FAIL` — connection error or timeout |
| 500, 502, 503 | `FAIL` — server error |
| other | `FAIL` — unexpected status |

**Important:** Treat 403s separately from genuine failures. Major publishers (NYT, Forbes, BCG, OpenAI, Substack, Seeking Alpha, McKinsey, etc.) routinely return 403 to automated requests even when the page is live.

---

## Step 5 — Report results

Output a URL validation report in this format:

```
URL VALIDATION REPORT
=====================
OK        (200) — https://example.com/article-one
BOT-BLOCK (403) — https://www.forbes.com/...
FAIL      (404) — https://broken-link.example.com/
FAIL      (000) — https://timed-out.example.com/

SUMMARY: X URLs checked — Y OK, Z bot-blocked (likely fine), W failed
```

Then output two sections at the bottom:

### BOT-BLOCKED URLs
List 403s with a note: "These likely work in a browser — verify manually if needed."

### Failed URLs
List genuine failures (404, 000, 500, etc.) that need attention.
