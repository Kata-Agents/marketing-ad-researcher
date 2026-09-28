# Marketing Ad Researcher

Turns competitor ad-library output and customer reviews into an evidence pack the rest of a campaign can be built on, ranking creatives by how long they have been running and refusing to fill a gap with something plausible.

Longevity is the only free performance signal the ad libraries give you: nobody pays to keep a losing creative alive. So it ranks by days active, deduplicates one advertiser's variant sprawl down to one entry, caps any single advertiser's share of the list, and says plainly that impressions, spend and click-through are not published and will not be estimated.

It is blunt about coverage, because coverage is where this work quietly fails. Outside the EU most libraries show only currently active ads, the archive window differs per platform, and a curated top-ads feed is a biased sample rather than an advertiser's output. Each of those is recorded as a named gap with its reason instead of being averaged into a confident number.

It fetches nothing. It has no browser, no ad-account connector and no video tools: the harvest plan it writes is for you to run, and the library rows, review text and frame descriptions come back to it as pasted text. Anything it was not shown is reported as not found, never as absent.

## What this is, precisely

A FindAgent **`mcp-tool`** agent. Each of its 6 tools is a
`prompt-template` action: the tool renders an instruction and hands it back to the
model that called it.

Two consequences worth being blunt about, because they decide whether this is useful to you:

- **It calls no model and reaches no network.** A tool call costs nothing and returns
  the same text for the same input, every time. There is no API key, no credential
  slot and no egress.
- **It observes nothing.** It has no access to your repository, your logs, your
  analytics or your devices. Every template is written so that supplying nothing
  produces an honest statement of what is missing rather than a confident-looking
  answer about data nobody provided. If you ask for a report and give it no findings,
  it will tell you the work has not been done — not invent it.

## Tools

| Tool | What it returns | Required input |
|---|---|---|
| `plan_competitor_harvest` | Write the exact ad-library harvest plan for someone else to run — per advertiser, per platform, with the two-pass Meta approach and what each platform's window will and will not return. | `frozen_brief` |
| `rank_by_longevity` | Rank harvested creatives by days active — the only free performance proxy the libraries give — with deduplication, a per-advertiser cap, and an explicit refusal to estimate spend or reach. | `harvest_rows` |
| `tear_down_creative` | Break a captured competitor creative into its hook, structure, claim and proof so the parts can be reused, working from supplied frames and transcript rather than from the video. | `creative_evidence` |
| `extract_customer_voice` | Pull verbatim customer language out of supplied reviews and comments into themed pains, desired outcomes, objections and switching triggers — keeping the exact words, because the phrasing is the asset. | `source_text` |
| `assess_coverage` | State honestly what the evidence pack reached and what it did not, with a confidence level derived from the gaps rather than from the volume collected. | `what_was_collected` |
| `draft_evidence_pack` | Assemble the evidence pack for the angle stage — competitor findings, customer voice, whitespace and coverage — refusing to produce findings when no evidence was supplied. | `brief_context` |

Optional inputs render as empty when omitted. Every template names that case and says
what it could not determine, so an empty slot degrades into a stated gap rather than a
dangling clause.

## Part of a department

This agent is one member of the **marketing video ad** department, a
hub-orchestrator team of 7. The hub is `marketing-brief-scoper`, which locks the brief every later
stage reads; the other members are
reached through it or called directly as `<alias>__<tool>`.

| Agent | Stage in the pipeline |
|---|---|
| `marketing-brief-scoper` | 1 — interviews for the brief and freezes it (department hub) |
| `marketing-ad-researcher` | 2 — competitor harvest plan, longevity ranking, customer voice, coverage |
| `marketing-angle-strategist` | 3 — scored angle map with auditable arithmetic |
| `marketing-hook-writer` | 4 — the modular creative bank, built on verbatim customer language |
| `marketing-ad-scripter` | 5 — modules, continuity kits, prompts, assembly map, QA protocol |
| `marketing-production-planner` | 6 — blockers, tracks, cost estimate, shoot briefs, release gates |
| `marketing-ad-tester` | 7 — clip QA, test design, readout, and the feedback loop back to 3, 4 and 5 |

Each member is published independently and works on its own.

## Provenance

`source/researcher.md` is the markdown skill this agent was converted from. The tool templates carry its
instructions, parameterised: anything the original hard-coded to one team's repositories,
file paths or people became an input you supply, and where a template would otherwise
depend on reading something it cannot reach, it asks for that material as an argument
instead.

## Licence and use

Published by Kata Team on FindAgent. Free to connect.
