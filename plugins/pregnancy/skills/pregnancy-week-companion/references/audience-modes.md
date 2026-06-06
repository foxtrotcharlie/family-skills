# Audience modes

The weekly companion has three output modes. Pick one per generation. The
user specifies which (or asks for "for me", "for partner", "for both" /
"combined" in their request); when ambiguous, ask once at the start.

The factual content (development milestones, symptom mechanisms,
evidence-based recommendations, red flags) is the same across modes. What
changes is **voice, emphasis, and depth allocation** — not the science.

## combined (default)

The original format — six sections, partner-facing voice. A single shared
document a couple can read together. The pregnant person reads "Mum's body"
as third-person about herself; the partner reads it as info about her.
"For your partner" and "What to say to her" are explicitly partner-only.

- **Filename:** `Week-XX.md`
- **Section structure:** as in `template.md`
- **Voice:** partner-facing — "she", "for your partner", "what to say to her"
- **Audience cue:** none (assumed default)

## partner

Same six sections, rebalanced for a partner reading on their own.
"Mum's body" is brief — what the partner needs to recognise, not the full
physiology breakdown. "For your partner" and "What to say to her" get the
deepest treatment of the three modes.

- **Filename:** `Week-XX-partner.md`
- **Section depth changes:**
  - **Mum's body** — brief view ≤ 3 bullets, detailed callout ≤ 100 words.
    The full physiology lives in the combined version.
  - **For your partner** — detailed callout 200-400 words.
  - **What to say to her** — unchanged from combined.
  - Other sections unchanged.
- **Voice:** "she", "for you", "what to say to her" — partner is the reader
  throughout.
- **Audience cue at top:** italicised line under the header:
  *For the partner. Combined version: [[Week-XX]].*

## pregnant

Speaks directly to the pregnant person in second person. Sections are
restructured:

1. **Your baby this week** — same content as Baby this week. Voice: "your
   baby".
2. **Your body this week** — what was Mum's body, but second-person ("you
   may be experiencing"). The detailed breakdown is the deepest of the
   three modes — this is the section the pregnant person most wants depth
   on (mechanism, what helps, when to expect change).
3. **What you might be feeling** — what was "What to say to her", reframed
   for the pregnant person reading about her own emotional landscape.
   Replace the suggested-phrases list with "things that often help to hear"
   or "things you can ask your partner for".
4. **How your partner can help this week** — compressed version of "For
   your partner". Brief list, framed as "things to ask of your partner".
   Useful to share with him or read together.
5. **Your week's to-dos** — same content, second-person voice.
6. **Red flags — when to call your doctor** — same content, second-person.

- **Filename:** `Week-XX-pregnant.md`
- **Voice:** "you", "your baby", "your partner"
- **Audience cue at top:** italicised line under the header:
  *For you. Partner version: [[Week-XX-partner]]. Combined: [[Week-XX]].*

## Cross-mode wikilinks

Each mode's footer wikilinks to the previous and next files **of the same
mode**:
- combined: `*Previous: [[Week-XX]] · Next: [[Week-XX]]*`
- partner: `*Previous: [[Week-XX-partner]] · Next: [[Week-XX-partner]]*`
- pregnant: `*Previous: [[Week-XX-pregnant]] · Next: [[Week-XX-pregnant]]*`

If the user has only generated one mode for a given week, dangling
wikilinks are fine — Obsidian tolerates them.

## When to ask vs. infer

Infer the mode from the user's wording when it's clear:

- "update for me at 22 weeks" → `pregnant`
- "weekly update for my partner" / "update for the partner" → `partner`
- "this week's update" / "Week 22" with no audience cue → ask, or fall
  back to `combined` if the user has previously expressed a preference.

If they regularly want all three, they can say so and you produce three
files in sequence — the factual content is shared so this is mostly voice
and re-emphasis work, not three independent generations.

## What stays consistent across modes

- The medical sources, decision tree, and "options not prescriptions" rule
  apply to every mode.
- Red flags are identical — the warning signs for the trimester don't
  change with audience.
- The brain / nervous system bullet in the "What's forming" detailed
  breakdown of `Baby this week` is mandatory in all three modes.
- Tone-guide rules apply uniformly: warm but not saccharine, direct on hard
  things, mechanism in detail, options not prescriptions.
