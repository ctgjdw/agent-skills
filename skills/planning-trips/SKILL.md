---
name: planning-trips
description: >-
  Use when the user requests help planning a multi-day trip with high-level
  details like origin, destination(s), and approximate duration (e.g., "plan a
  trip from Singapore to Osaka for 10D9N", "help me plan a 2-week Italy
  vacation", "let's plan a Hokkaido honeymoon"). Triggers on the initial
  trip-planning request before any itinerary structure or plan documents exist
  for the trip. Do NOT use for single-recommendation requests (e.g., "best
  ramen in Kyoto") or for on-the-go edits to an already-planned trip.
---

# Planning Trips

## Overview

Structured collaborative process for turning a vague trip idea ("plan a trip from X to Y for ND") into a complete, durable set of trip-planning documents. The skill enforces a single upfront questionnaire to gather all planning inputs, then builds out a standard project structure that subsequent sessions can resume from.

**Core principle:** Ask once, capture everything, persist to disk. Future sessions read the files instead of re-asking.

## When to Use

- User's first message about a trip mentions origin + destination + duration
- User says "plan", "help me plan", "let's plan", "structure", or "organize" + travel context
- No existing trip plan files (CLAUDE.md, `00_overview.md`, etc.) for this trip in the current directory

## When NOT to Use

- Single-recommendation requests ("best ramen", "where to stay in Tokyo") → just answer
- Existing trip with plan files already in the current directory → read & extend, don't re-trigger
- Mid-trip adjustments / on-the-go re-routing → modify itinerary directly

## Process (4 Phases)

### Phase 1 — Confirm Scope (1 message)

Acknowledge the request and confirm the project structure in ONE message before asking anything else:

1. Propose the standard output: `CLAUDE.md` + `00_overview.md` → `05_bucketlist.md` in the current working directory.
2. Confirm any high-level inputs already provided (dates, destination, duration, origin).
3. Tell the user the next message will be a single questionnaire covering everything needed to start.

Do not start research, do not write files yet.

### Phase 2 — Single Upfront Questionnaire

Use `AskUserQuestion` to send the questionnaire as 1–4 grouped questions at a time (the tool's max is 4 per call; iterate calls if needed). Ask all questions before writing any file. Cover every section below; skip a section only if the user already supplied that info.

**Sections to cover (mandatory):**

1. **Travel party** — count, ages/relationship if relevant, destination experience level per person (first-timer vs returnee), any mobility / dietary / health constraints.
2. **Dates** — exact dates if known;.
3. **Destinations & night allocation** — which cities/regions inside the country/region; nights per base; appetite for day trips.
4. **Trip goal / vibe** — first-timer-friendly vs deep-dive; relaxed vs packed; "unforgettable" priorities (e.g., first major international trip for partner, milestone celebration).
5. **Budget tier** — daily spend per person excl. accommodation (low / mid / high) in local or home currency; accommodation budget tier; splurge tolerance and which categories deserve splurges.
6. **Interests** (multi-select) — food & dining, culture & history, shopping, nature & outdoors, theme parks / pop culture, photography, hot springs / spa / wellness, nightlife, sports, festivals, religious sites, architecture.
7. **Dietary restrictions** — allergies, religious, things to avoid (e.g., offal, raw fish, pork, alcohol).
8. **Accommodation preferences** — preferred chains, chains to avoid, must-haves (pool, spa, breakfast, twin beds, view, gym), style (business hotel / boutique / resort / apartment / hostel / traditional inn).
9. **Itinerary style** — hour-by-hour detailed vs flexible day themes; transport-time inclusion; reservation-flagging.
10. **Tourist passes** — open to using regional rail passes, city tourist cards, multi-attraction sightseeing bundles?
11. **Confirmed bookings** — hotels, activities, tours, transport already booked (paste details or retrieve via gmail/google drive mcp).
12. **Documentation preferences** — Wanderlog / Notion / Google Docs/Sheet sync? Photo-research style? Any external tool the user wants integrated.

**How to batch:** group 3–4 related questions per `AskUserQuestion` call. Aim for 3–4 total calls. Don't fan out into a long Q&A — the user chose "one big upfront questionnaire" style for a reason.

### Phase 3 — Initialize Project Files

After all answers are collected, write the seven files in this order using the templates in `file_templates.md`:

1. `CLAUDE.md` — durable project preferences + confirmed bookings + tooling guidance (this is the "memory" future sessions read)
2. `00_overview.md` — trip summary, at-a-glance table, flights, day-by-day theme table, budget snapshot, packing/etiquette/money
3. `01_itinerary.md` — hour-by-hour skeleton per day (fill themes from questionnaire; mark concrete details as TBD)
4. `02_accommodation.md` — criteria + (if not yet booked) shortlist scaffolding
5. `03_reservations.md` — priority timeline + tracker table
6. `04_transport.md` — decision matrix per leg + day-by-day cost estimates
7. `05_bucketlist.md` — categorized place lists per location (food, shopping, sights, etc. — categories driven by user's stated interests, NOT food-only)

After writing, surface a short summary of what was created and what's next (Phase 4).

### Phase 4 — Iterate with Research

For every subsequent message, deepen the plan using available tools. Always verify before writing.

**Research order of preference:**

1. Official sources (hotel/restaurant/attraction sites) for hours, prices, dates
2. `mcp__google-maps__*` to verify place existence, address, coordinates
3. `mcp__claude_ai_Booking_com__accommodations_search` for hotel shortlisting
4. `WebSearch` for everything else
5. `mcp__opentab__reddit_*` for crowd-sourced tips (cross-reference with official sources)
6. `mcp__wanderlog-mcp__*` to mirror the itinerary in Wanderlog if user opted in

If an MCP isn't available, fall back gracefully — use `WebSearch` + manual verification.

## File Templates

See `file_templates.md` in this skill directory for the structural skeleton of each output file.

## Critical Rules — Accuracy & Grounding

**The trip plan must be 100% factual or 100% labeled as unverified. There is no middle ground. Operational facts (hours, prices, dates, addresses, schedules, reservation policies) drift constantly — training data is *never* a valid source for them.**

### 1. No fabrication, ever

Never write a specific hour, price, address, phone number, fare, distance, opening date, closure date, event schedule, or reservation policy unless it came from a verified live source in this session. If you can't verify it now, either omit it or label it `*(unverified)*`. Do not present training-data guesses as facts.

### 2. Verification hierarchy (in this order)

1. **Official source** — operator / hotel / restaurant / attraction / transit operator / government tourism site, accessed via `WebSearch` or `WebFetch`.
2. **Google Maps MCP** (`mcp__google-maps__*`) — for existence, address, coordinates, current operating status, current hours.
3. **Booking.com MCP** (`mcp__claude_ai_Booking_com__*`) — for hotel pricing/ratings *at the snapshot date* (always record the snapshot date next to the price).
4. **Reddit / social** (`mcp__opentab__reddit_*`) — only as a *lead*, never as a standalone fact. Must be cross-referenced with #1 or #2 before being added without a label.
5. **Training data** — never accepted for operational facts. May only be used to *suggest leads to verify*.

### 3. Mandatory uncertainty labels

Every external fact in every file falls into one of these states:

| State | Label | Meaning |
|-------|-------|---------|
| Verified, current | (no label) + source link | From an authoritative source consulted in-session |
| Verified, but >3 months old | `*(verify near date)*` | Likely still correct but re-confirm closer to trip |
| Single-source / partial | `*(verify)*` | Found in one place, not corroborated |
| Social-media only | `*(Reddit, unverified)*` or `*(social, unverified)*` | Lead only, treat as candidate |
| Source unknown / from memory | `*(unverified)*` | Not allowed for operational facts — must be verified or removed |
| Booking decision pending | `❌ TBD` | Awaiting user decision/action |
| Known issue / closed | `⚠️` + explanation | Place closed / under renovation / holiday closure |

### 4. Surface uncertainty in chat, not just in files

When responding to the user, explicitly state which items are unverified and what verification step is needed. Examples:

- ✅ "I've added these 3 restaurants — 2 are official-site-verified, the third (Maguroya Kurogin) is currently `*(verify)*` because only Google Maps confirmed existence; I haven't found official hours yet."
- ✅ "I don't have current hours for X — I'll verify via WebSearch before adding to the itinerary, or you can confirm me skipping it."
- ❌ "Maguroya Kurogin opens 8:00–17:00" (no source, no label)

### 5. Cite the source

Every external fact in a file gets a hyperlink to its primary source in the same cell, or a `Source: <url>` line nearby. If multiple facts share one source, link once in the section header.

### 6. Re-verify time-sensitive facts

Opening hours, event dates, illumination dates, seasonal schedules, ticket prices, and transit timetables drift. When adding them, record the verification date. Re-verify `T-7 days` before the trip.

### 7. Convert relative dates to absolute

`DD-MM-YYYY` everywhere. Never "next Tuesday", "this weekend", "in a few weeks" inside the files.

### 8. Update CLAUDE.md immediately

The moment a preference or booking changes, update `CLAUDE.md` in the same turn. That file is the truth source for the next session.

### 9. Speculation ≠ recommendation

"Try X" without verification is not a recommendation — it's a fabricated suggestion presented as advice. Either verify and recommend, or present explicitly as "candidate to verify". The user must always be able to tell which is which.

### 10. Tell the user when you're uncertain

If asked "is X open Tuesday at 9am?" and you don't have a verified source, say so plainly: "I don't know — I haven't verified hours from a primary source." Do not guess from training data and present it as fact. Plausible-sounding guesses are the most dangerous failure mode.

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Starting research before the questionnaire completes | Phase 2 must finish first. No exceptions. |
| Asking questions one-at-a-time in chat | Use `AskUserQuestion`. Batch 3–4 per call. |
| Making `05_bucketlist.md` food-only | Categorize by user's interests — shopping, photography, nature, etc. |
| Skipping `CLAUDE.md` | It's mandatory. Future sessions need it. |
| Writing a price/time/date without an in-session source | Verify now, label `*(unverified)*`, or omit. Never fabricate. |
| Citing "I recall" or "I believe" as a source | Not a source. Either verify via the hierarchy or label as unverified. |
| Presenting Reddit consensus as fact | Cross-reference with official source, or label `*(Reddit, unverified)*`. |
| Burying uncertainty inside the file only | Also tell the user in chat which items are unverified. |
| Forgetting to record sources | Every external fact gets a link in the doc. |
| Treating MCP availability as required | Always check; fall back to `WebSearch` if missing. |
| Inventing restaurant recommendations | Use the user's bucketlist or verified sources only. Flag unverified suggestions. |

## Red Flags — STOP

- About to write a specific time (`09:00–17:00`), price (`¥1,500`), date (`Nov 5–25`), or address without a source link → **stop, verify or label unverified**
- About to claim a place is "open", "closed", "good", "the best" without a verified source → **stop, verify**
- Saying "I recall" / "I believe" / "I think" about an operational fact → **stop, that's not a source**
- About to recommend a restaurant from training data → **stop, verify on Google Maps + a review platform first**
- About to add an event date (festival, illumination, fireworks) without consulting the official site → **stop, verify**
- Reddit said something useful and I'm about to add it unlabeled → **stop, either cross-reference or add `*(Reddit, unverified)*`**
- Writing `00_overview.md` before all questionnaire answers are in → **stop, finish Phase 2**
- `05_bucketlist.md` has only one category → **re-read the user's stated interests, expand categories**
- User asked "is this open?" and I'm about to guess → **stop, say "I don't know, let me verify"**
