# Implementation Plan

> EDITING DIRECTIVE: USER AND AGENT EDIT THIS FILE COLLABORATIVELY. THE USER MUST REVIEW AND APPROVE ITS CONTENT.

Purpose of this file: Turn the approved specification into ordered, updatable implementation and verification work.

## Instructions for the user

Preserve the approved requirements and verify the completed work. Direct priorities, scope, and meaningful checkpoints; judge technical choices, risks, and proposed changes; and approve results only after checking them against the specification rather than relying solely on the agent's report.

If the intended result changes, update the specification. If only the route changes, update this plan and record the revision.

## Instructions for the agent

Read AGENTS.md, brief.md, research.md, spec.md, and this file, then inspect the relevant project files. Begin with a concise orientation and one focused question.

Guide planning one stage at a time. Surface dependencies, risks, and verification needs without expanding scope or making decisions for the user. Draft concise, project-specific tasks and keep them current. Never mark approval gates or user-verification items complete on the user's behalf.

## Approach

**Target file:** `calculator/index.html` only — a standalone HTML build (all CSS/JS inline, no build step) of Andy Masley's current CC0 calculator source. The unchanged original is kept as `calculator/ai-prompt-footprint-source.astro` for reference and is not edited. See the 2026-09-27 revision.

**Build order and dependencies:**
1. **Feature 1 — Beyond the prompt** (image/video rows). Independent of the others; adds new row types to the existing per-day table.
2. **Feature 2 — Project totals**. Independent; adds a new, separate section outside the per-day table.
3. **Feature 3 — Everyday digital habits**. Independent; adds a new input section and its own comparison total.
4. **Feature 4 — Device matters**. Depends on #3 — it adds a device dropdown to the streaming/gaming rows #3 creates, so #3 must exist first.
5. **Feature 5 — Uncertainty control**. Built last on purpose: the range toggle needs to touch every displayed number on the page (daily bars, annual bars, the mini comparison chart, the headline verdict, plus the new project-total and digital-habits numbers from #1-4), so it's safest to wire up once everything it needs to cover already exists.

**Non-obvious choices carried over from the spec:**
- Water is excluded from image/video rows (feature 1) and from the digital-habits total (feature 3) unless a source already covers it — no new water figures are invented.
- Project totals (feature 2) and the digital-habits total (feature 3) are kept visually and numerically separate from the existing AI daily/annual total, not summed into it.
- Feature 5's "dense Claude" multiplier (~3.3x-10x) and feature 4's device ratios (streaming: phone 1x/laptop ~20x/TV ~100x; gaming: console ~140/regular PC ~233/gaming PC ~1,400 kWh/yr) are already pinned down in spec.md — no further research needed before building them.

**Risks:**
- Feature 5's range toggle has wide surface area (many places a number is rendered); missing a spot would leave stale point-estimates alongside toggled ranges. Worth a dedicated pass grepping every place `wh`/`ml` values are formatted for display.
- Feature 1's video-model list needs exact per-model Wh figures pulled from the Jegham et al. table already recorded in research.md (LTX-2, HunyuanVideo-1.5, Veo 3, Seedance-1, Gen-4.5, Sora 2.0 Pro) — transcription errors here would misrepresent the source.
- Feature 3's default device assumptions (e.g., "laptop" for streaming, "typical gaming PC" for gaming) need to line up with feature 4's device tiers so switching the feature 4 dropdown to the matching tier doesn't change the number when it shouldn't.

## Checklist

Replace or expand the implementation placeholders below with tasks specific to the approved specification.

### Approval gates

- [ ] User has reviewed, verified, and approved the research claims and selected features
- [x] User has reviewed and approved the specification — user confirmed 2026-09-27
- [x] User has reviewed and approved the implementation approach and task sequence — user confirmed 2026-09-27

### Implementation

**Feature 1 — Beyond the prompt**
- [x] Add "Image generation" and "Video generation" row types to the per-day table (add/remove, count-per-day input)
- [x] Add the model-agnostic image-generation energy figure (Luccioni et al. 2024, ~2.9 Wh/image)
- [x] Add the video-generation model picker with per-model Wh/whmin/whmax figures (Jegham et al. 2026: LTX-2, HunyuanVideo-1.5, Veo 3, Seedance-1, Gen-4.5, Sora 2.0 Pro) — proprietary models use the midpoint of the published range as the typical value (user decision 2026-09-27)
- [x] Wire both row types into the existing daily/annual carbon total and comparison bars; confirm water total is unaffected by them
- [x] Add methodology text and citations for both row types, including the water-exclusion rationale

**Feature 2 — Project totals**
- [x] Add a new "Project totals" section, separate from the per-day table
- [x] Add model picker (reuse existing model list) and a session-count input
- [x] Compute the project total as a range (low = benchmark's low CI, high = ~2x per Bai et al. 2026), shown next to a familiar comparison — high end = 2x EcoLogits' high estimate (user decision 2026-09-27); comparison = largest sourced reference item not above the low end, with "up to N times" / "less than" wording below the smallest item (user decision 2026-09-27). Project also added to the downloadable report as section 11 (user request)
- [x] Add the footnote on run-to-run variance and difficulty not predicting cost
- [x] Confirm the project total is not added into the annual chart

**Feature 3 — Everyday digital habits**
- [x] Remove the existing unsourced "social media" dropdown (state, UI, URL param) from the combined comparison sentence — done 2026-09-21, ahead of the rest of this feature, at the user's direct request after seeing it in the running page. That edit was lost with the old file, but the 2026-09-27 rebased `index.html` never had this dropdown, so it stays satisfied.
- [x] Remove the existing "gaming" and "streaming" dropdowns from the combined comparison sentence and their GAMING/STREAMING placeholder data — done 2026-09-21, same as above (superseded by this feature's sourced versions, still to be added). Also absent from the 2026-09-27 base.
- [x] Add a "Your digital habits" input section: streaming hours/day, video-call hours/day (camera on/off), gaming hours/day
- [x] Compute a separate digital-habits daily/annual total, kept distinct from the AI total — carbon only (no source measures water); each habit's energy is backed out from its source's own grid and costed on the reader's region (user decision 2026-09-27); gaming = measured draw while gaming, ~300 W typical, 150–450 W range (user decision 2026-09-27); video calls use the midpoint of Mortas' range
- [x] Replace the relevant generic reference lines (e.g., "an hour on a PS5") in the existing comparison charts with the person's entered values where they overlap
- [x] Add methodology text and citations (Kamiya/IEA 2020, Mortas 2026, Mills et al.), including confidence-level and social-media-exclusion notes

**Feature 4 — Device matters** (after Feature 3)
- [ ] Add a device dropdown to the streaming row (phone/laptop/TV) using the confirmed ratios (1x/~20x/~100x)
- [ ] Add a device dropdown to the gaming row (console/regular PC/gaming PC) using the confirmed figures (~140/~233/~1,400 kWh/yr)
- [ ] Confirm feature 3's default device assumption matches the corresponding feature 4 tier (no jump when the picker first appears)
- [ ] Add methodology text for both ratios, including the gaming figure's secondary-source and dated-hardware caveats

**Feature 5 — Uncertainty control** (after Features 1-4)
- [ ] Add the range toggle; audit every place a carbon/water number is rendered (daily bars, annual bars, mini chart, headline verdict, project totals, digital-habits totals) and swap each to use `whmin`/`whmax` when toggled on
- [ ] Add the "what if Claude is dense" toggle, active only when a Claude model is in use; apply the ~3.3x-10x multiplier to Claude-attributable figures only
- [ ] Confirm both toggles can be combined (dense + range) and that non-Claude figures are unaffected by the dense toggle
- [ ] Add methodology text explaining both toggles' basis and caveats in plain language

### Verification

- [ ] User has checked feature behavior and calculations against the specification and sources independently of the agent
- [ ] User has confirmed factual and numerical claims have working citations and communicate important limitations or uncertainty
- [ ] User has confirmed the project runs locally, serves all three reference profiles, and matches the specification

### Delivery

- [ ] Commit meaningful checkpoints and export the working chat transcripts
- [ ] Add the provided Project 2 debrief, complete it after verification, and export its transcript

## Revisions

**2026-09-21 — Existing unsourced gaming/social/streaming code discovered in index.html.** Before starting implementation, found that `index.html` already carries over rough, explicitly unsourced "gaming," "social media," and "streaming" dropdowns from Project 1 (3-tier pick, folded additively into the same combined comparison baseline AI use is measured against). This conflicts with the approved spec: feature #3 calls for a separate, sourced comparison, and research.md already decided to exclude social media for lack of a source — which this existing code already violates. User decided: restructure to match the approved spec (pull gaming/streaming out of the combined-comparison sentence into the new "Your digital habits" section with sourced figures; add video calls alongside them) and remove the unsourced social-media dropdown entirely. Added to Feature 3's implementation tasks below.

**2026-09-27 — Calculator rebased on Andy Masley's current source.** The Project 1 `index.html` (with the uncommitted 2026-09-21 dropdown removals) and `footprint-calculator.astro` were deleted from the working tree. `calculator/index.html` is now a standalone HTML conversion of the current CC0 source (andymasley.com/visuals/ai-prompt-footprint-source.txt), with the site's spacing/text styles and a fix so it runs under VS Code Live Server; the original is kept unchanged as `calculator/ai-prompt-footprint-source.astro`. Checked the new base against this plan: it has no social/gaming/streaming dropdowns (so Feature 3's two removal tasks remain satisfied without redoing them), and still has everything later features rely on — per-model `whmin`/`whmax` ranges, the "coding / agent session" size, the "An hour on a PS5" reference line, and the Claude disclaimer. Route change only; the specification is unchanged.

## Commands

### Start planning

User: Open the project repository as your workspace, start a fresh chat, and type `start planning`.

### Start implementation

User: After approving the plan, open the project repository in a fresh chat and type `start implementation`.

Agent: Read AGENTS.md, brief.md, spec.md, and this file, then inspect only the project files relevant to the approved work. Follow AGENTS.md and the approved plan. Do not begin implementation if the plan has not been approved. Keep the plan current, but never mark approval gates or user-verification items complete on the user's behalf.

### Save transcript

Agent: At the end of planning, remind the user that the transcript is a deliverable and ask them to say `save transcript`. Wait for that direction. When directed, save the entire conversation in the `transcripts/` directory as `plan-YYYY-MM-DD_HHMMSS.md`, mark user and agent responses clearly, and confirm the saved relative path.

Agent: At the end of every implementation chat, remind the user to say `save transcript`. When directed, save the entire conversation as `build-YYYY-MM-DD_HHMMSS.md` using the same location and formatting.
