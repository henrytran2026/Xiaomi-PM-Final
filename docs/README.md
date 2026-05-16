<a id="readme-top"></a>

<br />
<div align="center">
  <h1>🚗 Commute Co-Pilot</h1>
  <p><strong>Your car knows when you should leave.</strong></p>
  <p>An intelligent departure assistant for the Xiaomi SU7 that connects your calendar, real-time traffic, and smart home into one seamless morning flow.</p>

  <a href="https://henrytran2026.github.io/Xiaomi-PM-Final/">View Live Demo</a>
  &middot;
  <a href="docs/README.md">Prototype Docs</a>
  &middot;
  <a href="#eval-results">Eval Results</a>
</div>

<br />

<div align="center">

![GitHub Pages](https://img.shields.io/badge/demo-live-brightgreen?style=flat-square&logo=github)
![Tests](https://img.shields.io/badge/tests-173%20passed-brightgreen?style=flat-square)
![Pass Rate](https://img.shields.io/badge/pass%20rate-100%25-brightgreen?style=flat-square)
![HTML](https://img.shields.io/badge/built%20with-HTML%20%2F%20CSS%20%2F%20JS-orange?style=flat-square)
![Mapbox](https://img.shields.io/badge/traffic%20data-Mapbox-blue?style=flat-square&logo=mapbox)

</div>

---

## About

Commute Co-Pilot is a proposed Xiaomi SU7 feature that learns your daily routine, syncs with your calendar, checks real-time traffic via Mapbox, and sends a smart departure nudge to your phone or watch. If you have Xiaomi smart home devices, it also triggers departure routines (lock door, lights off, AC to away mode) automatically.

This repo contains the interactive prototype and supporting materials for MGMT 275 (Product Management) and MGMT 276 (Product Strategy) at UCLA Anderson, FEMBA 2026.

**This is a concept prototype, not a production application.** It uses rule-based nudge generation and simulated smart home states. Read below for full prototype documentation, including the scripted walkthrough beats, interactive demo instructions, and tech notes.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Getting Started

Open the [live demo](https://henrytran2026.github.io/Xiaomi-PM-Final/) and follow these steps:

### Step 1: Watch the scripted walkthrough
The top of the page shows three synchronized device frames (phone, SU7, Mi Home). Tap the **play button** to watch a morning departure unfold across six beats — from calendar sync to the car pulling away with the house locked behind you. You can also click any dot on the timeline to jump to a specific beat.

### Step 2: Try it yourself
Scroll to the interactive section and build your own nudge:

1. **Type an event name** — try something normal first, like "Team Standup"
2. **Pick a time and search for a real destination** — the system pulls live traffic data from Mapbox
3. **Tap "Co-Pilot, plan my drive"** — a nudge appears on the phone frame with your departure time, traffic level, and drive duration
4. **Tap "Start navigation"** — watch the Mi Home dashboard animate device-by-device (lock → lights → AC → cameras)
5. **Or tap "Not today"** — the nudge dismisses, simulating the learning signal

### Step 3: Test the safety features
Use the quick presets below the input form to see how the prototype handles edge cases:

| Preset | What to look for |
|--------|-----------------|
| ⚠️ Divorce lawyer | Title redacted to "Appointment" (sensitivity filter) |
| 💉 HIV test | Medical privacy — same redaction |
| 🔴 XSS Attack | `<script>` tag rendered as harmless text, not executed |
| 📏 200-char title | Truncated to 60 characters with ellipsis |

Toggle the **🔒 Sensitivity filter** checkbox to see redacted titles displayed verbatim when the filter is off.

For the full walkthrough beat-by-beat breakdown and tech notes, see below.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## What to Look For

These are the five interactions that best demonstrate the product thinking behind the prototype.

**1. The ecosystem loop — one tap, three systems.** Tap "Start navigation" after generating a nudge. Watch what happens: the route loads on the SU7 center screen, the Mi Home dashboard animates device-by-device (lock → lights → AC → cameras), and the phone confirms the handoff. That single tap replaces checking traffic, opening a maps app, locking the door, turning off lights, and adjusting the AC. This is the Xiaomi "Human × Car × Home" moat made tangible — and it's why no competitor with just a car or just a phone can replicate this experience.

**2. The "Not today" button as a learning signal.** Tap "Not today" instead of "Start navigation." The nudge dismisses cleanly with a confirmation. This isn't just a close button — it's a feedback mechanism. In production, every dismissal teaches the system about your schedule (WFH days, irregular weeks, days you don't drive). Our survey found 7/8 users said they'd tap "Not today" without annoyance if a nudge fired on the wrong day — validating this as the right design over a settings screen or manual schedule input.

**3. The sensitivity filter — privacy by design.** Select the "⚠️ Divorce lawyer" or "💉 HIV test" preset. The nudge displays "Appointment" instead of the actual calendar title. Now toggle the 🔒 sensitivity filter off and regenerate — the real title appears. This demonstrates thinking beyond the functional job (get me there on time) to the emotional and social jobs (don't expose my private life on a lock screen that my passenger or coworker might see). The filter covers 43 keywords across medical, legal, recovery, and financial categories.

**4. The XSS protection — security-tested, not just built.** Select the "🔴 XSS Attack" preset. The `<script>alert("XSS")</script>` tag renders as harmless text instead of executing. The original prototype had a real XSS vulnerability (event names inserted via `innerHTML`). We found it by running a 173-test eval against our own code, confirmed it in the source (line 1790), and fixed it by switching to `textContent` rendering. Most course prototypes are never security-tested. This one was tested with 10 distinct XSS attack vectors.

**5. Input validation — what happens when things go wrong.** Try submitting an empty event name (blocked with an error), a 200-character title (truncated to 60 with ellipsis), or a drive time under 5 minutes (suppressed: "too short to nudge"). These aren't flashy, but they show that the prototype handles bad inputs gracefully instead of breaking — which is what separates a product that ships from one that demos.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Key Features

**Sanitized Nudge Generation** — Three-step pipeline: `sensitizeTitle()` → `truncate(60)` → `escapeHtml()`. Renders via `textContent` (not `innerHTML`) to eliminate XSS. Blocks empty titles, suppresses drives under 5 minutes, fixes negative departure time wrapping, and collapses whitespace.

**Animated Smart Home Departure Routine** — Tap "Start navigation" and the Mi Home dashboard animates a staggered device cascade (500ms per device): lock → lights → AC → cameras. Users toggle which devices are included.

**Sensitive Content Filter** — 43 keywords across medical, legal, recovery, and financial categories. Sensitive calendar titles are redacted to "Appointment" in the nudge display. Benign titles pass through unmodified (verified with 10 false-positive checks).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Eval Results

The prototype was evaluated against the golden dataset defined in Internal FAQ 12 of the PR-FAQ. Nudge generation logic was extracted from source code and tested programmatically.

| Category | Tests | Pass | Rate |
|----------|-------|------|------|
| Standard Commute | 62 | 62 | 100% |
| Edge Cases | 40 | 40 | 100% |
| Adversarial / Safety | 71 | 71 | 100% |
| Tone / Personalization | — | — | N/A (requires LLM) |
| **Total** | **173** | **173** | **100%** |

**Coverage:** 87% of the 200-prompt golden dataset. The remaining 13% is Tone/Personalization, which requires an LLM-based nudge generator deferred to production (Internal FAQ 6).

<details>
<summary><strong>What was tested</strong></summary>

**Standard (62 tests):** Every hour 5AM–11PM, all 15-min increments, midnight/noon/11:59PM boundaries, traffic thresholds at every boundary (25/26/45/46 min), 10 departure time calculations, 10 character types

**Edge (40 tests):** 7 empty/whitespace variants, 11 length boundaries (1–10,000 chars), 10 drive suppression boundaries, 6 negative time wraps (1h–24h), 5 whitespace normalizations, unicode

**Adversarial (71 tests):** 10 XSS vectors, 4 prompt injections, 3 code injections, all 43 sensitive keywords individually verified, 10 false positive checks on benign titles

</details>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Architecture

```
User Input (event + time + destination)
         │
         ▼
┌─────────────────────────────────────┐
│      Sanitization Pipeline          │
│  sensitize → truncate → escapeHtml  │
└────────────────┬────────────────────┘
         │
    ┌────┴────┐
    ▼         ▼
 Validate   Mapbox API
 (empty?    (distance,
  <5 min?)   drive time)
    │         │
    └────┬────┘
         ▼
┌─────────────────────────────────────┐
│        Nudge Generation             │
│  departure = event − drive − 15min  │
│  traffic: >45 heavy, >25 mod, ≤25  │
│  render via textContent (XSS-safe)  │
└────────────────┬────────────────────┘
         │
    ┌────┴────┐
    ▼         ▼
 Start Nav  Not Today
    │        (dismiss +
    ▼         learn)
┌─────────────────────────────────────┐
│   Smart Home Departure Routine      │
│   500ms stagger per device          │
│   Lock → Lights → AC → Cameras     │
└─────────────────────────────────────┘
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Project Deliverables

This prototype is part of a larger product strategy analysis for the Xiaomi SU7.

| Deliverable | Description |
|-------------|-------------|
| Product Strategy Analysis | Competitive landscape, moat thesis, winning strategy (MGMT 276) |
| PR-FAQ | Amazon-style press release and FAQ for Commute Co-Pilot (MGMT 275) |
| Product Brief | `product_brief.md` — strategic context for an AI agent partner |
| Prototype | This repo — interactive demo of the departure flow |
| User Research | 8-respondent survey mapped to PR-FAQ claims |
| Eval Report | 173-test golden dataset evaluation |
| PM Framework | MD file defining how AI agents approach product work |

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## File Structure

```
Xiaomi-PM-Final/
├── docs/
│   ├── index.html        # The prototype (~2000 lines, single file)
│   └── README.md         # Prototype-specific docs (walkthrough, demo, tech notes)
├── images/
│   ├── logo.png
│   └── screenshot.png
├── LICENSE.txt
└── README.md             # This file (project overview, eval, architecture)
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Known Limitations

- **Rule-based, not LLM-powered.** Nudge phrasing uses templates. Production would use a fine-tuned LLM (Internal FAQ 6).
- **Single-event input.** Multi-event support scoped for v2.
- **No calendar sync.** Events entered manually.
- **Simulated smart home.** No actual Xiaomi Mi Home API calls.
- **Keyword substring matching.** Sensitivity filter uses `indexOf`. Production would use word-boundary regex or NLP.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## License

Distributed under the Unlicense License. See `LICENSE.txt` for more information.

## Contact

Henry Tran — UCLA Anderson, FEMBA 2026
Chris Zikry — UCLA Anderson, FEMBA 2027

Project Link: [github.com/henrytran2026/Xiaomi-PM-Final](https://github.com/henrytran2026/Xiaomi-PM-Final)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
