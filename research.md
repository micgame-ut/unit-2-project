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

**Open question, not yet assumed either way:** whether device/platform (phone vs. laptop vs. TV vs. desktop PC) meaningfully changes the resource-use estimate, since Alex and Jordan both use multiple devices.

## Possible features

Generate several possibilities before choosing. Keep the initial notes brief. For each idea, record:

- what it would help someone learn or do
- the profiles or needs it would serve
- any evidence or implementation challenge that might affect it

- **"Beyond the prompt" — image/video generation resource use.** Would let someone estimate the footprint of image- and video-generation tool use, not just text prompts. Serves Alex directly (named need, water-use concern) and touches Jordan (uses generative tools beyond chat). Energy figures are now well-supported for both image generation (Luccioni et al. 2024) and video generation (Jegham, Gamazaychikov & Luccioni 2026) — video costs roughly 30x an image and ~1,600-2,000x a text prompt, scaling quadratically with resolution/duration. Water use is the remaining weak point: no verified per-image or per-video water figure exists yet (see Source assessments), so that part of the feature needs a more visible disclaimer than the energy part, or should be deferred until a WUE figure is verified.

- **"Everyday digital habits" — video calls, streaming, and gaming footprint comparisons.** The current calculator has zero coverage of these (confirmed by inspecting the code) despite all three profiles naming them: Alex streams reference media and uses social platforms on phone/laptop/TV; Jordan games on a desktop PC and has frequent video calls; Robin spends substantial time in video meetings, streaming, and social platforms. This directly serves the brief's requirement that at least two features connect AI use to broader digital life. Evidence found is solid for streaming and gaming (peer-reviewed/institutional), and preliminary-but-usable for video calls (small pilot study). Bonus: the streaming figure has the same "corrected-viral-myth" story as the AI water-use figure, which is a good fit for Robin's skepticism (showing sources get corrected, not just cited). Social-media browsing itself remains unsourced — likely low-footprint relative to video, but not yet verified, so left out for now rather than guessed.

- **"Project totals" — irregular coding-agent sessions over a project's real timespan, with an honest range.** Instead of forcing agent sessions into the tool's per-day cadence, let someone enter a handful of sessions (or a rough session count) across a project and see a total, presented as a range rather than a single number — reflecting how much real agentic token use actually varies. Serves Jordan directly (named need: "prompts per day" too simplistic, wants assumptions/ranges/project totals). Challenge: the best evidence found says token use for the *same* task typically varies ~2x run-to-run (occasionally much more), and difficulty ratings barely predict cost — so a single flat number would misrepresent it, and the feature needs to communicate a range rather than false precision.

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
  - **Claim/figure:** ~36 g CO2/hour of streaming on the global average grid (18 g per 30-min show); varies hugely by device (100x difference between a smartphone and a 50" TV) and by grid (France ~2 g CO2/hour on its low-carbon grid vs. ~36 g global average). Resolution matters too: SD ≈0.7 GB/hr, HD ≈3 GB/hr, 4K ≈7 GB/hr.
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
  - **Claim/figure:** Tested 26 gaming systems. Nameplate power for low/typical/high-efficiency complete systems ≈300/600/900 W, but *measured* power draw runs about 50% of nameplate for most systems (so roughly 150–450 W actual, in line with commonly-cited "250–400 W" figures). GPU power alone ranges 60–500 W. A typical gaming PC uses ~1,400 kWh/year — about 6x a typical (non-gaming) PC and ~10x a gaming console.
  - **Evidence checked directly:** Partially — the paper itself is paywalled (Springer login required); figures confirmed via Berkeley Lab's own news coverage of their study (eta.lbl.gov) and a research-summary aggregator, cross-checked against each other, not the original full text.
  - **Limitations/uncertainty:** Study is from 2019 — hardware efficiency has likely shifted since. Wide system-to-system variation (5–1,200 kWh/year across configurations) means any single "typical" figure hides a lot of spread.
  - **Confidence and decision:** **Use with qualification** — peer-reviewed and LBNL-affiliated (same rigor tier as the original calculator's other sources), but since I couldn't read the full paywalled text directly, treat the specific numbers as provisional until independently re-confirmed, and keep the range wide rather than picking one "typical gaming hour" number.

- **Social media browsing:** Not yet researched. Flagged as an open item, not assumed to be negligible or significant either way.

## Selected features

List the five selected features. Briefly explain why each was selected and how the set serves all three reference profiles. Name a few serious alternatives and explain why they were rejected.

User approval: Review the completed research directly. Confirm that sources exist and support the claims the project will use, correct the document as needed, and explicitly approve the selected features before developing the specification. The agent cannot complete this approval on the user's behalf.

## Commands

### Start research

User: Open the project repository as your workspace, start a fresh chat, and type `start research`.

### Save transcript

Agent: After the user approves the selected features, remind them that the transcript is a deliverable and ask them to say `save transcript`. Wait for that direction.

When the user directs the agent to save the transcript, the agent saves the entire conversation in the `transcripts/` directory as `research-YYYY-MM-DD_HHMMSS.md`, marks user and agent responses clearly, and confirms the saved relative path.
