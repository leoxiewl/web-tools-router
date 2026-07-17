# Routing Guide

## Principle

Choose by task semantics:

- Search discovers candidate URLs.
- Fetch reads a known static URL.
- A site adapter performs a known structured operation.
- Browser automation operates rendered or authenticated interfaces.

Do not treat these as interchangeable commands.

## Decision procedure

### No URL

1. Use the host's native web search.
2. Judge success by relevance and source quality, not merely by receiving results.
3. If native search is unavailable or produces no useful result, check for an OpenCLI search adapter.
4. If no healthy adapter is available, use agent-browser to operate a search engine.

Do not call `web_fetch` without first obtaining a URL.

### Known URL with read-only content

1. Use the host's native fetch/read tool for articles, documentation, feeds, public files, and APIs.
2. Treat a response as failed when it is empty, an access-denied page, a CAPTCHA, a JavaScript shell, or unrelated boilerplate.
3. Check whether OpenCLI has an adapter for the site and the exact read operation.
4. Use agent-browser when the content requires rendering, scrolling, login, or interaction.

### Known site with a structured operation

Use OpenCLI when discovery shows an exact command matching the requested outcome. Examples include listing items, searching within a supported site, reading a record, or downloading through a documented adapter.

Do not select OpenCLI merely because it supports the site's name. Confirm the required command, arguments, access mode, and output format.

### Interactive or authenticated page

Use agent-browser directly for:

- login or an existing authenticated session;
- JavaScript-rendered interfaces;
- form filling, buttons, menus, pagination, uploads, and downloads;
- screenshots, visual verification, console inspection, or network inspection;
- unfamiliar sites without a deterministic adapter.

## Failure handling

Classify the failure before changing routes:

| Failure | Response |
|---|---|
| Invalid input or ambiguous target | Clarify or correct the input |
| Transient timeout or rate limit | Retry once only after a meaningful change |
| Empty, blocked, or JS-only fetch | Use a site adapter or browser |
| Adapter unavailable or unsupported | Use browser |
| Authentication expired | Ask the user to restore login |
| CAPTCHA or security challenge | Stop for manual intervention |
| Permission denied | Stop and explain the required permission |

Never loop between two tools without new evidence. Preserve sources and partial output when escalating.

## Verification

Verify according to outcome:

- Search: relevant results with usable URLs.
- Fetch: requested content, title, or fields are present.
- Structured adapter: expected schema and non-empty records.
- Navigation: final URL and page identity match.
- Form preparation: fields contain intended values.
- Submission: success state, receipt, message ID, or equivalent evidence.
- Download: artifact exists and has the expected type and size.
