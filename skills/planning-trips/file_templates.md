# File Templates

Skeleton structures for the seven output files produced by the `planning-trips` skill. Adapt headings to the actual trip; keep section ordering consistent across trips so future sessions know where to look.

**Formatting rules that apply to every file:**

- All files use Markdown.
- Dates are absolute (`YYYY-MM-DD`). Never use "next Tuesday" / "this weekend" / relative forms.
- Currency in destination's local currency with home-currency conversion in parentheses at totals (record the FX snapshot date).
- Every external operational fact (hours, prices, dates, addresses, phone numbers, fares) carries either a source link or one of these labels: `*(verify)*`, `*(unverified)*`, `*(Reddit, unverified)*`, `*(social, unverified)*`, `❌ TBD`, `⚠️`. See `SKILL.md` "Critical Rules" for the full label spec.
- Snapshot date next to any price (`Booking.com 2026-05-20`, `verified 2026-05-21`, etc.) so future sessions know its freshness.
- No fabricated places, restaurants, hotels, or events. If it isn't verified in-session, it isn't in the doc unless explicitly labeled unverified.

---

## `CLAUDE.md` — Project Memory (mandatory)

The persistent context future sessions read first. Keep it terse, fact-only, no narrative.

```markdown
# <Trip Name>

## Trip Overview

- **Dates:** <Start> – <End> (<NDayN-1N>)
- **Travellers:** <Name + experience>, <Name + experience>
- **Origin:** <City> (<Airport code>)
- **Goal:** <One-line "why this trip matters">

## Destinations

<City> · <City> · <City>

## Trip Plan

**Night allocation (<N> nights total):**

- <City> × <N> nights (<dates>) — base near <area>
- <City> × <N> nights (<dates>) — base near <area>

**Day-by-day theme:**
| # | Date | Day | Base | Theme |
|---|------|-----|------|-------|
| 1 | <date> | <weekday> | <city> | <theme> |
| 2 | ... | ... | ... | ... |

## Preferences

**Budget:** <tier with daily ¥ excl. accom>. <splurge policy>.

**Accommodation:** <chain prefs / avoid list / must-haves>.

**Interests:** <list from questionnaire>. <photography note>.

**Diet:** <restrictions and avoids>.

**Itinerary style:** <detail level + travel-time inclusion>.

**Tourist passes:** <interested / not interested / specific passes>.

## Confirmed Bookings

| Item | Details | Status |
|------|---------|--------|
| Flights | <airline + flight numbers + dates + times + booking ref + seats> | ✅ Booked / ❌ TBD |
| Accom <city> | <hotel + address + dates + price + cancellation deadline> | ✅ Booked / ❌ TBD |
| Transport passes | <local IC card, airport express, regional rail/city pass, etc.> | ❌ TBD |

## Documentation

**Primary tool:** <Wanderlog/Notion/Markdown only>
- <trip_key if Wanderlog>
- <other tool-specific notes>

## Tools & Research

- WebSearch — latest info before recommendations
- Google Maps MCP (`mcp__google-maps__*`) — verify place existence/hours/coords
- Booking.com MCP (`mcp__claude_ai_Booking_com__*`) — hotel research
- Reddit MCP (`mcp__opentab__reddit_*`) — crowd-sourced tips, cross-reference with official
- Never fabricate place info — verify or flag as unverified
```

---

## `00_overview.md` — Trip Summary

Reader's landing page. One-screen scannable summary + supporting reference sections.

```markdown
# <Trip Name> — Overview

> **Status:** Updated <YYYY-MM-DD>. <Key change summary>. Most operational items still TBD — see [03_reservations.md](03_reservations.md).

## At a Glance

| | |
|---|---|
| **Dates** | <Start> → <End> (<NDayN-1N>) |
| **Travellers** | <Names> |
| **Route** | <Origin> → <hub> → <city> → <city> → <hub> → <Origin> |
| **Style** | <one-line style note> |
| **Nights** | <City> × N → <City> × N |

## Flights (status)

| Leg | Date | Flight | Route | Time | Notes |
|-----|------|--------|-------|------|-------|
| Outbound | <date> | <airline + #> | <orig→dest> | <dep→arr> | <seats, ref> |
| Return | <date> | <airline + #> | <orig→dest> | <dep→arr> | <seats, ref> |

- Booking ref: **<ref>**
- Add-ons: <baggage etc.>
- Check-in window: <T-48h app> etc.

## Day-by-Day Plan

Detailed hour-by-hour schedule in [01_itinerary.md](01_itinerary.md).

| # | Date | Day | Base | Theme |
|---|------|-----|------|-------|
| 1 | <date> | <weekday> | <city> | <theme> |
| ... | ... | ... | ... | ... |

## Budget Snapshot

User-set budget: **~<¥/SGD/day per person, excl. accom>**. <Splurge policy>.

| Category | Estimate (<N> pax, <N> days) | Notes |
|----------|------------------------------|-------|
| Accommodation | <range> | <link to 02> |
| Food & dining | <range> | <avg lunch/dinner> |
| Transport intra-country | <range> | <pass strategy ref> |
| Activities & entries | <range> | <key entries> |
| Theme parks / splurges | <range> | <if applicable> |
| Shopping buffer | flexible | User-defined |
| **Estimated total (excl. flights, shopping)** | **<range>** | ≈ SGD/USD <range> *(rate verify near date)* |

## Connectivity

- **eSIM:** <provider options>. Activate <when>.
- **Wi-Fi:** <coverage notes>.
- **Apps to install before flying:**
  - <list relevant apps: Google Maps offline, Translate, transit, payment wallets, booking apps>

## Money

- **Cash:** <use cases + withdrawal advice>.
- **Cards:** <acceptance notes>.
- **Mobile pay:** <Apple Pay / wallet support>.
- **Tax-free shopping:** <min spend + passport requirement>.

## Packing Essentials

> **Weather (<month>, <region>):** <temp range>. <layering advice>. Verify forecast T-7 days.

**Clothing** — <bulleted list>
**Photography** — <bulleted list>
**Documents** — <bulleted list>
**Tech** — <bulleted list>
**Other** — <bulleted list>

## Local Etiquette Quick Reference

- <Key rule>
- <Key rule>
- <Key rule>

## Useful Phrases

| English | Romaji/Pinyin | Local script |
|---------|---------------|--------------|
| ... | ... | ... |

## Emergency & Reference

- **Police:** <#>
- **Ambulance/Fire:** <#>
- **Embassy:** <#> *(verify before trip)*
- **Tourist hotline:** <#>

## Document Index

| File | Purpose |
|------|---------|
| 00_overview.md | This file |
| 01_itinerary.md | Hour-by-hour schedule |
| 02_accommodation.md | Hotel shortlist + criteria |
| 03_reservations.md | Booking tracker |
| 04_transport.md | Transit decisions + costs |
| 05_bucketlist.md | Place lists by city/category |

## Open Questions / Decisions Needed

- [ ] <decision>
- [ ] <decision>
```

---

## `01_itinerary.md` — Hour-by-Hour

```markdown
# Itinerary — <Trip Name>

> **Format note:** All times local (<UTC±N>). Travel times typical, re-verify via Google Maps / transit app closer to date. <Sunset note if relevant>.
>
> Restaurants below are *suggestions to research* — confirm via local review sites before relying. Do **not** treat as verified until cross-checked.

---

## Day 1 — <Date> · <Theme>

**Base:** <city/area>
**Weather plan:** <layering / contingency>

> <Optional context block: known crowd patterns, holiday warnings, research-sourced tweaks>

| Time | Activity | Notes |
|------|----------|-------|
| 06:55 | **Depart <origin>** <flight#> | <seats> |
| 14:10 | **Arrive <dest>** | <immigration/transit notes> |
| 14:10–15:30 | <Arrival flow> | <links, queue tips, ticket purchase> |
| 15:30 | <First leg of intra-country transit> | <line, duration, cost> |
| ... | ... | ... |

**Key places**
- [<Place>](<Google Maps URL>)
- [<Place>](<Google Maps URL>)

**Photography:** <golden hour spots, restrictions>

**Pacing tip:** <intentional light/heavy day rationale>

---

## Day 2 — <Date> · <Theme>
(repeat structure)

---

## Cross-Day Reservations Checklist

See [03_reservations.md](03_reservations.md) for full tracker. Time-ordered callouts:

| Item | When to book | Notes |
|------|--------------|-------|
| <hotel> | Now / ~N months out | <peak season note> |
| <activity> | ~N days out | <urgency note> |

---

## Backup / Skip Activities

- **<Place>** — <when to swap in, cost, time required>
```

---

## `02_accommodation.md` — Hotel Shortlist

```markdown
# Accommodation Shortlist — <Trip Name>

> **Status:** Updated <date>. Prices snapshot from Booking.com on <date>. **Re-check before booking.** **Tax-included** unless noted.
>
> **Decision approach:** Shortlist 2–3 per location, verify amenities directly on hotel website (breakfast inclusion, late check-in, luggage hold).

---

## Criteria

| | Hard requirements | Strong preferences |
|---|-------------------|--------------------|
| All | <non-smoking, ≥8.0 reviews, free Wi-Fi> | <near transit, luggage hold, EN front desk> |
| <City> (<dates>, <N>N) | <location proximity rule> | <design / amenities> |

**User-stated chain preferences:** <list>. **Avoid:** <list>.

---

## <City> — Nights <N–N> (<dates>) — <area>

### Top picks

#### 1. ⭐ <Hotel> · <star>★
- **Booking.com:** [link](<url>)
- **Price snapshot (<NN>):** <¥> (≈<¥/night>) — **re-quote for <NN>** as needed
- **Rating:** <score> (<N> reviews)
- **Location:** <address> — <walk time to key transit + landmarks>
- **Why pick:** <1–2 sentences>
- **Caveats:** <if any>
- [Maps](<google-maps-url>)

#### 2. <Hotel> · <star>★
(repeat)

#### 3. <Hotel> · <star>★
(repeat)

### ❌ Reject for this stay
- **<Hotel>** — <reason: location mismatch, user avoid list, etc.>

### Honorable mentions
- <Hotel> · <star>★ · <price> · <rating> · [link](<url>) — <one-liner>

---

## Booking Strategy

1. **Now:** lock <which> first based on availability pressure
2. **Compare:** Booking.com vs Agoda vs Hotels.com vs direct hotel site (direct often gets perks)
3. **Cancellation:** prefer free-cancel rates if available
4. **Luggage:** confirm storage policy for arrival/departure days

## Open To-Dos

- [ ] <decision>
- [ ] <verification>
```

---

## `03_reservations.md` — Booking Tracker

```markdown
# Reservations Tracker — <Trip Name>

> **Status:** All items TBD unless ✅. Updated <date>.
>
> **Time horizon today:** <date>. Trip starts in ~<N> months.

---

## Priority Timeline

### Immediate (<month range>)
- [ ] **<Item>** — <dates, urgency reason>
- [ ] **Travel insurance** — purchase before non-refundable activities

### ~60 days before (<month>)
- [ ] **<Activity/ticket>** — <reason to book early>

### ~30 days before (<month>)
- [ ] **<Restaurant reservation>**
- [ ] **<Activity>**

### Within 2 weeks (<month>)
- [ ] **eSIM activation**
- [ ] **Online check-in** (opens T-48h)
- [ ] **Cash withdrawal**
- [ ] **IC card setup** (mobile or pickup plan)
- [ ] **Verify time-sensitive event dates** on official sites

---

## Detailed Tracker

| Item | Date | Vendor / Provider | Booking deadline | Cost est. | Status | Notes |
|------|------|-------------------|------------------|-----------|--------|-------|
| Flight outbound | <date> | <airline> | — | (paid) | ✅ Booked | <ref> |
| Hotel <city> | <dates> | TBD — <shortlist> | <month> | <¥ range> | ❌ TBD | See [02_accommodation.md](02_accommodation.md) |
| <Activity ticket> | <date> | <vendor> | <month> | <cost> | ❌ TBD | |
| <Restaurant> | <date> | <name> | <month> | <cost/pax> | ❌ TBD | |
| Travel insurance | — | TBD | Before any non-refundable | <range> | ❌ TBD | |
| eSIM (×N) | <dates> | TBD | <month> | <range> | ❌ TBD | |

---

## Booking References (fill in as you go)

| Booking | Provider | Confirmation # | Total paid | Cancel deadline |
|---------|----------|----------------|------------|------------------|
| Flight | <airline> | <ref> | (paid) | n/a |
| <Hotel> | | | | |

---

## Quick Links

- [<Government immigration site>](url)
- [<Airline manage booking>](url)
- [<Major attraction tickets>](url)
- [<Restaurant booking>](url)
```

---

## `04_transport.md` — Intercity & Local

```markdown
# Transport — <Trip Name>

> **Status:** Updated <date>. Verify exact fares closer to date — operators adjust pricing yearly.
>
> **Decision summary:** <one paragraph: IC card choice + pass strategy + airport transfer choice>.

---

## Quick Decision Matrix

| Need | Best option | Why |
|------|-------------|-----|
| <Origin airport → first base> | <train/bus + line> | <duration, cost, reservation> |
| Inside <city A> | <local card + mode> | <reason> |
| <City A> → <City B> (move day) | <line + class> | <duration, cost> |
| <City B> ↔ <day trip city> | <line> | <duration, cost> |
| <Last base> → <departure airport> | <line + class> | <luggage comfort note> |

---

## IC Card / Local Payment

<Card name> — default choice. <Operator>'s IC card; works on <coverage>.

### Options to acquire
1. **<Package at airport>** — <details>
2. **Physical card only** — <details>
3. **Mobile (Apple Wallet)** — <details>

### Recommendation
<which option + why>

---

## <Key Limited Express / Premium Train>

| Direction | Time | Cost | Notes |
|-----------|------|------|-------|
| <Airport → city> | <min> | <¥> | <reserved seats etc.> |

[<Operator info link>](<url>)

---

## Day-by-Day Transport Breakdown

### Day 1 — <date> (<theme>)
- <Leg>: <cost> × <pax> = <total>
- **Total ~<¥>**

(repeat per day)

---

## Total Intra-Country Transport Estimate

| Mode | Cost (<N> pax) |
|------|----------------|
| <category> | <¥> |
| ... | ... |
| **Subtotal** | **<¥>** |
| Top-up buffer | <¥> |
| **Total estimate** | **~<¥> / <SGD/USD>** |

---

## Passes Considered But Don't Recommend

### <Pass name>
- <price + coverage>
- **Analysis:** <break-even math>

### Verdict
<which passes to use / skip>

---

## Luggage Transfer / Storage

For <move day>, <luggage strategy options>:
1. **Forward via <courier>** — cost, cut-off
2. **Coin lockers** at <station> — fallback

[<Courier guide link>](<url>)

---

## What to Confirm Before Trip

- [ ] Verify <train> <year> timetable + pricing
- [ ] Test Mobile IC setup on both phones
- [ ] Confirm hotel luggage policy for <move day>
```

---

## `05_bucketlist.md` — Categorized Places

Categories are driven by the user's stated interests. Food is one category; expand to shopping, sights, photography spots, nature, nightlife, etc. as relevant. Group by location, then by category.

```markdown
# <Trip> Bucket List

Compiled from <sources: previous Wanderlog trips, Google Maps saved places, Reddit shortlists>. Verify each via Google Maps before relying.

---

## <City A>

### Food

| Name | Address | Type | Notes |
|------|---------|------|-------|
| <Restaurant> | <street + ward> | <Ramen/Soba/etc.> | <verdict, hours, queue notes> |

### Shopping (if user listed shopping interest)

| Name | Address | Type | Notes |
|------|---------|------|-------|
| <Store> | <street + ward> | <Streetwear/Department/Souvenir> | <what to buy here, hours> |

### Sights & Culture

| Name | Address | Type | Notes |
|------|---------|------|-------|
| <Site> | <street + ward> | <Temple/Museum/Garden> | <hours, entry fee, photo restrictions> |

### Photography Spots (if user listed photography)

| Name | Address | Best time | Notes |
|------|---------|-----------|-------|
| <Spot> | <street + ward> | <golden hour / blue hour / etc.> | <subject, gear note> |

### Nature & Outdoors (if user listed)

| Name | Address | Type | Notes |
|------|---------|------|-------|
| ... | ... | ... | ... |

### Cafes & Desserts

| Name | Address | Type | Notes |
|------|---------|------|-------|
| ... | ... | ... | ... |

---

## <City B>
(repeat structure, only including categories that match user's interests + research finds)

---

## <City C>
(repeat)
```

**Category selection rules:**

- Always include **Food** (every trip).
- Include **Shopping** if user listed shopping interest.
- Include **Sights & Culture** if user listed culture/history.
- Include **Photography Spots** if user listed photography.
- Include **Nature & Outdoors** if user listed nature/outdoors.
- Include **Nightlife / Bars** if user listed nightlife.
- Include **Hot Springs / Wellness** if user listed wellness.
- Include **Cafes & Desserts** as default if food is an interest (sub-category).
- Skip categories the user didn't express interest in — don't bloat the file.

**Place entry quality:**

- Always include: name, address (street + ward/district), type, verified note.
- Use ⚠️ for places known to be closed/changed.
- Use ❓ for places that might be closed (verify before going).
- Use ★ rating only if from a verified source (Google Maps, local review platform like Tabelog/Yelp/TripAdvisor — name the platform).
- Never fabricate addresses or ratings. If unverified, omit or label `*(unverified)*`.
