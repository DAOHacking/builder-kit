# voice.md

This file defines the voice sessionclips drafts in. Edit it to match yours.
The skill reads this file before drafting and follows it strictly.

---

## hard rules (never break)

- **all lowercase.** no caps except for proper nouns (project names, brand names) and acronyms (API, JWT, CSP).
- **no em dashes.** not the em dash (U+2014), not `--`. use periods, commas, line breaks, or "+".
- **no en dashes.** `–` also banned. for ranges write "5 to 10."
- **no AI-tells.** no "delve," "leverage," "robust," "seamless," "in the realm of," "it's worth noting," "navigate the landscape," "elevate," "unlock," "harness." if a draft sounds like a LinkedIn post, scrap it.
- **no hashtags** unless tagging a specific project handle.
- **no thread emoji** (🧵), no "a thread:" framing, no "1/" numbering unless it's an actual thread.
- **no CTAs.** no "what do you think?" no "let me know below." end on a concrete detail or a hook.
- **no self-aggrandizement.** no "as a builder," no "in my journey of." show, don't announce.

## stylistic preferences

- **shorthand allowed:** fr, ngl, idk, lowkey, tbh, imo. use sparingly, not in every post.
- **emoji:** zero is usually right. one max, and only if it adds something. never decorative.
- **line breaks for emphasis:** okay, max 2 per post.
- **specificity wins:** "400 lines of auth code" beats "a lot of code." "caught a timing attack" beats "found a security issue." pull real numbers, file names, error messages, durations.

## hook rule

first 7 words have to earn a stop. lead with the concrete thing. not "today i learned..." not "i was working on..." just the thing.

## examples of good voice

```
spent 2 hours on an EAS build that kept failing silently.

fix: add `legacy-peer-deps=true` to .npmrc. expo SDK 54 has a peer dep conflict that throws no readable error.

filing under "things claude figured out faster than the docs."
```

```
just shipped a holder visualizer where every character in the park is a real $BAGS token holder.

whales get crowns. creators get their own bench. 12 sprite variants, day/night cycle, png export.

took 4 days. claude wrote ~80% of the phaser code.
```

## examples of bad voice (do NOT do this)

```
Today I learned that prompt engineering is all about leveraging the right context -- it's a journey of discovery! 🚀✨ What's your favorite prompting tip?
```
violations: caps, em dash, "leveraging," emoji spam, CTA, generic. teaches nothing.

```
Excited to share that I've been navigating the landscape of AI tooling and unlocking some powerful insights about how to elevate my workflow.
```
violations: caps, "navigating the landscape," "unlocking," "elevate," "powerful insights." pure AI slop.

---

## how to make this yours

If you're forking sessionclips for your own voice:

1. Edit the hard rules to match what you'd never do.
2. Replace the good/bad examples with posts from your own feed (or competitors' feeds you admire).
3. Add a "phrases I say" or "phrases I don't say" section if your voice has specific tics.
4. Add a "topics I post about" section so the skill biases extraction toward what you actually publish on.
