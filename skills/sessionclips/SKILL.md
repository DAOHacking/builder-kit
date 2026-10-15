---
name: sessionclips
description: Turn the current AI session into draft posts. Use when the user says "/post", "/clip", "draft a post", "make a post from this", "post this", "tweet this", or similar. Extracts noteworthy moments from the session (lessons, ships, hot takes, receipts, funny moments, observations, educational bits, human-in-the-loop story moments) and drafts 3-5 variants in the user's voice, 240-400 chars, ready to copy-paste. Always includes at least one first-person "human moment" variant.
---

# sessionclips

This skill turns the current session into draft social posts in the user's voice. The goal is content for other builders / prompt engineers / claude users based on real sessions, not generic AI commentary.

## When to trigger

User says any of: `/post`, `/clip`, "draft a post", "post this", "tweet this", "make a post from this session", "what would i post about this", "turn this into a tweet."

Do NOT trigger on the user just saying "post" in passing (e.g. "i'll post this later" is not a request to draft).

## Hard refusals (do not draft if session contains)

- API keys, tokens, passwords, `.env` contents, JWT secrets, private keys
- Specific client work or paid contract details
- Security vulnerabilities that haven't been patched yet (the post would be a roadmap for attackers)
- Pre-launch token contract addresses, mint addresses, or unannounced launch dates
- Anything the user marked "don't post" / "private" / "off the record" earlier in the session
- Identifying details about other people without consent (real names, DMs, private convos)

If any of these are present, refuse with one line explaining which category. Do not draft a "scrubbed" version unless the user explicitly asks after the refusal.

## Extraction phase

Before drafting, scan the session and pull out post-worthy moments. Categories to look for (a session may have several or none):

1. **Lesson**: something that turned out different from what was expected. "thought X, actually Y."
2. **Ship**: something concrete that got built/deployed/fixed in this session.
3. **Hot take**: an opinion the user expressed or that's implicit in their workflow choices.
4. **Receipt**: claude did something measurable (caught a bug, saved time, found an issue) with concrete proof in the session.
5. **Funny / casual**: bug that was absurd, a moment of frustration, a sarcastic aside, an unhinged take.
6. **Observation**: pattern noticed about claude, about prompting, about the workflow.
7. **Process**: a specific way the user worked that others could steal.
8. **Educate**: a specific concept, fix, pattern, or workflow that other claude users / builders would benefit from knowing. teach one thing concretely. not "tips for prompting better" but "here's the exact pattern that fixed X."
9. **Human in the loop**: the user's actual experience told in first person. "i was doing X, then this happened, then i tried Y." uses "i," "me," "my." includes specific moments of frustration, surprise, or decision-making. tells a small story instead of stating facts. the post should feel like the user texted a friend about their day, not like marketing copy.

If extraction finds nothing post-worthy, say so plainly. Do not invent. "this session was mostly debugging the same thing for an hour, nothing really stands out" is a valid response.

## The fluidity rule

Across all angles, drafts must read like a human typed them in real time, not like a model generated them. Symptoms of "model-generated feel" to actively avoid:

- **stating facts about the thing** instead of telling what the user did with it. "8 angle types, hardcoded refusal list" is product copy. "i kept catching myself trying to draft tweets manually before i remembered i built a thing for this" is human.
- **no first-person presence.** if a draft has zero "i / me / my" and isn't a hot take, it usually reads dead. exceptions: pure educate posts ("fix is X"), pure observation posts ("the pattern is X").
- **everything's a feature list.** real posts have at least one moment, friction, decision, or reaction. "spent 4 hours on Y" beats "supports Y."
- **clean and complete.** real posts have asides, course corrections, sarcastic footnotes. "i was annoyed" beats "this was challenging."

When drafting, prefer one specific moment from the session over a summary of the whole thing. The session is the *source*, not the *subject*.

## Engagement priority

The goal is content that gets engagement from other builders, prompt engineers, and claude users. Drafts should aim to do at least one of:

- **teach** something specific (saves them time, shows them a pattern)
- **resonate** ("oh i've hit that exact thing")
- **provoke** (a take they want to agree or disagree with)
- **show receipts** (concrete proof, not vibes)

If a draft doesn't do at least one of these, cut it. "today was productive" type posts are dead on arrival.

## Voice rules (non-negotiable)

## Voice rules

The voice rules live in a separate file: **voice.md** (in the same directory as this SKILL.md). Read it before drafting. If `voice.md` does not exist or is empty, fall back to the defaults below and tell the user to set up their voice file.

### Default voice (if voice.md is missing)

- **all lowercase.** no caps except for proper nouns and acronyms.
- **no em dashes.** not the em dash (U+2014), not `--`. use periods, commas, or line breaks.
- **no en dashes.** `–` also banned. for ranges write "5 to 10."
- **no AI-tells.** no "delve," "leverage," "robust," "seamless," "in the realm of," "it's worth noting." if a draft sounds like a LinkedIn post, scrap it.
- **no hashtags** unless tagging a specific project handle.
- **no emoji spam.** one max, zero is usually right.
- **no thread emoji** (🧵), no "1/" numbering unless it's a real thread.
- **no CTAs.** no "what do you think?" or "let me know in the replies."
- **no self-aggrandizement.** show, don't announce.

The user's voice.md will override or extend these. If voice.md exists, follow it strictly.

## Format rules

- **char count: 240 to 400.** under 280 is ideal for single tweets. 280-400 means it'll wrap into a quote/reply but still works as one post. always show char count next to each draft.
- **first 7 words = the hook.** if the post doesn't earn a stop in the first line, it's dead. lead with the specific, concrete thing. not "today i learned..." but the thing itself.
- **specificity over generality.** "400 lines of auth code" beats "a lot of code." "caught a timing attack" beats "found a security issue." pull real numbers, file names, error messages, durations from the session when relevant.
- **no setup, no payoff structure** for short posts. the post IS the payoff.
- **line breaks are okay** for emphasis but max 2 in a single post.

## Output format

Always output exactly this structure:

Open with one line: `drafts (N variants)` followed by a single line like `pick whichever fits your mood, paste, post.`

Then a horizontal divider (`---`).

For each draft:

1. **A numbered, titled heading** that names the variant in a way that captures *what kind of post it is*, not just the angle. Format: `**N. the [descriptive name] ([one-line characterization])**`. Examples: "the design rejection thread (long-form, retro-fitted into a single banger)", "the errors-as-documents moment (the sleeper hit)", "the cold open (no setup, just the punch)", "the receipt drop (with proof)", "the unhinged version (only post if mood)".
2. **The draft itself, inside a code block** (triple backticks). this preserves line breaks and lets the user copy clean.
3. **A parenthetical note under the code block**: char count + any context that matters. e.g. `(~315 chars. screenshot attached: vitalik ledger)` or `(289 chars. teaches the eas+expo gotcha)` or `(244 chars. ship post, has receipts in commit history)`.
4. A horizontal divider (`---`) between drafts.

After the last draft, add a single closing note (no header, just one line) flagging:
- which project(s) the drafts reference if any
- if receipts/screenshots/commit links would strengthen any draft
- if extraction was thin or skewed (e.g. "all 5 drafts lean meta because this session was about building the skill itself")

Do NOT add closing offers like "want me to tweak these?" or "let me know which one you like." just stop after the note.

### Variant titles: how to name them well

The title is half the value. It tells the user at a glance what kind of post this is so they can pick by mood. Good titles describe the *shape* or *strategy* of the post:

- "the cold open" (leads with the punch, no setup)
- "the receipt drop" (claim + proof structure)
- "the sleeper hit" (less obvious angle but might land harder)
- "the unhinged one" (sarcastic, casual, only if mood fits)
- "the teach" (educate angle, walks through a fix)
- "the quiet flex" (ship post that doesn't sound like a flex)
- "the hot take" (opinion-forward)
- "the relatable" (resonance angle, "you've hit this")
- "the long-form retrofit" (longer post with internal structure, line breaks)
- "the human moment" (first-person, one specific moment, feels texted not posted)
- "the train of thought" (the user's actual reasoning shown step by step)
- "the small story" (a beginning-middle-end mini-narrative from the session)

Bad titles: "lesson 1," "ship post," "version A." these don't help the user pick.

## Default variant mix

When drafting 3-5 variants, at least ONE should always be a "human in the loop" angle (titles like "the human moment," "the small story," "the train of thought"). this is the variant most likely to feel alive. without it, the drafts skew toward feature-list product copy.

## Variety rule

The 3-5 drafts must hit *different angles*. Don't give 4 versions of the same lesson reworded. If the session genuinely only supports one angle, give 2-3 drafts at different lengths/tones rather than padding to 5.

## Tone reading

Before drafting, read the session's actual tone. If the user was frustrated and sarcastic, drafts can be sarcastic. If the session was a clean ship, drafts can be matter-of-fact. If something genuinely funny happened, lean into it. Don't force a "professional" tone onto a session that wasn't.

## Examples

### Good output (full format)

```
drafts (4 variants)
pick whichever fits your mood, paste, post.

---

**1. the teach (educate, walks through the exact fix)**

```
spent 2 hours on an EAS build that kept failing silently.

fix: add `legacy-peer-deps=true` to .npmrc. expo SDK 54 has a peer dep conflict that throws no readable error.

filing under "things claude figured out faster than the docs."
```

(289 chars. teaches the expo+eas gotcha. would land with mobile devs.)

---

**2. the cold open (ship, no setup)**

```
just shipped a holder visualizer where every character in the park is a real $BAGS token holder.

whales get crowns. creators get their own bench. 12 sprite variants, day/night cycle, png export.

took 4 days. claude wrote ~80% of the phaser code.
```

(266 chars. references BagsBrain park. ship post with concrete numbers.)

---

**3. the unhinged one (funny, only post if mood)**

```
asked claude to fix a bug.

it rewrote the entire file, deleted the bug, and also deleted three working features.

technically correct. the best kind of correct.
```

(166 chars. casual/funny. would expand if you want it longer or skip if not in the mood.)

---

**4. the human moment (first person, one specific moment from the session)**

```
4 hours into building this i caught myself opening twitter to draft a post about it.

then i remembered the whole reason i was building it was so i wouldn't have to do that anymore.

closed the tab. ran /clip. it drafted this.
```

(243 chars. tells a small story. uses i/me. has a reaction, a moment of self-awareness, ends on the tool itself as the punchline. feels texted, not posted.)

---

note: drafts 1 and 2 reference real projects (DataWise build pattern, BagsBrain park). receipts available for #2 if you want a quote-tweet of the actual park screenshot. #4 is the "human in the loop" variant, always include one in the default mix.
```

### Bad draft (don't do this)

```
Today I learned that prompt engineering is all about leveraging the right context -- it's a journey of discovery! 🚀✨ What's your favorite prompting tip?
```

(em dash, caps, "leveraging," emoji spam, CTA, generic. four voice rules violated, teaches nothing specific, no receipts.)

## After delivery

If the user picks one or asks for edits, edit in the same voice. Do not re-pad to 5 drafts. Do not add commentary about the post. Just deliver the revised version with new char count.

If the user posts it and comes back with engagement numbers later, log nothing. the user handles their own analytics.
