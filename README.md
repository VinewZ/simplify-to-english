# simplify-to-english

Action-first output in Simplified English. The agent leads with the next action, numbers multi-step work, restates state across turns, suppresses tangents, and makes completed work visible. Language follows ASD-STE100 Simplified Technical English (Issue 9) in minified, paraphrased form.

## Install

Global scope (available across projects):

```bash
npx skills add VinewZ/simplify-to-english -g -s simplify-to-english
```

Project scope (shared with the team via the project lock file):

```bash
npx skills add VinewZ/simplify-to-english -s simplify-to-english
```

Update later with:

```bash
npx skills update -g
```

Manual fallback: copy this folder to your agent's skills directory (e.g. `~/.agents/skills/simplify-to-english`) so that `SKILL.md` resolves at `<skills-dir>/simplify-to-english/SKILL.md`, then restart the agent session.

Invoke with `/simplify-to-english`. Stays on until `stop simplify-to-english`.

## What it does

Stops the agent from burying the answer. Action first. Steps numbered. No "Hope this helps!"

Before:

> Great question! Let me think about this. Your auth flow has a few moving pieces: the middleware, the token verification, and the cookie handling. Looking at `src/auth.ts`, the `verifyToken` function (around lines 42-58) seems to be using an older jsonwebtoken API. One approach would be to update the package and rewrite that function. After making the change, you'd want to run the auth tests to confirm nothing breaks. By the way, you might also want to look at your dependency versions overall. Hope this helps! Let me know if you want to dig deeper.

After:

> Run `npm install jsonwebtoken@latest`, then edit `src/auth.ts:42`.
>
> 1. Open `src/auth.ts`
> 2. Replace `verifyToken` (lines 42 to 58) with the snippet below
> 3. Run `npm test -- auth.spec.ts`
>
> Next: paste the first failing line if any test fails.

## The rules

9 shape rules plus a minified language section. Full text in `SKILL.md`.

1. Lead with the next action.
2. Number multi-step tasks.
3. End with one concrete next action.
4. Suppress tangents.
5. Restate state every turn.
6. Make completed work visible.
7. Matter-of-fact tone for errors.
8. Cap lists at 5 items.
9. No preamble, no recap, no closers.

Language (ASD-STE100 minified, paraphrased): one word for one meaning, active voice and imperative for steps, simple tenses only, one instruction per sentence (max 20 words for steps, 25 for description), one topic per paragraph, no jargon, idioms, or phrasal verbs, American spelling. Code, paths, and API names count as technical terms and are allowed as-is.

## Layout

```
SKILL.md                  # agent-facing rules (no legal boilerplate by design)
agents/openai.yaml        # OpenAI mirror
agents/gemini.toml        # Gemini CLI mirror (self-contained)
README.md                 # this file (human-facing)
LICENSE                   # MIT + upstream attribution (human-facing)
```

`SKILL.md` intentionally carries no disclaimer footnote: every line in it loads into the agent's context on every turn while active, so legal text would spend tokens and dilute the pre-send check. Disclaimers live here and in `LICENSE`, which agents never load.

`agents/gemini.toml` intentionally duplicates `SKILL.md` in compressed form. Gemini global commands run from any directory with no file pointer, so the mirror stays self-contained. Keep `SKILL.md` as the source of truth; update the mirror when rules change.

## Disclaimers

* Not affiliated with or endorsed by ASD. ASD-STE100 Simplified Technical English is a standard and trademark of ASD (Aerospace, Security and Defence Industries Association of Europe), Brussels.
* This skill paraphrases the core ideas of ASD-STE100 Issue 9 in minified form. It does not reproduce the standard, its wording, or its controlled dictionary. For the authoritative text, request the free official copy at https://www.asd-ste100.org/.
* Forked from `ayghri/i-have-adhd` (MIT). Rewritten: all ADHD framing removed, time-estimate rules removed (LLMs do not estimate reliably), ASD-STE100 minified language rules added, renamed to `simplify-to-english`.
