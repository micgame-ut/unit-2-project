# Research

> EDITING DIRECTIVE: USER AND AGENT EDIT THIS FILE COLLABORATIVELY. THE USER MUST REVIEW AND APPROVE ITS CONTENT.

Purpose of this file: Develop and record the evidence and decisions that will guide the technical specification.

## Instructions for the user

You are responsible for the ethics, accuracy, and fairness of the research. Direct the inquiry toward useful questions, judge sources and suggestions rather than accepting them at face value, and approve only results supported by verified evidence and audience needs. Seek evidence that challenges your assumptions, represent uncertainty honestly, and reject claims you cannot verify. See [UNESCO's Guidance for generative AI in education and research](https://www.unesco.org/en/articles/guidance-generative-ai-education-and-research).

## Instructions for the agent

Read AGENTS.md, brief.md, and this file. Begin with a concise orientation and one focused question.

Guide the research one stage at a time. Help the user explore options, assess sources, and identify contrary evidence or uncertainty without making decisions for them. Draft concise updates for review, and never mark research or feature choices approved on the user's behalf.

## Reference employee profiles

- Alex — Los Angeles, 24, junior video editor: Uses text, image, and video-generation tools for production work. Streams reference media and uses social platforms across a phone, laptop, and television. Wants to understand impacts beyond text prompts and is particularly attentive to water use.
- Jordan — Austin, 38, creative technologist: Uses coding agents and generative tools in long, irregular sessions. Games on a desktop PC and participates in frequent video calls. Finds "prompts per day" too simplistic and wants assumptions, ranges, and project-level totals.
- Robin — Chicago, 56, operations manager: Uses text AI occasionally but spends substantial time in video meetings, streaming media, and social platforms. Is skeptical of the company's motives and needs plain-language explanations, visible sources, and honest indications of uncertainty.

These are fictional starting profiles, not evidence about demographic groups. Research the activities, circumstances, and needs they represent rather than making assumptions based on age or location.

## Audience needs

Record information about the activities, circumstances, and needs represented by all three reference profiles. Separate evidence from assumptions that still need checking.

**Given directly by the profiles (established, not assumptions):**

- Alex uses text, image, and video-generation tools for production work — not just chat prompts. Streams reference media and uses social platforms across phone, laptop, and TV. Explicitly cares about water use.
- Jordan uses coding agents and generative tools in long, irregular sessions — "prompts per day" doesn't fit this pattern. Games on a desktop PC, frequent video calls. Wants assumptions, ranges, and project-level totals rather than a single prompt count.
- Robin uses text AI only occasionally, but spends substantial time in video meetings, streaming, and social platforms. Skeptical of the company's motives — needs plain-language explanations, visible sources, and honest uncertainty, not reassurance.

**Needs this implies, still to verify with evidence:**

- Image/video-generation resource use is likely far higher per unit than a single text prompt (needs a source — the current calculator only covers text-model prompts via EcoLogits).
- Long/irregular agentic coding sessions don't map cleanly onto "prompts" — need a way to estimate resource use from tokens, session length, or another unit that Jordan would find credible. **Confirmed by inspecting the current calculator:** it already has a "coding / agent session" preset (EcoLogits' 100,000-output-token "assist application development" benchmark) as one flat per-day row, but the whole tool is built around a *per-day, then annualized* cadence — there's no way to enter a handful of irregular sessions over a project's actual timespan and get a project total, and no visible range beyond EcoLogits' own 95% CI. This is a real, unmet gap, not just Jordan's perception.
- Comparisons to everyday digital habits (streaming, video calls, gaming, social media) need per-hour or per-session resource-use figures with citations, the same way the current calculator cites driving/food/appliances.
- Robin's skepticism is a design/communication need, not a data need: methodology transparency, source visibility, and uncertainty framing (partly a features question, partly how existing features are presented).

**Resolved:** whether device/platform (phone vs. laptop vs. TV vs. desktop PC) meaningfully changes the resource-use estimate. For AI compute itself, no — inference runs server-side regardless of the device used to access it, and the only claim found that argues otherwise (device/network >> data center) traces to The Shift Project, the same source whose streaming figure is already flagged below as ~90x too high (see Source assessments). For everyday digital habits, yes — device already drives the estimate for streaming (Kamiya/IEA, up to 100x between a smartphone and a 50" TV) and gaming (Mills et al., the device's own power draw); this became feature #4 below rather than an AI-side feature.

## Possible features

Generate several possibilities before choosing. Keep the initial notes brief. For each idea, record:

- what it would help someone learn or do
- the profiles or needs it would serve
- any evidence or implementation challenge that might affect it

- **"Beyond the prompt" — image/video generation resource use.** Would let someone estimate the footprint of image- and video-generation tool use, not just text prompts. Serves Alex directly (named need, water-use concern) and touches Jordan (uses generative tools beyond chat). Energy figures are now well-supported for both image generation (Luccioni et al. 2024) and video generation (Jegham, Gamazaychikov & Luccioni 2026) — video costs roughly 30x an image and ~1,600-2,000x a text prompt, scaling quadratically with resolution/duration. Water use is the remaining weak point: no verified per-image or per-video water figure exists yet (see Source assessments), so that part of the feature needs a more visible disclaimer than the energy part, or should be deferred until a WUE figure is verified.

- **"Everyday digital habits" — video calls, streaming, and gaming footprint comparisons.** The current calculator has zero coverage of these (confirmed by inspecting the code) despite all three profiles naming them: Alex streams reference media and uses social platforms on phone/laptop/TV; Jordan games on a desktop PC and has frequent video calls; Robin spends substantial time in video meetings, streaming, and social platforms. This directly serves the brief's requirement that at least two features connect AI use to broader digital life. Evidence found is solid for streaming and gaming (peer-reviewed/institutional), and preliminary-but-usable for video calls (small pilot study). Bonus: the streaming figure has the same "corrected-viral-myth" story as the AI water-use figure, which is a good fit for Robin's skepticism (showing sources get corrected, not just cited). Social-media browsing itself remains unsourced — likely low-footprint relative to video, but not yet verified, so left out for now rather than guessed.

- **"Project totals" — irregular coding-agent sessions over a project's real timespan, with an honest range.** Instead of forcing agent sessions into the tool's per-day cadence, let someone enter a handful of sessions (or a rough session count) across a project and see a total, presented as a range rather than a single number — reflecting how much real agentic token use actually varies. Serves Jordan directly (named need: "prompts per day" too simplistic, wants assumptions/ranges/project totals). Challenge: the best evidence found says token use for the *same* task typically varies ~2x run-to-run (occasionally much more), and difficulty ratings barely predict cost — so a single flat number would misrepresent it, and the feature needs to communicate a range rather than false precision.

- **"Device matters" — a device picker for streaming and gaming.** Lets someone pick the device they actually use (phone, laptop, TV for streaming; console vs. gaming PC) and see the estimate swing accordingly, instead of one flat per-hour figure. Serves Alex directly (named devices: phone, laptop, TV) and Jordan (desktop PC gaming). Distinct from "Everyday digital habits" above: that feature is about which activities and how much time; this one is about how much the device itself changes the answer, which per the sourced evidence is a bigger lever than most people expect (streaming: up to 100x smartphone vs. TV). Uses sources already vetted under "Everyday digital habits" (Kamiya/IEA for streaming, Mills et al. for gaming) — no new sourcing needed. Explicitly does not extend to AI use itself (see "Resolved," above, and the Shift Project rejection below).

- **"Uncertainty control" — a range toggle plus a "what if Claude is dense" toggle.** Addresses Robin's need (plain-language explanations, visible sources, honest uncertainty, not reassurance) as the strongest remaining need with no assigned feature. Two parts: (1) a display toggle that swaps every point-estimate number for its low–high band, using the `whmin`/`whmax` 95% CI already in the EcoLogits-derived model data — no new sourcing needed, since the range is already computed and cited, just not shown. (2) A toggle that recalculates Claude's figures under the "dense, not mixture-of-experts" architecture assumption, operationalizing the already-verified "Cross-cutting: Claude's architecture" finding below — turning an existing text disclaimer into a control Robin can press and see move. Both change the displayed calculation rather than just wording, satisfying the brief's requirement.

## Source assessments

For each source, record:

- the full citation and working link
- the claim or figure the project may use
- evidence checked directly
- important limitations or uncertainty
- confidence and decision: use, use with qualifications, or reject

**Cross-cutting: Claude's architecture (verifies an existing disclaimer, not a new feature)**

- **Citation:** EcoLogits methodology, "Proprietary models." https://ecologits.ai/latest/methodology/proprietary_models/ — plus corroborating search across Anthropic's own model announcements/system cards and independent AI-analyst coverage (LifeArchitect.ai, and reporting on Elon Musk's unconfirmed claims about Opus/Sonnet parameter counts).
  - **Claim/figure:** Anthropic has never officially disclosed parameter counts or architecture (dense vs. mixture-of-experts) for any Claude model. EcoLogits' Claude 3 Opus estimate (~2 trillion total parameters, sparse MoE, 200B–600B active) is derived purely by comparing Claude's benchmark scores to GPT-4's separately-leaked architecture — not from any Claude-specific disclosure.
  - **Evidence checked directly:** Yes — fetched EcoLogits' methodology page directly, and independently searched for any official Anthropic architecture disclosure (system cards, model announcements); found none. Every parameter-count figure in public circulation for any Claude model is explicitly caveated by its own source as an unconfirmed leak, tweet, or analyst estimate, not an Anthropic-published number.
  - **Limitations/uncertainty:** This is a "confirmed absence" finding — I can't prove Anthropic will never disclose this, only that nothing found as of this research counts as an official disclosure.
  - **Confidence and decision:** **Use** — reinforces (doesn't change) the original calculator's existing disclaimer that Claude's numbers are the "shakiest" and may be underestimates if Claude is actually dense rather than sparse. Relevant to any feature that estimates Claude-specific resource use, including the image/video feature above if Claude's image tools are ever included.

**For "Beyond the prompt" (image/video generation):**

- **Citation:** Luccioni, A. S., Jernite, Y., & Strubell, E. (2024). "Power Hungry Processing: Watts Driving the Cost of AI Deployment?" *ACM Conference on Fairness, Accountability, and Transparency (FAccT '24)*. https://arxiv.org/abs/2311.16863
  - **Claim/figure:** Across 1,000 inferences on identical hardware (8x NVIDIA A100 GPUs, AWS), image generation averaged 2.907 kWh (≈2.9 Wh/image), vs. 0.047 kWh for text generation (≈0.047 Wh/query) — image generation ~60x more energy per output in their tests. Also gives image captioning (0.063 kWh/1000) and image/text classification figures.
  - **Evidence checked directly:** Yes — fetched the full paper (arXiv HTML), confirmed the exact figures, hardware, sample size (1,000 inferences × 3 datasets × 10 repeats), and carbon-intensity assumption (297.6 g CO2e/kWh, AWS Oregon).
  - **Limitations/uncertainty:** Authors state this is "not representative of all deployment contexts" — single hardware config, non-batched inference, doesn't cover diffusion-model variants released after the study, and gives no video-generation figure at all.
  - **Confidence and decision:** High confidence for the energy comparison order-of-magnitude; peer-reviewed, methodology transparent. **Use**, cited as an estimate/order-of-magnitude, not an exact per-model figure (matches how the current calculator already treats EcoLogits numbers as estimates).

- **Citation:** Li, P., Yang, J., Islam, M. A., & Ren, S. "Making AI Less 'Thirsty': Uncovering and Addressing the Secret Water Footprint of AI Models." *Communications of the ACM* (2025); preprint at https://arxiv.org/abs/2304.03271
  - **Claim/figure:** Establishes the energy × water-usage-effectiveness (WUE) method for estimating AI water footprint, and figures for GPT-3 scale training/inference. Does not give an image/video-generation-specific water figure.
  - **Evidence checked directly:** Only via search-result summary, not the full paper — flagged for follow-up if we use it.
  - **Limitations/uncertainty:** Focused on text-model (GPT-3) training, not image/video inference.
  - **Confidence and decision:** **Use with qualification** — as the methodological basis for deriving a water estimate (energy-per-image × WUE), the same way the existing calculator derives carbon from EcoLogits energy × Ember's grid carbon intensity. Not a source for a direct image-water number.

- **Citation:** Data-center water usage effectiveness (WUE) baseline, commonly cited as ~1.8 L/kWh on-site (Shehabi et al., 2016 U.S. Data Center Energy Usage Report, LBNL), with newer estimates for efficient/hyperscale facilities cited around 0.18–0.3 L/kWh (2017–2023).
  - **Evidence checked directly:** **No** — only found through secondary summaries; the LBNL PDF blocked direct fetch (403). Not verified — do not use until pulled from the primary report.
  - **Limitations/uncertainty:** Wide range (0.18–1.8+ L/kWh) depending on facility age/efficiency/climate; off-site (power-plant) water use adds more on top (~7.6 L/kWh cited elsewhere, also unverified here).
  - **Confidence and decision:** **Reject for now / needs direct verification** before any number is used in the spec.

- **Citation:** understandingyourai.org, "Does AI Really Use 10 Gallons of Water Per Image?" — https://understandingyourai.org/ai-water-usage-myth/
  - **Claim/figure:** Argues viral "gallons per image" claims are exaggerated 100–600x; states "no peer-reviewed study has measured water usage specifically for AI image generation."
  - **Evidence checked directly:** Read the article directly; also checked the site's authorship.
  - **Limitations/uncertainty:** Run by a marketing consultant (Tim Bish / Socoz Design LLC), not an academic or journalistic outlet — no disclosed methodology of its own, aggregates others' numbers.
  - **Confidence and decision:** **Reject as a citable source** for this project (doesn't meet the bar the original calculator sets), but its core caveat — no peer-reviewed image-specific water study exists — matches what independent searching also found, so it's a useful uncertainty flag, not a number to cite.

- **Citation:** Jegham, N., Gamazaychikov, B., & Luccioni, A. S. (2026). "Lights, Camera, Carbon: Architectural Scaling Laws for Video Generation Energy Consumption." Sustainable AI Group. arXiv:2607.04553. https://arxiv.org/abs/2607.04553
  - **Claim/figure:** Directly measured (not estimated) energy for 6 open video models (8.3B–27B params) on real GPUs (H200/B200), sampled every 100ms. Per-clip energy ranges enormously by resolution/duration/steps: ~2.95 Wh (LTX-2, 1024p, 3s) up to 478.5 Wh (HunyuanVideo-1.5, 8s, 8-GPU config). For proprietary models (8s, 720p, estimated from API latency + assumed hardware): Veo 3 ≈19.8–43.4 Wh, Seedance-1 ≈72.1–87.9 Wh, Gen-4.5 ≈241.6–410.9 Wh, Sora 2.0 Pro ≈315.1–534.4 Wh. An 8-second 720p video can cost **~1,600–2,000x** the energy of a single text prompt, and roughly **30x** a single generated image, though the exact multiple depends heavily on resolution/duration/model. Energy scales *quadratically* with resolution and video length (not linearly) — a 6-second clip can cost far more than double a 3-second one.
  - **Evidence checked directly:** Yes — fetched the full paper (arXiv HTML), confirmed hardware, per-model figures, the scaling-law derivation, and stated error margins (<3% MAPE for open models).
  - **Limitations/uncertainty:** Authors themselves flag: proprietary-model figures carry "additional uncertainty from assumed hardware deployment and power draw" that can't be directly verified (unlike the open-model measurements); results are specific to compute-bound diffusion architectures and may not generalize to other video-gen approaches; only 6 open models tested; one model (Veo 3.1) had noisier data from API queuing.
  - **Confidence and decision:** **Use** — open-model figures are directly measured and high-confidence; proprietary-model figures (the ones most tools people actually use) should be presented as estimates with visibly wider uncertainty, mirroring how the authors themselves separate the two.
  - **Remaining gap:** This paper covers energy, not water. A video's water footprint would still have to be *derived* (energy × WUE), which compounds with the still-unverified WUE figure above — so water for video generation should carry a bigger, more visible uncertainty caveat than energy does, or be left out of the water part of the feature until WUE is verified.

**For "Project totals" (Jordan's coding-agent need):**

- **Citation:** Bai, L., Huang, Z., Wang, X., et al. (2026). "How Do AI Agents Spend Your Money? Analyzing and Predicting Token Consumption in Agentic Coding Tasks." Microsoft Research. arXiv:2604.22750. https://arxiv.org/abs/2604.22750
  - **Claim/figure (verified against the full paper, not just the abstract):** Tested 8 frontier models (Claude Sonnet 3.7/4/4.5, GPT-5/5.2, Qwen3-Coder-480B, Kimi-K2, Gemini-3-Pro) on 500 SWE-bench Verified instances. Agentic coding tasks consume **3,500x more tokens than a single-round reasoning task** and **1,200x more than a multi-round chat task** — larger than the abstract's rough "1000x" framing suggested. Input tokens (not output) drive cost. Human-rated difficulty barely predicts cost (Kendall τb = 0.32): 6.7% of "<15-minute" tasks used more tokens than the average ">1-hour" task, and 11.1% of ">1-hour" tasks used fewer than the average "<15-minute" task. Models themselves can't predict their own usage well (best correlation 0.39, Claude Sonnet 4.5) and systematically underestimate it.
  - **Important correction to my earlier read:** the "**30x**" figure is an extreme/tail case across the whole 500-task set, not typical run-to-run variance. The paper's own Figure 2(b) shows **~2x is the typical spread** between the cheapest and most expensive run of the *same* task; the gap between the cheapest and most expensive *problem* (not run) averaged ~7 million tokens. Higher spend also doesn't buy higher accuracy — accuracy peaks at intermediate cost, and expensive failed runs often just repeat the same file actions.
  - **Evidence checked directly:** Yes — pulled the full paper via its HTML rendering (arxiv.org/html/2604.22750), not just the abstract.
  - **Limitations/uncertainty:** Built on SWE-bench Verified, a benchmark — real-world tasks (like Jordan's actual work) may distribute differently. Preprint — Microsoft Research-authored, but not yet confirmed peer-reviewed/published at a venue.
  - **Confidence and decision:** **Use with qualification.** Solid basis for the feature's core design point: a single "typical session" number would be misleading, so present a range and say plainly that even similar-looking tasks vary — but design around the **~2x typical / occasionally far more** picture, not the headline "30x," to avoid overstating routine variance.

- **Rejected sources:** Several SEO-style pages (terseai.org, ai-cost-estimator.com, tokenade.net, kunalganglani.com) surfaced similar "50K–500K+ tokens, sometimes 1M+" claims. These read as marketing/content-mill pages with no disclosed methodology — **not citable**, though their rough range is consistent with the peer-source-adjacent Microsoft paper's framing, so it's not contradicted, just not independently trustworthy.

**For "Everyday digital habits" (video calls, streaming, gaming — shared need):**

- **Citation:** Kamiya, G. (2020). "The carbon footprint of streaming video: fact-checking the headlines." International Energy Agency (IEA) commentary, 10 December 2020. https://www.iea.org/commentaries/the-carbon-footprint-of-streaming-video-fact-checking-the-headlines
  - **Claim/figure:** ~36 g CO2/hour of streaming on the global average grid (18 g per 30-min show); varies hugely by device (a 50" LED TV uses ~100x a smartphone and ~5x a laptop, so laptop ≈ 20x a smartphone) and by grid (France ~2 g CO2/hour on its low-carbon grid vs. ~36 g global average). Resolution matters too: SD ≈0.7 GB/hr, HD ≈3 GB/hr, 4K ≈7 GB/hr. Kamiya's own breakdown (average habits, not Shift Project's) attributes 72% of streaming's footprint to the viewing device itself, 23% to data transmission, and 5% to data centers — device dominance is real for streaming specifically, unlike the rejected generic AI-use claim above.
  - **The myth being corrected:** The widely-repeated Shift Project claim of 3.2 kg CO2/hour was **~90x too high** — Kamiya traces this to a 6x bitrate error, a ~35x data-center-intensity error, and a ~50x network-intensity error compounding together.
  - **Evidence checked directly:** Yes — fetched the IEA analysis directly, confirmed the figures, methodology (2019 Netflix bitrate/device-mix data, IEA grid carbon factors), and the error breakdown for the corrected myth.
  - **Limitations/uncertainty:** From 2020 — streaming bitrates and grid mixes shift over time. Excludes set-top boxes/game consoles as streaming devices. Netflix's reported electricity use includes some non-streaming (studios/offices) load.
  - **Confidence and decision:** **Use** — institutional source (IEA), transparent methodology, and its "corrected 90x-inflated myth" framing is directly useful for Robin's need to see sources get corrected rather than taken at face value.

- **Citation:** Mortas, F. (2026). "Assessing the Carbon Footprint of Virtual Meetings: A Quantitative Analysis of Camera Usage." arXiv:2601.06045, January 2026. https://arxiv.org/abs/2601.06045
  - **Claim/figure:** Camera on: 10.2–21.6 g CO2e/hour of video calling; camera off: 4.8–10.8 g CO2e/hour — turning the camera off roughly halves the footprint.
  - **Evidence checked directly:** Yes — fetched the full paper. Methodology: 9 Microsoft Teams meetings (30–60 min each), one HP laptop, one 4G connection in France, data-transfer-based estimate using French grid carbon intensity (79.1 g CO2e/kWh).
  - **Limitations/uncertainty:** Authors themselves call it "preliminary" — very small sample (9 meetings), single hardware/network/location configuration, no screen-sharing scenarios tested, and they note energy-intensity assumptions "vary significantly across existing studies." French grid is unusually low-carbon (nuclear-heavy), so the gram figures would be higher on other grids.
  - **Confidence and decision:** **Use with qualification** — treat as an order-of-magnitude/relative figure (camera on ≈2x camera off), not a precise number; flag the small sample and single-country grid assumption openly, the same way the current calculator flags EcoLogits' own caveats.

- **Citation:** Mills, E., Bourassa, N., Rainer, L., Mai, J., Shehabi, A., & Mills, N. (2019). "Toward Greener Gaming: Estimating National Energy Use and Energy Efficiency Potential." *The Computer Games Journal* (Springer), with Lawrence Berkeley National Laboratory. https://link.springer.com/article/10.1007/s40869-019-00084-2
  - **Claim/figure:** Tested 26 gaming systems. Nameplate power for low/typical/high-efficiency complete systems ≈300/600/900 W, but *measured* power draw runs about 50% of nameplate for most systems (so roughly 150–450 W actual, in line with commonly-cited "250–400 W" figures). GPU power alone ranges 60–500 W. A typical gaming PC uses ~1,400 kWh/year — about 6x a typical (non-gaming) PC (≈233 kWh/yr) and ~10x a gaming console (≈140 kWh/yr).
  - **Evidence checked directly:** Partially — the paper itself is paywalled (Springer login required); figures confirmed via Berkeley Lab's own news coverage of their study (eta.lbl.gov) and a research-summary aggregator, cross-checked against each other, not the original full text. **Correction:** the "1,400 kWh/yr, 6x/10x" headline figure traces most directly to an earlier Mills (2015) Berkeley Lab study/report ("Taming the Energy Use of Gaming Computers"), not originally to this 2019 paper — the 2019 paper appears to restate the same lead author's earlier finding rather than being its original source. Treat the figure as Mills-group-attributed, not exclusively this citation.
  - **Limitations/uncertainty:** Underlying measurements are from 2015–2019 — hardware efficiency has likely shifted since. Wide system-to-system variation (5–1,200 kWh/year across configurations) means any single "typical" figure hides a lot of spread.
  - **Confidence and decision:** **Use with qualification** — peer-reviewed and LBNL-affiliated (same rigor tier as the original calculator's other sources), but since I couldn't read the full paywalled text directly, treat the specific numbers as provisional until independently re-confirmed, and keep the range wide rather than picking one "typical gaming hour" number.

- **Added 2026-09-27 during Feature 4 build (per-hour gaming power by device), at the user's direction:**
  - **Citation:** Mills, E., Bourassa, N., Rainer, L., Mai, J., Shehabi, A., & Mills, N. (2019), "A Plug-Loads Game Changer: Computer Gaming Energy Efficiency without Performance Compromise." California Energy Commission, CEC-500-2019-042 (LBNL final project report, April 2019). Full text via the Berkeley Lab Green Gaming publications page: https://greengaming.lbl.gov/publications
    - **Claim/figure:** Average system power *during gameplay*, measured across 26 systems (2016 market): desktops 34–410 W, laptops 21–212 W, consoles 11–158 W, media-streaming devices ~4 and ~8 W. Read from its Figure 7 (per-system averages across games, ±~5 W chart-reading precision): entry-level desktops E1 ~62, E2 ~45 (integrated graphics only), E3 ~183, E4 ~140 W (mean ~108); mid-range M1 ~127, M2 ~192, M3 ~282, M4 ~235 W; high-end H1 ~328, H2 ~240 W (mid + high mean ~234 W); laptops ~25–170 W (mean ~72); consoles PS3 ~82, PS4 Slim ~72, PS4 Pro ~128, Xbox 360 ~104, Xbox One ~95, Xbox One S ~60, Wii ~18, Wii U ~30, Switch ~12 W.
    - **Evidence checked directly:** Yes — downloaded the full report PDF and the full 2019 *Computer Games Journal* paper (both from the authors' own links), read the text, and rendered Figure 7 to read the per-system bars. The 2019 paper confirms the same 26-system test set and says detailed results are in Bourassa et al. (2018b). The paper's text does not state the "1,400 kWh/yr typical gaming PC" figure, consistent with the earlier correction that it traces to Mills (2015).
    - **Limitations/uncertainty:** 2016-era hardware; per-system values are read from a chart, not a table; power varies strongly by game (e.g. 15–270 W on one desktop depending on title). Consoles tested are previous generations — current PS5 / Xbox Series X are not covered.
    - **Confidence and decision:** High confidence as measured data for the systems tested; pending the user's decision on how to map it to the console / regular PC / gaming PC tiers.
  - **Citation:** NRDC (Noah Horowitz), "Latest Game Consoles: Environmental Winners or Losers?" (2021). https://www.nrdc.org/bio/noah-horowitz/latest-game-consoles-environmental-winners-or-losers
    - **Claim/figure:** PS5 and Xbox Series X draw roughly 160–200 W playing current-generation games (e.g. ~180–200+ W for Astro's Playroom; ~80–104 W for a PS4-era title); previous generation PS4 ~137 W, Xbox One ~112 W.
    - **Evidence checked directly:** **No** — the NRDC page blocked automated access three times (HTTP 403 / bot check). Figures seen only in secondary news coverage (GamesRadar, Tom's Guide, Tech Times) that attributes them to NRDC's own measurements.
    - **Confidence and decision:** Plausible and from a reputable NGO, but unverified here; use only with a visible "confirmed via secondary coverage" caveat, if at all.

- **Social media browsing:** Not yet researched. Flagged as an open item, not assumed to be negligible or significant either way.

**For "Device matters" (and for resolving the device/AI-use open question):**

- **Citation:** The Shift Project, "Environmental impacts of digital technology — 5-year trends and 5G governance" (March 2021). https://theshiftproject.org/wp-content/uploads/2023/04/Environmental-impacts-of-digital-technology-5-year-trends-and-5G-governance_March2021.pdf — surfaced via secondary blogs (marmelab.com, arbor.eco) repeating its claim that data centers are under 15% of a web service's footprint, with the rest split across network, user device, and equipment manufacturing.
  - **Claim/figure:** Device + network + embodied manufacturing emissions dominate over data-center energy for typical web/digital service use (>85% vs. <15%).
  - **Evidence checked directly:** Partially — traced the claim from secondary blogs back to the Shift Project PDF as its origin; did not re-derive the 15% figure from the PDF's own methodology.
  - **Limitations/uncertainty:** This is the same organization whose streaming-carbon figure (3.2 kg CO2/hour) Kamiya/IEA already found to be ~90x too high due to compounding methodology errors (see "Everyday digital habits" sources above). No independent, AI-specific source was found corroborating a device/network effect on AI *compute* itself, which is inconsistent with how inference actually works (compute happens server-side regardless of the client device).
  - **Confidence and decision:** **Reject** as a basis for an AI-use device feature — same source-reliability problem as its already-rejected streaming claim. Does not affect the "Device matters" feature above, which relies on Kamiya/IEA and Mills et al. instead, not this source.

## Selected features

1. **Beyond the prompt** (professional AI use) — image- and video-generation resource use, alongside text prompts. Serves Alex directly (named need, water-use concern); touches Jordan. Energy figures well-sourced (Luccioni et al. 2024 for images; Jegham, Gamazaychikov & Luccioni 2026 for video); water figures remain unverified and will carry a visible disclaimer or be deferred.
2. **Project totals** (professional AI use) — irregular coding-agent sessions across a project's real timespan, presented as a range rather than a flat "prompts/day" number. Serves Jordan directly (named need). Sourced to Bai et al. 2026 (Microsoft Research), with the range framed around the paper's typical ~2x run-to-run spread, not its tail-case ~30x figure.
3. **Everyday digital habits** (broader digital life) — adds streaming, video-call, and gaming footprint comparisons, none of which the current calculator covers. Serves all three profiles (Alex: streaming/social; Jordan: gaming/calls; Robin: meetings/streaming/social). Sourced to Kamiya/IEA 2020 (streaming), Mortas 2026 (video calls, preliminary), and Mills et al. 2019 (gaming, provisional pending full-text confirmation).
4. **Device matters** (broader digital life) — a device picker for streaming and gaming that shows how much device choice itself changes the estimate (up to 100x for streaming, per Kamiya/IEA). Serves Alex (named devices: phone, laptop, TV) and Jordan (desktop PC). Explicitly excludes an AI-use device effect, since the only source found for that (The Shift Project) was rejected for the same reliability problem as its already-corrected streaming claim.
5. **Uncertainty control** (strongest remaining need) — a range toggle (using the model data's existing 95% CI) and a "what if Claude is dense" toggle (operationalizing the already-verified architecture-uncertainty finding), so the calculation itself communicates uncertainty rather than a caveat in prose. Serves Robin directly (skeptical of the company's motives; needs visible sources and honest uncertainty, not reassurance).

**Coverage check:** #1 and #2 improve professional AI use (≥2 required); #3 and #4 connect AI use to broader digital life (≥2 required); #5 addresses the strongest remaining need (Robin's transparency need, the one flagged gap with no feature). All three profiles are served by at least one feature; Alex and Jordan each by three, Robin by two (#3 and #5).

**Serious alternatives considered and rejected:**
- **Social media browsing footprint** (candidate for #3 or #4): left out entirely — no credible source found yet quantifying it, and guessing a figure would violate the "cite every claim" requirement. Could be revisited later if a source turns up, but not blocking this feature set.
- **Device effect on AI use itself** (candidate for #4): rejected — the only source claiming this (The Shift Project) has the same reliability problem as its already-corrected streaming claim, and contradicts how server-side inference actually works.
- **A pure "sources/methodology" panel for Robin** (candidate for #5): rejected as the sole design — the brief requires features to change the calculation, not just present it more clearly, so the range and dense-Claude toggles were added to make the feature calculation-affecting rather than presentational.

User approval: Review the completed research directly. Confirm that sources exist and support the claims the project will use, correct the document as needed, and explicitly approve the selected features before developing the specification. The agent cannot complete this approval on the user's behalf.

## Commands

### Start research

User: Open the project repository as your workspace, start a fresh chat, and type `start research`.

### Save transcript

Agent: After the user approves the selected features, remind them that the transcript is a deliverable and ask them to say `save transcript`. Wait for that direction.

When the user directs the agent to save the transcript, the agent saves the entire conversation in the `transcripts/` directory as `research-YYYY-MM-DD_HHMMSS.md`, marks user and agent responses clearly, and confirms the saved relative path.
