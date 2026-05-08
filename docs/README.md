# Commute Co-Pilot — Interactive Demo

An interactive walkthrough of the morning departure flow for **Commute Co-Pilot**, a proposed Xiaomi SU7 feature that connects calendar, real-time traffic, and Xiaomi smart home devices into a single intelligent flow.

This demo accompanies the PR/FAQ submitted for **MGMT 275 · Product Management in Tech Companies** (UCLA Anderson, FEMBA 2026) by Henry Tran.

## What you're looking at

The page has two sections:

**1. Scripted walkthrough.** Three synchronized device frames — **phone**, **SU7 center screen**, and **Mi Home dashboard** — running through six beats of a real morning:

| Beat | Time | What happens |
|------|------|--------------|
| 1 | 7:45 AM | Calendar sync. Traffic prediction runs in the background. |
| 2 | 8:02 AM | Nudge appears: *"Leave by 8:15 for your 9am standup."* |
| 3 | 8:14 AM | User taps Start navigation. |
| 4 | 8:14 AM | Route handed to the SU7 center screen. |
| 5 | 8:15 AM | Smart home departure routine fires (door, lights, AC, cameras, stove, vacuum). |
| 6 | 8:16 AM | User in the car. House handled. One tap. |

Tap the play button to run the journey, or scrub directly to any beat by clicking the dots on the timeline. The orange "Start navigation" button inside the on-phone nudge also kicks off the handoff sequence.

**2. Try it yourself.** Below the walkthrough, type your own calendar event ("9am standup at Building 4", "1:30pm lunch in Beverly Hills") and watch a Co-Pilot nudge get generated for it on a working phone frame. Toggle heavy traffic and smart home on/off to see the output adapt. The nudge generator uses rule-based logic for the demo — the real product uses a fine-tuned small LLM, as described in Internal FAQ #6 of the PR/FAQ.

## Scope

This demo intentionally covers **only the morning departure flow** — the highest-leverage moment in the feature, and the one that proves "Human × Car × Home" works as a daily habit. Arrival, weekly summary, and settings screens are out of scope for this artifact.

## How to view

**Local:** Open `index.html` in any modern browser. No build step, no dependencies beyond Google Fonts loaded over the network.

**Hosted (GitHub Pages):**
1. Push this folder to a GitHub repo
2. In repo Settings → Pages, set Source to "Deploy from a branch", branch `main`, folder `/docs` (or wherever this is committed)
3. The demo will be live at `https://<username>.github.io/<repo>/`

## Tech notes

Single HTML file. All CSS and JS inline. No frameworks. No build pipeline. Designed to be readable end-to-end by anyone curious how the prototype works — open the file and scroll. Typography is Fraunces (display) and JetBrains Mono (monospace UI text), reflecting the Xiaomi HyperOS design language: confident serif headings, monospaced metadata, restrained accent color (#FF6900) used only at decision moments.

## Source material

Full PR/FAQ is available in the parent repo. The PR/FAQ defines the feature, target user, success metrics, RICE prioritization, competitive landscape, A/B experiment design, and technical architecture. This demo brings Section C — the user journey — to life.
