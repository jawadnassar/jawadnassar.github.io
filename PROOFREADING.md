# Proofreading posts

Task guide for an LLM asked to proofread a post in `_posts/`. This is **not** a coding
guide — for code, design, or build rules see [`AGENTS.md`](AGENTS.md).

Attach this file (or point the model at it) and name the post(s) to review, e.g.
"Proofread `_posts/2025-03-08-htb-nibbles.md` using `PROOFREADING.md`."

## Golden rules

1. **Fix English, not content.** Spelling, grammar, punctuation, and broken Markdown only.
2. **Smallest possible diff.** One corrected word beats a rewritten sentence.
3. **Never touch code, commands, payloads, URLs, numbers, or frontmatter.**
4. **Keep the author's voice** — first person, direct, informal. Do not make it corporate.
5. **If a technical claim looks wrong, report it. Do not "fix" it.**
6. **English only.** Do not translate, and do not switch spelling variants.

## Fix these

- Spelling, typos, doubled words (`the the`), missing/extra words.
- Grammar: subject–verb agreement, tense consistency, articles, prepositions.
- Punctuation: missing periods, comma splices, unbalanced parentheses/quotes.
- Capitalization of ordinary prose (not commands, flags, or identifiers).
- Broken Markdown: unclosed code fences, malformed links/images, skipped heading levels.
- Obvious title typos in `title:` (no stylistic rewrites).

## Never change

- **Code of any kind:** fenced blocks, inline `code`, commands, flags, payloads, SQL,
  output, config, paths, filenames, ports, IPs, hashes, versions, timestamps.
- **URLs and links**, including `{:target="_blank"}` attributes and image URLs.
- **Frontmatter** other than an obvious typo in `title`. Never change `date`,
  `categories`, `tags`, `series`, `layout`, or `permalink`.
- **Technical facts:** numbers, versions, tool names, HTTP status codes, payload syntax.
- **Structure:** don't add or remove sections, intros, conclusions, summaries, TL;DRs,
  disclaimers, or examples.
- **Paragraphs:** don't rewrap, merge, or split them. Posts use long unwrapped lines.
- **Voice:** don't formalize, shorten, or "punch up" prose.
- **Formatting flourishes:** no emoji, no em dashes added, no bold/italics for emphasis,
  no tables converted to lists (or vice versa).
- **Spelling variant:** the site uses US English (`color`, `behavior`, `organization`).
  Don't convert to en-GB.

## English and voice

- Target: clear, plain US English that still sounds like the author.
- Prefer the author's existing wording when it is already correct.
- Don't add Oxford commas or remove them — match the sentence's existing style.
- Keep first person ("I ran", "I found"). Don't convert to "we" or passive voice.
- Keep technical jargon and tool names as-is (`nmap`, `Burp Suite`, `sudo`, `payload`).
- Don't translate quoted error messages or output.

## Categories and tags

Use these exactly, including case. Never invent new ones while proofreading, and never
add, remove, or reorder them without an explicit instruction.

**Categories**

| Category |
|---|
| `Writing` |
| `TIL` |
| `CTF` |
| `PortSwigger` |
| `HTB` |

**Tags**

| Tag |
|---|
| `SQLi` |
| `Web` |
| `Recon` |
| `Linux` |
| `Windows` |
| `Privacy` |
| `Passwords` |
| `Shells` |

- `series` is a free-text grouping key. If present, keep it **character-for-character**
  (`PortSwigger SQLi`), otherwise the series silently splits.
- Adding a brand-new category or tag is a site-wide decision, not a proofreading edit:
  chips, counts, archives, and filter rules all regenerate from the frontmatter.

## Markdown conventions

- `##` for sections, `###` for subsections. The page title is the `h1` from the layout —
  never add an `#` heading inside a post.
- External links use kramdown attributes: `[text](https://example.com){:target="_blank"}`.
  Preserve them exactly.
- Images: `![descriptive alt](https://jawad.ca/images/...)`. Alt text is required and
  should describe what the screenshot shows (5–15 words). Never change the URL.
- Code fences keep their language tag (```` ```shell ````, ```` ```sql ````, etc.) and
  their contents byte-for-byte.
- Blank lines separate paragraphs, lists, and fences. Don't collapse them.

## Workflow

1. Read the whole post before editing.
2. Edit in place, one file at a time, with minimal diffs.
3. Re-read the edited paragraphs to confirm meaning didn't change.
4. Do not commit, push, or touch files other than the ones named.
5. Do not run site-wide refactors, renames, or formatting passes.

## Report back

After editing, return a short summary:

```
Files changed: 1

_posts/2025-03-08-htb-nibbles.md
  - 6 spelling fixes
  - 2 grammar fixes
  - 1 broken link fixed

Flagged, not changed:
  - line 42: "port 8080" looks wrong for Tomcat 7 (probably 8080 is right, but verify)
  - title: "Github" vs "GitHub" — left as-is, decide yourself
```

List anything you were unsure about instead of guessing. If a sentence is ambiguous,
ask before rewriting it.

## Examples

| Do | Don't |
|---|---|
| `recieve` → `receive` | Rewrite a whole paragraph for flow |
| `it's` → `its` (possessive) | Change `' OR 1=1--` payload |
| Close an unclosed code fence | Reformat code or change indentation |
| Fix `##Enumeration` → `## Enumeration` | Add a "## Conclusion" section |
| Fix a typo in `title:` | Rename the post or change its slug |
| Flag "port 8080 seems wrong" | Change `8080` to `80` |

## Out of scope

SEO, keyword density, meta descriptions, internal linking strategy, tone changes,
fact-checking beyond flagging, translating, and generating new content.
