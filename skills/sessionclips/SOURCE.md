# Source

Copied from [iam25th1/Sessionclips](https://github.com/iam25th1/Sessionclips) at commit `c988e41ff6f565be3689475e89d22aa605e2b28d`.

These files came across: `sessionclips/SKILL.md` and `sessionclips/voice.md`, the two files the repository's `pack.sh` packages as the skill, and the repository's `LICENSE`. The nested `sessionclips/sessionclips/` folder in the source is an identical copy of the same two files and was left out, so agents do not find the skill twice.

One change from the source: the four em dash characters were replaced, because files in this repository carry none.

- In `SKILL.md` line 74 and `voice.md` line 11, the rule that bans em dashes named the character by example. It now reads "not the em dash (U+2014), not `--`".
- In `SKILL.md` line 218 and `voice.md` line 51, the deliberately bad example post used an em dash. It now uses `--`, which the same rule also bans, so the example is still a bad one.

Licence: MIT, in `LICENSE`. To update, copy the same files again and repeat the change above.
