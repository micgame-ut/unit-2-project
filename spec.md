# Technical Specification

> EDITING DIRECTIVE: USER AND AGENT EDIT THIS FILE COLLABORATIVELY. THE USER MUST REVIEW AND APPROVE ITS CONTENT.

Purpose of this file: Define what the completed project must do so it can be planned, built, and verified.

## Instructions for the user

Translate the approved research into a specification without distorting its evidence, limitations, or uncertainty. Direct the work toward the intended result, judge gaps and trade-offs rather than accepting invented requirements, and approve only a complete, testable specification grounded in the research.

## Instructions for the agent

Read AGENTS.md, brief.md, research.md, and this file. Begin with a concise orientation and one focused question.

Guide the specification one feature at a time. Help turn approved decisions into precise requirements and surface gaps or trade-offs without inventing requirements or making product decisions. Draft concise updates for review, focus on the intended result rather than implementation steps, and never approve the specification on the user's behalf.

## Goal

Give employees at a national creative-media company a way to estimate their professional AI use's carbon and water footprint, and see that estimate in context alongside their everyday digital habits and lifestyle — with clear methodology, sources, and honest uncertainty, so employees can reach their own informed conclusions rather than being told AI use is harmless or harmful.

## Features

For each feature, define:

- the need it addresses and intended audience outcome
- its behavior, inputs, and outputs
- its calculations, supporting evidence, and uncertainty
- its interface expectations and acceptance checks

### Feature 1: Beyond the prompt (image and video generation)

**Need / audience outcome:** Alex uses image- and video-generation tools for production work, not just chat prompts, and specifically cares about water use (per the reference profile). The current calculator only covers text-model prompts. This lets Alex — and Jordan, who also uses generative tools beyond chat — see that part of their usage reflected in the total, not left out.

**Behavior, inputs, outputs:**
- Two new row types, addable alongside the existing text rows in "Your AI use": **Image generation** and **Video generation**, each with a count-per-day input, matching the existing rows' interaction pattern (add row, set count, remove row).
- **Image generation** rows use a single, model-agnostic energy figure (not a model picker) — the source doesn't break results down by commercial tool.
- **Video generation** rows include a model picker listing the specific tools the source measured or estimated (open models: e.g. LTX-2, HunyuanVideo-1.5; proprietary: Veo 3, Seedance-1, Gen-4.5, Sora 2.0 Pro), each carrying its own per-clip energy figure and range at the duration/resolution the source reports for that model (these differ model to model, same as the paper).
- Both row types feed into the same daily/annual carbon total, comparisons, and bars that existing text rows already populate.
- **Water is excluded from both row types** — no verified per-image or per-video water figure exists (see research.md), and deriving one from an unverified WUE baseline would overstate confidence. Only carbon/energy is shown for image and video rows; the existing water total continues to reflect text prompts only.

**Calculations, evidence, uncertainty:**
- Image: Luccioni et al. 2024 (FAccT) — ~2.9 Wh/image (peer-reviewed, directly verified, single hardware config; presented as an order-of-magnitude estimate, not an exact figure, matching how the calculator already treats EcoLogits numbers).
- Video: Jegham, Gamazaychikov & Luccioni 2026 — open-model figures are directly measured (<3% MAPE, high confidence); proprietary-model figures are latency-derived estimates with wider stated ranges (e.g., Veo 3: 19.8–43.4 Wh; Sora 2.0 Pro: 315.1–534.4 Wh) reflecting the authors' own caveat about assumed hardware deployment.
- Both are excluded from training-cost accounting, consistent with the existing calculator's stated methodology.

**Interface expectations and acceptance checks:**
- Adding an image or video row changes the displayed daily/annual carbon total and comparison bars (not just an informational display) — verifies the "must contribute to the calculation" requirement.
- Every figure has a citation in the methodology section, consistent with the existing calculator's citation style.
- The methodology text distinguishes image (single order-of-magnitude figure) from video (per-model figures, open vs. proprietary confidence levels) and states plainly that water is not estimated for these two row types and why.
- Removing all image/video rows returns the total to exactly what it would be with only text rows (no residual effect).

### Feature 2: Project totals (coding-agent sessions)

**Need / audience outcome:** Jordan's coding-agent sessions are long and irregular, not a steady daily count. The existing calculator's "coding/agent session" option is a flat per-day row, annualized ×365 — confirmed by inspecting the code to have no way to enter a handful of sessions across a real project timespan and get a project total. This lets Jordan estimate an actual project instead of forcing it into a daily-then-annualized shape it doesn't fit.

**Behavior, inputs, outputs:**
- A separate "Project totals" section, distinct from the per-day row table (the existing per-day "coding/agent session" size option stays as-is for anyone who wants a flat daily estimate).
- Inputs: a model picker (reusing the existing model list) and a rough count of sessions for the project.
- Output: a project total shown as a **range**, not a single number, displayed next to a familiar comparison (e.g., a load of laundry, a mile driven) — standing on its own, not folded into the annual "ways you add" chart, since a project isn't something that repeats 365 times a year.

**Calculations, evidence, uncertainty:**
- Base per-session figure: EcoLogits' 100,000-output-token "assist application development" benchmark (same figure the existing per-day "agent" row already uses).
- Range: low end at the benchmark's own low CI; high end scaled to reflect Bai et al. 2026's finding that ~2x is the typical run-to-run spread for the same task (not the paper's tail-case ~30x, which the feature explicitly avoids treating as typical).
- A footnote states plainly that even similar-looking sessions vary, occasionally far more than the shown range, citing Bai et al.'s finding that human-rated difficulty barely predicts actual cost — communicating the limitation rather than hiding it.

**Interface expectations and acceptance checks:**
- The project total changes with the session count and model choice (verifies it contributes to the calculation, not just informational text).
- The range visibly widens/narrows based on the underlying CI and typical-spread multiplier, not a fixed cosmetic band.
- The project total is not added into the annual chart's totals.
- Citation to Bai et al. 2026 and the EcoLogits benchmark appears in the methodology section.

### Feature 3: Everyday digital habits (streaming, video calls, gaming)

**Need / audience outcome:** All three profiles named these activities (Alex: streaming/social on phone, laptop, TV; Jordan: gaming on a desktop PC, frequent calls; Robin: video meetings, streaming, social platforms), and the calculator currently has zero coverage of any of them (confirmed by inspecting the code). This lets someone compare their AI use against their own actual digital habits, not a generic fixed reference line.

**Behavior, inputs, outputs:**
- A new "Your digital habits" input section, structurally separate from "Your AI use": hours/day of streaming, hours/day of video calls (with a camera on/off toggle), and hours/day of gaming.
- These compute their own daily/annual carbon total, kept distinct from the AI total (not summed into it), and are shown side-by-side with the AI total in the existing "day of your AI use vs. everyday things" mini chart and the annual "ways you add" chart — replacing the generic fixed reference lines (e.g., "an hour on a PS5") with the person's own entered numbers where they overlap.
- Gaming's baseline device assumption (see feature #4) is a "typical" gaming PC unless changed via feature #4's device picker.

**Calculations, evidence, uncertainty:**
- Streaming: Kamiya/IEA 2020 — ~36 g CO2/hour on the global-average grid; institutional source, methodology transparent, and its "corrected 90x-inflated myth" framing (see feature #5) is a good fit for Robin's skepticism.
- Video calls: Mortas 2026 — camera on 10.2–21.6 g CO2e/hour, camera off 4.8–10.8 g CO2e/hour (roughly halves). Flagged as preliminary: small sample (9 meetings), single hardware/network/location configuration (French grid, unusually low-carbon).
- Gaming: Mills et al. 2019 (LBNL/Springer) — ~1,400 kWh/year for a typical gaming PC, ~6x a regular PC, ~10x a console. Verified only via secondary summaries since the primary paper is paywalled — treated as provisional.
- Social media browsing is deliberately excluded — no source was found quantifying it, and guessing would violate the "cite every claim" requirement.

**Interface expectations and acceptance checks:**
- Changing any habit hour input changes the "Your digital habits" total and the comparison charts it feeds (verifies it contributes to the calculation).
- The AI total and the digital-habits total remain visibly distinct numbers, never silently combined.
- Each activity's methodology note states its source, its confidence level (institutional/preliminary/provisional), and the grid or hardware assumption it rests on.
- Setting all three habit inputs to zero removes their bars from the comparison charts without affecting the AI total.

### Feature 4: Device matters (streaming and gaming)

**Need / audience outcome:** Extends feature #3's streaming and gaming inputs with a device selector, since device choice swings the estimate far more than most people expect. Serves Alex (named devices: phone, laptop, TV) and Jordan (desktop/gaming PC).

**Behavior, inputs, outputs:**
- Adds a device dropdown to the streaming row from feature #3: **phone / laptop / TV**. Adds a device dropdown to the gaming row: **older console (PS4 / Xbox One) / current console (PS5 / Xbox Series X) / regular PC / gaming PC** (revised 2026-09-27: the single "console" tier was split in two; see Revisions).
- Video calls get no device picker — Mortas 2026 is camera-on/off only, not device-specific.
- Changing the device recalculates that row's contribution to the "Your digital habits" total from feature #3; it's a multiplier on an existing input, not a new standalone total.

**Calculations, evidence, uncertainty:**
- Streaming device ratios, from Kamiya/IEA 2020 (directly verified): a 50" TV uses ~100x a smartphone and ~5x a laptop, so — anchored to phone = 1x — **phone 1x, laptop ~20x, TV ~100x** *for the device's own electricity*. Kamiya's own breakdown further attributes 72% of streaming's footprint to the device itself, 23% to data transmission, 5% to data centers — device dominance is well-supported for streaming specifically (unlike the rejected generic AI-use claim in feature #4's original scoping). **Revised 2026-09-27:** the ratios scale only that 72% device share; network and data-centre energy stay fixed. Kamiya's average (0.077 kWh/hour, confirmed via Carbon Brief's republication) is treated as a laptop — an assumption stated in the methodology. Result per hour: phone ≈ 0.024, laptop 0.077, TV ≈ 0.299 kWh (about 1 : 3 : 12 overall).
- ~~Gaming device figures, from the Mills research group (~1,400 kWh/yr gaming PC, ~233 kWh/yr regular PC, ~140 kWh/yr console)~~ **Revised 2026-09-27:** yearly kWh totals mix in hours of use and idle time, so they don't convert to an hour of play. Gaming now uses measured average power *during gameplay* (Berkeley Lab for the California Energy Commission, CEC-500-2019-042, Figure 7; 2016 hardware; directly verified — see research.md): regular PC (entry-level desktops) ~108 W (45–183), gaming PC (mid/high-end desktops) ~234 W (127–328), older console (PS4 / Xbox One models) ~89 W (60–128). Current consoles (PS5 / Xbox Series X) use NRDC 2021's ~160–200 W (typical 180), seen only via news coverage and flagged as unverified. This also replaces feature #3's derived 300 W gaming-PC figure.

**Interface expectations and acceptance checks:**
- Switching the streaming device between phone/laptop/TV changes the streaming bar by the device-share ratios (about 1 : 3 : 12; e.g. US grid ≈ 9 / 29 / 114 g CO2e per hour), not a placeholder or flat value. *(Revised 2026-09-27 from "roughly 1x/20x/100x".)*
- Switching the gaming device changes the gaming bar to that device's measured power during play (older console ~89 W, current console ~180 W, regular PC ~108 W, gaming PC ~234 W, each with its range). *(Revised 2026-09-27 from the 140/233/1,400 kWh/yr ratios.)*
- The methodology section states both sources, the laptop-as-average assumption, the dated (2016) hardware, and that the current-console figure is unverified.
- Device selection persists per-row (streaming and gaming can have different device settings independent of each other).

### Feature 5: Uncertainty control (range toggle + "what if Claude is dense")

**Need / audience outcome:** Robin is skeptical of the company's motives and needs plain-language explanations, visible sources, and honest uncertainty — not reassurance. This is the one flagged need in research.md with no feature yet, and the brief's requirement for a fifth feature addressing the strongest remaining need. Both parts change the displayed calculation, not just its wording, per the brief's requirement.

**Behavior, inputs, outputs — Part 1, range toggle:**
- A toggle (e.g., "Show as: typical / range") that switches every displayed carbon and water number — daily total, annual total, comparison bars — between a single point estimate and its low–high band.
- Uses the `whmin`/`whmax` 95% CI fields already present per model/size in the calculator's model data; no new sourcing needed, since the range is already computed and cited but currently unused in the UI.

**Behavior, inputs, outputs — Part 2, "what if Claude is dense":**
- A separate toggle, available only when at least one Claude model is in use, that recalculates Claude's figures under a "dense, not mixture-of-experts" assumption.
- Turns the calculator's existing text disclaimer ("if the dense camp is right, the Claude numbers here are too low") into something Robin can press and see move, rather than a caveat left in prose.

**Calculations, evidence, uncertainty:**
- Part 1: EcoLogits' own 95% CI, already the basis for the existing per-model low/high figures cited in the current methodology section.
- Part 2: ~~multiplies Claude's current figures by **~3.3x–10x**, derived from EcoLogits' own stated Claude 3 Opus assumption … (confirmed: EcoLogits' own model data has no Claude entries at all).~~ **Revised 2026-09-27 (user decision):** EcoLogits' model data does have current Claude entries (see research.md correction): Opus 4.8 and Sonnet 4.6 are mixture-of-experts with 10–30% of parameters active; Haiku 4.5 is already dense and is left out. Because EcoLogits' energy and water are linear in active parameters, the dense estimate extends EcoLogits' own low-to-high line to every parameter active (Opus 670B, Sonnet 440B): about 3.0x (Sonnet) / 3.5x (Opus) typical energy, ~3.5–4.3x water, embodied emissions unchanged. In range view with dense on, the band runs from EcoLogits' low estimate to the dense estimate. The toggle appears only when Opus or Sonnet is in use (a usage row or the project).
- Both toggles apply only to already-computed figures; neither introduces a new resource-use claim beyond what the existing methodology already cites.

**Interface expectations and acceptance checks:**
- Toggling "range" changes every visible carbon/water number simultaneously to its low–high band and back — verifies it's a real display recalculation, not a static alternate view.
- Toggling "dense Claude" changes only Claude-attributable figures (rows using a Claude model, and any totals that include them); non-Claude figures are unaffected.
- Both toggles can be on simultaneously (a dense-Claude range, showing an even wider band).
- The methodology section states both toggles' basis and caveats in plain language, directly addressing why the numbers move and how uncertain each part is — matching Robin's stated need for honest uncertainty over reassurance.

## User approval

Review the completed specification directly and explicitly approve it before planning begins. The agent cannot complete this approval on the user's behalf.

## Out of scope

- **Social media browsing footprint** — no credible source found quantifying it; guessing would violate the "cite every claim" requirement. Could be revisited later if a source turns up.
- **Water use for image/video generation** — no verified per-image or per-video water figure exists; deriving one from an unverified water-usage-effectiveness baseline would overstate confidence. Only carbon/energy is estimated for these two row types.
- **A device effect on AI use itself** (as opposed to streaming/gaming) — the only source claiming this (The Shift Project) has the same reliability problem as its already-corrected streaming claim, and contradicts how server-side inference actually works.
- **Training-cost accounting** for any feature, including image/video generation — excluded as too uncertain, consistent with the existing calculator's stated methodology.
- **A presentation-only transparency/methodology panel** for Robin's need — rejected as the sole approach to feature #5, since the brief requires features to change the calculation, not just present it more clearly.
- **Pursuing further disclosure of Claude's actual architecture** — confirmed absent; no official Anthropic source exists, and continued searching is unlikely to change that.

## Revisions

If implementation changes the intended result, update the specification and record what changed and why.

**2026-09-27 — Feature 4, streaming device scaling (user decision).** Kamiya gives only device ratios and says the device is 72% of streaming's energy. Scaling the whole per-hour figure by 1x/20x/100x would imply a phone stream uses almost no network or data-centre energy, which contradicts that split. The user chose to scale only the device share, with the 0.077 kWh/hour average treated as a laptop. The streaming acceptance check changes from "roughly 1x/20x/100x" to the resulting ~1 : 3 : 12.

**2026-09-27 — Feature 4, gaming devices (user decision, after new research).** The spec's yearly kWh ratios (140/233/1,400) mix in hours of use and idle time, so they don't describe an hour of play (applied per hour they'd put a console at ~30 W). At the user's direction, per-hour power during gameplay was researched (recorded in research.md): Berkeley Lab's directly verified measurements for PCs and PS4/Xbox One-era consoles, and NRDC's PS5 / Xbox Series X figures, seen only through news coverage. The user chose to offer both console generations, so the gaming picker has four devices instead of three, and the gaming PC changes from feature #3's derived 300 W to the measured ~234 W.

**2026-09-27 — Feature 5, dense Claude (user decision, after checking EcoLogits' data).** The spec's "no Claude entries" premise was wrong: EcoLogits models Opus 4.8 and Sonnet 4.6 as mixture-of-experts (10–30% active) and Haiku 4.5 as dense. A flat 3.3–10x would double-count Haiku and overstate Opus/Sonnet (the 10x end implies more active parameters than the model's total), because part of each reply's energy doesn't scale with model size. The user chose to extend EcoLogits' own linear relationship to all parameters active for Opus and Sonnet only (~3–3.5x energy). Also, "typical" view now shows single figures everywhere (including the habit notes and the headline detail, which previously always showed a range), so the toggle switches every AI and habit number; project totals stay a range per Feature 2.

**2026-09-27 — Feature 3, streaming energy figure.** Streaming uses Kamiya's quoted 0.077 kWh/hour (confirmed via Carbon Brief's republication) instead of the 0.075 kWh derived earlier from the page's 480 g/kWh world average; US laptop streaming moves from 28.5 to 29.3 g CO2e per hour.

## Commands

### Start specification

User: Open the project repository as your workspace, start a fresh chat, and type `start specification`.

### Save transcript

Agent: After the user approves the specification, remind them that the transcript is a deliverable and ask them to say `save transcript`. Wait for that direction.

When the user directs the agent to save the transcript, the agent saves the entire conversation in the `transcripts/` directory as `spec-YYYY-MM-DD_HHMMSS.md`, marks user and agent responses clearly, and confirms the saved relative path.
