---
name: pregnancy-week-companion
description: "Generate a weekly pregnancy update markdown file for a personal Obsidian vault. Use this skill whenever the user asks for 'this week's update', 'week X update', 'pregnancy update for week N', or any variation referring to weekly pregnancy info, fetal development, or guidance for an expecting couple. Trigger this even if the user just says 'let's do this week' in the context of a pregnancy. Produces a single markdown file with six sections — baby development, mum's body, partner support, what to say to her, to-dos, red flags — each with brief highlights and a collapsible detailed breakdown. Three audience modes are supported: combined (default, partner-facing, shared document), partner (rebalanced for the partner reading alone), and pregnant (second-person, addressed to the pregnant person) — pick one per generation from the user's wording or by asking. Synthesises NHS, Mayo Clinic, Cleveland Clinic, ACOG/RCOG, peer-reviewed maternal physiology research, Evidence Based Birth, and the couple's own curated research briefs on diet, exercise, and evidence-based birth interventions — explaining not just what is happening but why, with recurring emphasis on fetal brain and neurological development (neurogenesis, neuronal migration, synaptogenesis, myelination). Presents options, not prescriptions."
---

# Pregnancy Week Companion

Generate one week's pregnancy update at a time as a markdown file ready to drop into an Obsidian vault. The skill captures the format, tone, and section structure agreed with the user so each week feels consistent.

## When to use

Trigger this skill when the user asks for an update for a specific pregnancy week, or asks for "this week's" update in a pregnancy context. If the week number isn't specified, ask before generating.

Do NOT use this skill for:
- General pregnancy questions that aren't structured weekly updates ("is X safe in pregnancy?")
- Generating many weeks at once — this skill is intentionally one-week-at-a-time so the user reads things when they're timely

## Audience mode

The skill produces one of three modes per run. **Pick the mode before drafting** — it changes voice, depth allocation, filename, and (for `pregnant`) the section structure.

- **`combined`** (default) — partner-facing voice, six sections per `references/template.md`. Filename: `Week-XX.md`.
- **`partner`** — six sections, but `Mum's body` is compressed and `For your partner` / `What to say to her` get the deepest treatment. Filename: `Week-XX-partner.md`.
- **`pregnant`** — second-person, addressed to the pregnant person. Sections are restructured (`Your body this week`, `What you might be feeling`, `How your partner can help this week`). Filename: `Week-XX-pregnant.md`.

**How to pick:**

- Infer from wording: "update for me" → `pregnant`; "for my partner" / "for the partner" → `partner`; "for both of us" / "combined" → `combined`.
- If the user says "all three", produce three files in sequence.
- If ambiguous, ask once at the start: "Should I generate this for you, your partner, or a combined version?"

See `references/audience-modes.md` for the full spec on what differs between modes.

## Output

A single markdown file. Filename depends on the audience mode:

- `combined`: `Week-XX.md` (zero-padded, e.g. `Week-08.md`)
- `partner`: `Week-XX-partner.md`
- `pregnant`: `Week-XX-pregnant.md`

**Where to write the file depends on the environment.** Before applying the per-environment defaults below, check whether the user has documented a preferred output path in their CLAUDE.md or elsewhere in the loaded context — if so, use it and don't ask.

- **In Claude Code CLI** (running on the user's machine): write to the current working directory by default. The simplest workflow is to launch Claude Code from inside the Obsidian `Weekly/` folder so the file lands where wikilinks resolve. If the cwd doesn't look like a Pregnancy / Weekly folder, ask once at the start: "Where should I write the weekly file? (default: current dir)" and remember the answer for the rest of the session. Do not silently write to a path the user hasn't seen — the file is the deliverable.
- **In Cowork** (desktop app, folder access granted): write the file directly into the granted workspace folder. The user will typically have granted access to either a `Weekly/` folder or its parent — write into `Weekly/` if visible, otherwise into the granted folder root. No `present_files` call is needed in Cowork; the file appearing in the folder is the deliverable. Confirm the path in your closing message.
- **In claude.ai chat** (web/mobile, sandboxed): write to `/mnt/user-data/outputs/Week-XX[-mode].md` and present via `present_files`. The user is responsible for relocating the file to wherever their pregnancy notes live (typically an Obsidian vault's `Weekly/` folder). If they haven't told you their destination, ask once. When presenting, surface the destination path in your closing message so they know where to drop it for the wikilinks to resolve.

**Detecting environment:**
1. If `/mnt/user-data/outputs/` exists and is writeable → claude.ai chat.
2. Else if a workspace folder named `Pregnancy` or `Weekly` is visible in the granted access → Cowork.
3. Otherwise → Claude Code CLI (write to cwd).

When in doubt, ask the user once at the start of the run.

**Either way:** the wikilinks in the file footer (`[[Week-XX]]` etc.) assume sibling files live in the same folder, so the file must end up in `Weekly/` for navigation to work.

## Section structure (mandatory — keep in this order)

Every weekly file MUST contain these six sections in this order, each with a brief highlights view followed by a collapsible Obsidian callout for detail:

1. **Baby this week** — fetal development
2. **Mum's body** — symptoms and physical changes
3. **For your partner** — practical support actions
4. **What to say to her** — partner mental health support, with suggested phrases
5. **This week's to-dos** — appointments, decisions, lifestyle reminders (use `- [ ]` checkboxes)
6. **Red flags — call your doctor** — specific to this week, one short paragraph

Plus header (size comparison, trimester, weeks to go) and footer (wikilinks to previous/next week).

## Format details

- **Filename:** `Week-XX.md` with zero-padded week number
- **Header line:** `**Size:** [Fruit/object] (~X cm) · **Trimester:** [First/Second/Third] · **Weeks to go:** ~XX`
- **Brief view per section:** 3–5 bullet points OR a 2–3 sentence paragraph (use whichever is in the template for that section — see references/template.md)
- **Detailed view:** Inside `> [!info]- Detailed breakdown` callout (the trailing `-` makes it collapsed by default in Obsidian)
- **Wikilinks at footer:** `*Previous: [[Week-XX]] · Next: [[Week-XX]]*` (omit "Previous" for week 4, omit "Next" for week 40)

For the exact template structure with section ordering, see `references/template.md`.

## Tone and voice

The voice is established and consistent. See `references/tone-guide.md` for full guidance, but key principles:

- Warm but not saccharine. Talks to the couple like a knowledgeable friend, not a medical pamphlet
- Direct about hard things (miscarriage anxiety, antenatal depression, partner mental health)
- Practical — every "for your partner" item should be something they can actually do today
- British/international phrasing acceptable (uses "mum" not "mom", metric units primary)
- Avoids: gendered assumptions beyond "her/she" for the pregnant person and "he/him" for the partner (this is the user's preference — adjust if user signals otherwise), toxic positivity, comparison ("at least it's not as bad as...")

## "What to say to her" section — special guidance

This section was added specifically by the user and matters to them. See `references/mental-health/mental-health-section.md` for the format. Key requirements:

- 4–5 suggested phrases the partner can borrow, in quotes
- Brief framing sentence above the phrases describing what she's likely feeling that week
- Detailed breakdown includes: why this week is emotionally specific, what tends to land well, what to avoid, mental health warning signs to watch for, and a brief reminder that the partner's mental health matters too

## "For your partner" section — the reading list

The detailed breakdown of *For your partner* often mentions "the partner reading list" — a curated set of topics the partner reads through over the course of the pregnancy. **The list is canonically defined in `references/partner-reading-list.md`.** Don't refer to it abstractly without grounding it; either:

- Name 1–3 specific items from the list when introducing or pointing toward something this week ("intervention cascade and cord clamping are good first reads"), or
- Reference it generically *only after* the list has been introduced in a prior week ("keep momentum on the reading list" assumes the user has already seen specific items).

The reading list is the spine for partner-prep content the same way `nutrition.md` is the spine for nutrition. Pull from it; don't invent ad-hoc topics.

## Source layers

The weekly file synthesises multiple evidence-based sources. No single source is the spine — the goal is breadth, accuracy, and giving the couple options rather than instructions.

Each domain bundle (`references/diet/`, `references/exercise/`, `references/birth/`) has its own `provenance.md` that documents the methodology, sources consulted, verification rounds, and draft chain that produced the evidence brief in that folder. You don't need to read provenance during normal generation — they exist for transparency and for anyone auditing the underlying research. Every weekly file's footer points to the public repo where these provenance files live (see `references/template.md`).

### Primary: medical sources

`references/medical-sources.md` is the central reference. Read it before drafting any week. It covers:

- **Fetal development** — NHS, Mayo Clinic, Cleveland Clinic, StatPearls (peer-reviewed embryology)
- **Maternal physiology** — peer-reviewed papers explaining the *why* behind symptoms (Soma-Pillay 2016, Jee & Sawal 2024, Chiang 2025). The user explicitly wants the science, not folk wisdom.
- **Birth and intervention decisions** — Evidence Based Birth, Cochrane reviews, ACOG, RCOG, NICE
- **Perinatal mental health** — NHS, Maternal Mental Health Alliance
- **South African / local context** — Soma-Pillay paper is locally authored; SA Maternity Care Guidelines for public-system context

The medical-sources file also has a **decision tree** at the bottom: for each kind of claim (size, milestone, symptom mechanism, decision, mental health, red flag), it tells you which sources to anchor to. Use it.

### Secondary: Vittorio protocol

`references/vittorio-protocol.md` is **one perspective among several**, not the document's spine. It came from a specific book the user read; its timing anchors and pragmatic suggestions are useful, but its prescriptive voice and contested choices (declining standard newborn interventions, etc.) are not the default frame.

Treat Vittorio as you would a knowledgeable but opinionated friend's input:

- **Useful for**: pragmatic week-anchored timings (e.g., "perineal massage tends to start around 34 weeks"), partner-action ideas (e.g., the morning protein-and-fat tray for nausea — this has reasonable physiological grounding around blood sugar and hCG), and the *menu* of decisions a couple faces.
- **Not useful as**: the only source, the source of truth on contested choices, or a prescriptive checklist.
- **When Vittorio and Tier 1 sources conflict**, Tier 1 wins, and the weekly file presents the mainstream evidence. The user-curated research briefs (`diet.md`, `exercise.md`, `interventions-vaginal-birth.md` and their practical/ELI5 companions) also outrank Vittorio — e.g. on raspberry leaf tea or dates, use the interventions review's evidence grading, not Vittorio's stance.
- **For contested choices** (Hep B at birth, vitamin K route, eye ointment, induction timing, cord clamping), present them as decisions to discuss with the care provider, with the evidence on both sides briefly noted. Never frame Vittorio's stance as the recommended one.

### Tone across all sources

- **Translate, don't transplant.** Source material — whether a Cochrane review, an ACOG guideline, or Vittorio — comes in clinical or declarative voice. The weekly file's voice is warm-friend (see `tone-guide.md`).
- **Present options, not prescriptions.** "Here's what tends to happen and what couples often do" — not "do this".
- **Don't moralise.** Mention lifestyle restrictions/choices once, factually.
- **Mechanism in detail, not in brief.** The brief view stays surface-level; the detailed breakdown is where the physiological "why" lives. This protects the page from feeling like a textbook while still respecting the user's preference for science-based content.

### Recurring focus: nutrition

`references/diet/nutrition.md` is the spine for any nutrition content in a weekly file — what to eat, vitamins, minerals, supplements, what to avoid, and the postpartum impact of inadequate diet. It covers the core nutrients with mechanisms (folate, choline, iron, iodine, vitamin D, DHA, calcium, magnesium, B12, zinc, protein, fibre), the evidence-supported supplement spine, what to avoid (with reasons, mentioned factually once not moralised), the first-trimester nausea protocol, trimester-by-trimester nutrient priorities, and a postpartum / 4th-trimester preview including what under-eating or poor diet actually costs.

**Important: there is no standalone "nutrition" section in the template.** Nutrition surfaces *through* the existing six sections when something is week-relevant:

- `Baby this week` detailed breakdown — when a specific nutrient ties to what's developing (folate at 4–6w neural tube, choline through neurogenesis weeks 8–20+, iodine ~14–16w fetal thyroid, DHA through brain growth phases, iron building fetal stores in the third trimester)
- `Mum's body` detailed breakdown — when a symptom has a food/nutrient lever (nausea, constipation, leg cramps, heartburn, fatigue, restless legs)
- `For your partner` — practical food prep (morning tray in first trimester, freezer-stocking in late third, snack stations)
- `This week's to-dos` — supplement check at booking, ferritin retest, GTT, dates from ~36w
- `Red flags` — when an eating-related sign warrants medical attention (hyperemesis especially)

The nutrition file has a "How nutrition surfaces in each section" map at the bottom — use it to keep nutrition content well-placed and proportionate, not crowding the page.

**Companion diet references (user-curated research):** `references/diet/diet.md` is the full evidence brief (ACOG/WHO/NHS/NIH guidelines, nutrient RDA tables with deficiency risks, caloric needs by trimester, food-safety lists with the *reason* for each restriction, caffeine dose-response data, prenatal supplement label guidance including the choline/iodine/DHA gaps, Mediterranean diet evidence). `references/diet/diet-practical-guide.md` is its plain-English action version with trimester-by-trimester action steps, the nausea protocol, heartburn strategies, vegetarian/vegan fixes, and the reassuring "don't worry" framings. Use these alongside `nutrition.md`: nutrition.md remains the spine for *where* nutrition surfaces in the six sections; the diet pair is the cross-check for specific numbers (calorie additions, RDAs, caffeine limits) and the source of practical framings ("you're eating for 1.15, not two"). When the documents differ on a specific dose, prefer diet.md's guideline-sourced figures and keep mechanisms from nutrition.md.

### Recurring focus: exercise

`references/exercise/exercise.md` (full evidence brief) and `references/exercise/exercise-practical-guide.md` (plain-English version with a trimester-by-trimester and week-by-week guide) are the spine for any movement content in a weekly file. They are user-curated research synthesising ACOG 804, WHO 2020, the Canadian CSEP guideline, and multiple meta-analyses. Key facts to keep consistent across weeks: the 150 min/week moderate-activity target and "talk test"; benefits (GDM risk down ~a third, cesarean odds down ~30%, meaningful antenatal/postnatal depression reduction — walking especially); the safety reassurance (no increased miscarriage/preterm/birth-weight risk in uncomplicated pregnancies); the supine-exercise cutoff at ~16–20 weeks; what to avoid (contact sports, fall-risk sports, scuba, hot yoga); the warning-signs-to-stop list; and the absolute/relative contraindications (always deferred to the care provider).

Like nutrition, **there is no standalone exercise section** — exercise surfaces through the six sections when week-relevant:

- `Mum's body` detailed breakdown — when a symptom has a movement lever (back/pelvic girdle pain, constipation, mood, sleep) or when a trimester transition changes what's comfortable or safe (supine cutoff ~16–20w, balance shift in the third trimester)
- `For your partner` — joining the walk or swim, taking over a task so she has time to move, booking the prenatal yoga or aquanatal class
- `This week's to-dos` — trimester-appropriate activity reminders, starting pelvic floor work (~12–20w), transitioning off supine exercises (~17–20w), adapting pace in the third trimester
- `Red flags` — the stop-exercising-and-call list when an exercise-related symptom appears (bleeding, fluid leak, regular painful contractions, calf pain/swelling, dizziness, chest pain)

The practical guide has week-by-week suggestions per trimester — use those to anchor exercise content to the specific week, and keep it proportionate (a bullet or two, not a programme).

### Recurring focus: birth preparation and interventions

`references/birth/interventions-vaginal-birth.md` is the spine for any birth-preparation content — a GRADE-graded evidence review of 18 physical interventions against measurable birth outcomes (perineal trauma, labour duration, mode of delivery, spontaneous onset, pelvic floor outcomes, pain/epidural use), with a tiered one-page summary at the bottom. `references/birth/interventions-vaginal-birth-eli5.md` is its plain-language companion — useful for borrowing warm-friend framings ("like stretching a new leather glove"). These are user-curated research and take precedence over Vittorio for birth-prep timing and evidence claims.

How to use the tiers:

- **Tier 1 (strongly supported)** — aerobic exercise throughout, perineal massage from 34w, warm compresses in second stage, upright positioning/mobility in labour, pelvic floor muscle training from 12–20w, gestational weight management. These can be presented confidently, with their week anchors, still as options ("many couples start...") not prescriptions.
- **Tier 2 (promising, lower-quality evidence)** — dates from ~36w, prenatal yoga, birth ball in labour, water immersion in first stage. Present as "reasonable to try, evidence is weaker"; note the caveat briefly (e.g. the best dates RCT found no benefit).
- **Tier 3/4 (insufficient evidence or possibly harmful)** — raspberry leaf tea, Spinning Babies, EPI-NO devices, evening primrose oil. Don't recommend these. If one is culturally ubiquitous for the week (raspberry leaf in late third trimester), it's fine to name it once as not evidence-supported — factually, without moralising. EPI-NO deserves an explicit caution if devices come up (possible increased severe-tear risk).

Week anchors for surfacing birth-prep content: pelvic floor training from ~12–20w (`to-dos`), perineal massage starting ~34w (`to-dos` + `For your partner` — the protocol details live in the full review), dates from ~36w (`to-dos`, with the GDM caution), labour positioning/warm compresses/water immersion in the birth-plan weeks (~32–36w, as things to discuss with the midwife and put in the birth plan). The integrated tear-prevention strategy section is the best single anchor for a late-third-trimester birth-prep summary.

### Recurring focus: perinatal mental health

`references/mental-health/perinatal-emotional-journey.md` is the spine for any mental-health content in a weekly file — a research-anchored evidence review covering normal experience (matrescence, ambivalence, baby blues, intrusive thoughts), clinical conditions (perinatal depression, anxiety, OCD, PTSD, tokophobia, postpartum psychosis), the partner's trajectory, and what works (CBT/IPT, exercise, sleep protection, peer support, sertraline as default). `references/mental-health/perinatal-emotional-journey-eli5.md` is its plain-language companion — useful for borrowing warm-friend framings (e.g. "having a baby rewires the brains, bodies, and relationships of *both* parents"). `references/mental-health/mental-health-section.md` is the structural / format guide for the *What to say to her* section's detailed breakdown.

Key numbers and framings to use consistently across weeks (anchored, no inline citations):

- **Perinatal depression: ~12–18% pooled prevalence** (sometimes "1 in 6"). Antenatal depression alone ~10–15%. The strongest predictor of postnatal depression is depression *during* pregnancy.
- **Perinatal anxiety: ~15% antenatal, ~15% postnatal**, often missed by depression-focused screens.
- **Partner perinatal depression: ~8–10% pooled, peaking 3–6 months postpartum**, correlated with maternal depression. An independent predictor of child outcomes — partner mental health is a clinical population, not just support.
- **Baby blues: 50–80%, days 3–5, resolves spontaneously by ~2 weeks.** The two-week rule is the dividing line between blues and PND.
- **Postpartum psychosis: 1–2 per 1,000.** Onset typically within 2 weeks postpartum. Severe insomnia + confusion + believed delusions = emergency. Largest single risk factor: bipolar disorder.
- **Maternal brain remodeling (Hoekzema 2017):** measurable selective gray-matter changes in social-cognition regions, lasting at least 2 years, predict bonding strength. Useful for "you're not crazy, your brain is literally rewiring" reassurance — biology, not folk wisdom.
- **Partner support is the strongest modifiable protective factor** for maternal perinatal mental health (Pilkington 2015). The partner role is clinically significant.
- **The intrusive-thoughts-vs-psychosis distinction:** distressing intrusive thoughts of harm to the baby occur in the *majority* of new mothers and are *not* predictive of harm — distress about the thought is reassuring. The opposite (believed, congruent, often with insomnia and confusion) is the emergency. This is the single most important public-health message in the brief and should be clearly framed in any week that surfaces postpartum-period content.
- **"No treatment" is not the safe default.** Untreated antenatal depression has its own real risks (preterm birth, low birth weight, child outcomes, maternal suicide). Sertraline is generally first-line in pregnancy and lactation when medication is indicated. The weekly file does not advise on medication directly — it can name that this is a both-sides decision and direct to specialist perinatal mental health care.

Like nutrition and exercise, **there is no standalone mental-health section in the template** — mental health surfaces through the existing six sections when week-relevant:

- `Mum's body` detailed breakdown — when the maternal experience has a documented neurobiological or hormonal driver (matrescence brain remodeling, the hormone cliff caveat, sleep-mood interaction). Use anchored numbers, not folk language.
- `What to say to her` detailed breakdown — this is where most mental-health content lives. The *Why this matters now* opener, *What tends to land well*, *What to avoid*, *Watch for warning signs*, and *Look after yourself too* subsections all pull from the brief. See `mental-health/mental-health-section.md` for the format.
- `For your partner` — partner emotional support as the strongest modifiable protective factor (Pilkington 2015), framed practically. Partner perinatal mental health awareness in the *Mental load* and *Look after yourself too* notes.
- `This week's to-dos` — booking-appointment EPDS / GAD-7 mention, partner reading list items on mental health (see `partner-reading-list.md`), peer-support / class enrolments.
- `Red flags` — postpartum-period weeks need the postpartum psychosis red flags (severe insomnia + confusion + delusions in first 2 weeks); any week needs suicidal ideation as a red flag.

**Crisis resources** to surface where appropriate (don't bloat — name once when relevant, e.g. in the *Look after yourself too* note or in the red-flags paragraph for the relevant weeks):

- **UK:** NHS 111 (option 2 for mental health), Samaritans 116 123, PANDAS Foundation 0808 1961 776, Action on Postpartum Psychosis (app-network.org)
- **US:** 988 (Suicide & Crisis Lifeline), Postpartum Support International 1-800-944-4773, National Maternal Mental Health Hotline 1-833-852-6262
- **Always:** emergency department / 999 / 911 for imminent danger.

The user has asked for extra emphasis on brain and neurological development as a recurring theme. Every weekly file should include a dedicated **Brain / nervous system** bullet in the "What's forming" detailed breakdown of `Baby this week`, drawing on the brain-development timeline in `medical-sources.md`. Use specific, accurate language — neurulation, neurogenesis, neuronal migration, synaptogenesis, myelination — and explain what each one means in plain words. Let the awe-inspiring numbers (e.g. 250,000 neurons per minute at peak) come through where they fit, without tipping into saccharine. When senses come online (hearing ~18 weeks LMP), point partners toward the practical implication ("she can hear you now").

## Workflow

1. Confirm the week number with the user if not given
2. Pick the audience mode (`combined` / `partner` / `pregnant`) — infer from wording or ask. See `references/audience-modes.md` for what differs between modes.
3. **Check what's already known in this pregnancy.** Before drafting, scan the loaded context (CLAUDE.md, prior weekly files in the same folder, anything the user has volunteered) for couple-specific context. If essentials aren't on the table, ask once: *"Anything I should know before drafting? E.g. NIPT done? Sex known? Anatomy scan booked or done? GTT done? Any complications, medications, or specific decisions I should reflect?"* Adapt the draft to what's known: don't tease decisions already made, don't suggest tests already done, and don't frame the sex-reveal as upcoming if they already know. The user shouldn't have to re-correct context every week — once volunteered, it's part of their CLAUDE.md or is implicit from prior weekly files.
4. Read `references/medical-sources.md` to get oriented on which sources apply to which kinds of claims (use the decision tree at the bottom)
5. Read `references/template.md`, `references/mental-health/mental-health-section.md`, `references/tone-guide.md`, `references/audience-modes.md`, and `references/partner-reading-list.md`
6. Skim `references/diet/nutrition.md` to identify which nutrients, food levers, or supplements are week-relevant — and where they belong across the six sections (see the map at the bottom of that file). Cross-check specific numbers (calorie additions, RDAs, caffeine limits, food-safety items) against `references/diet/diet.md`, and borrow practical framings from `references/diet/diet-practical-guide.md`
7. Skim `references/exercise/exercise-practical-guide.md` for the trimester- and week-relevant movement guidance (target, modifications, what changes this week); pull evidence detail or contraindications from `references/exercise/exercise.md` when needed
8. Check `references/birth/interventions-vaginal-birth.md` for any birth-prep intervention activating this week (PFMT ~12–20w, perineal massage ~34w, dates ~36w, birth-plan items ~32–36w) — use the tiered summary at the bottom, and `references/birth/interventions-vaginal-birth-eli5.md` for plain-language framings
9. Skim `references/mental-health/perinatal-emotional-journey.md` for prevalence, mechanism, and intervention claims relevant to this week's mental-health content. The `-eli5.md` version is a good first read for warm-friend framings. Cross-anchor any mental-health claim against the brief's anchored numbers (depression ~12–18%, partner depression ~8–10% peaking 3–6 months postpartum, etc.) rather than folk numbers.
10. Skim `references/vittorio-protocol.md` for week-anchored timings and pragmatic ideas — but treat it as one input, not the source of truth
11. Skim `references/example-week-08.md` to calibrate tone and structure — this is the user-approved reference
12. For factual claims about this specific week:
    - Cross-check fetal development across at least two Tier 1 sources (NHS, Mayo, Cleveland, StatPearls)
    - For maternal symptoms, source the *mechanism* from the peer-reviewed physiology papers (Soma-Pillay, Jee & Sawal, Chiang) and put it in the detailed breakdown
    - For nutrition mechanisms and the nutrient–development link, draw on `references/diet/nutrition.md` and cross-check specific dose claims against `references/diet/diet.md` and NHS/NICE/ACOG
    - For exercise claims, anchor to `references/exercise/exercise.md` (guideline-sourced: ACOG 804, WHO 2020, CSEP 2019)
    - For decisions/interventions activating this week, draw on `references/birth/interventions-vaginal-birth.md` (evidence tiers) and Evidence Based Birth, and present options, not prescriptions
13. Generate the file (in `/home/claude/` first if in claude.ai sandbox; directly in working location if in Cowork or CLI)
14. Place the final file in the right destination for the environment (see Output section above)
15. In claude.ai: present with `present_files` and note the destination folder. In Cowork or CLI: confirm the path in your closing message.
16. Briefly note (1–2 sentences) anything notable about this week — major milestone, scan window, decision newly active this week, etc.

## What to avoid

- Do not generate multiple weeks in one invocation
- Do not give medical advice beyond "call your doctor if X" — always defer to their care provider for diagnosis or treatment
- Do not assume circumstances (single parent, twins, IVF, complications) unless user has told you — keep language inclusive but don't over-customise without input
- Do not be preachy about lifestyle (no alcohol, no soft cheese etc.) — mention once in to-dos, don't moralise
- Do not import any single source's voice or stance wholesale — synthesise across sources, translate into the warm-friend register
- Do not present contested choices (declined Hep B at birth, declined eye ointment, induction timing, vitamin K route, etc.) as recommended one way or the other — frame them as conscious decisions for the couple to make with their care provider, briefly noting the evidence on both sides
- Do not include folk wisdom or "old wives' tales" that aren't supported by current evidence (gender by bump shape, heartburn = hairy baby, raspberry leaf tea claims, "eating for two", etc.). The user explicitly wants science-based content.
- Do not cite sources by name inline in the body of the file — the warm-friend voice doesn't have footnotes. Sources inform your reasoning; they don't appear in the body. The static "Sources" footer line (see `references/template.md`) is the single exception, and it's identical across all weeks.
