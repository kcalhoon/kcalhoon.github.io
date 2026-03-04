---
description: Format image references in a post using Jekyll link syntax, add alt text, and add source captions.
argument-hint: [file-path]
allowed-tools: Read, Edit, Glob, Grep, Bash
---

# format-image-references

Fix all image references in a Jekyll blog post to follow the correct format, add missing alt text, and insert source captions.

## Usage

Invoked as: `/format-image-references [file-path]`

If `$ARGUMENTS` is provided, treat it as the path to the post file to process. If no argument is given, ask the user which file to process.

---

## What this skill does

For every image in the target post, ensure:

1. **Jekyll link syntax** — The image src uses `{% link assets/images/... %}` instead of a bare path or absolute URL.
2. **Minimal Mistakes border style with sizing** — The image tag ends with `{: style="border: 1px solid #2E8B57; width: 67%; display: block; margin: 0 auto;"}`.
3. **Alt text** — If the `![alt]` portion is empty or generic, replace it with descriptive alt text derived from the filename and surrounding content.
4. **Source caption** — Immediately after the image line, there must be a `<p>` caption block:
   ```html
   <p style="text-align: center; font-size: 0.9em; color: #555;">
     Source: <Source Name>
   </p>
   ```
   If missing, infer the source from the image filename (see rules below).

---

## Correct format reference

The canonical pattern (from `_posts/2026-01-12-newsletter-january-2026.md`):

```markdown
![The $39B AI Market]({% link assets/images/newsletter-2026-01/menlo_ai_spending.png %}){: style="border: 1px solid #2E8B57; width: 67%; display: block; margin: 0 auto;"}

<p style="text-align: center; font-size: 0.9em; color: #555;">
  Source: Menlo Ventures
</p>
```

---

## Step 1 — Read the file

Read the full content of `$ARGUMENTS` (the post file).

---

## Step 2 — Identify all image references

Find every markdown image reference: lines matching `!\[.*\]\(.*\)`.

For each image:
- Extract the current alt text
- Extract the current src path
- Check whether a `<p style=...>Source:` block immediately follows (within 3 lines)

---

## Step 3 — Fix the image src to use Jekyll link syntax

Convert these patterns to the canonical form:

| Current form | Convert to |
|---|---|
| `![alt](/assets/images/folder/file.png)` | `![alt]({% link assets/images/folder/file.png %})` |
| `![alt](assets/images/folder/file.png)` | `![alt]({% link assets/images/folder/file.png %})` |
| Already `{% link ... %}` | No change needed |

Preserve the existing folder path and filename exactly.

---

## Step 4 — Ensure border style attribute

After the closing `)` of the image, ensure `{: style="border: 1px solid #2E8B57; width: 67%; display: block; margin: 0 auto;"}` is present.

If a different `{:...}` attribute block exists, replace it with the standard border style. This scales images to 67% of container width (a 33% reduction) and centers them.

---

## Step 5 — Generate alt text if missing or empty

If `![](...)` or `![ ](...)`, generate descriptive alt text using these rules:

1. Convert the filename (without extension) to title case: replace `-` and `_` with spaces, then capitalize each word.
   - `menlo_ai_spending.png` → `Menlo AI Spending`
   - `anthropic_econ-impact.png` → `Anthropic Economic Impact`
   - `openai-fastest-sectors.png` → `OpenAI Fastest Growing Sectors`
2. Incorporate context from the surrounding paragraph if helpful.

---

## Step 6 — Infer and insert source caption

If no `<p style="text-align: center; ...">Source:` block follows the image, infer the source from the image **filename prefix** using this mapping:

| Filename contains | Source |
|---|---|
| `anthropic` | Anthropic |
| `openai` or `openai-` | OpenAI |
| `google` | Google |
| `microsoft` | Microsoft |
| `menlo` | Menlo Ventures |
| `mckinsey` | McKinsey |
| `meta` | Meta |
| `aws` or `amazon` | AWS |
| `gartner` | Gartner |
| `forrester` | Forrester |
| `deloitte` | Deloitte |
| `hbs` or `harvard` | Harvard Business School |
| `stanford` | Stanford |
| `mit` | MIT |
| `idc` | IDC |
| `pwc` | PwC |
| `bcg` | BCG |

If no prefix matches, look at the folder name and surrounding article text for context. If still unclear, insert `Source: [Unknown — please update]` as a placeholder.

Insert the caption block immediately after the image line, separated by a blank line:

```markdown
<p style="text-align: center; font-size: 0.9em; color: #555;">
  Source: <Inferred Source>
</p>
```

---

## Step 7 — Apply all changes

Use the Edit tool to apply each fix to the file. Make precise, targeted edits — do not rewrite surrounding content.

---

## Step 8 — Report results

After completing all edits, output a summary table:

| Image file | Alt text changed | Source caption added/updated | Other fixes |
|---|---|---|---|
| filename.png | Yes / No | Yes / No | e.g. fixed link syntax |

If no changes were needed, say so clearly.
