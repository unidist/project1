# Writing issues

An issue is a numbered task in the repo's Issues tab. Writing a good one is the
single highest-value skill in this project, and it's the one that transfers
directly to anything bigger we build later.

Here's why it matters more than it looks. An issue is a **specification** — a
description of what should exist, precise enough that someone other than you
could build it. When you hand work to a teammate, a vague issue costs you a
round of questions. When you hand work to an AI assistant, a vague issue is the
*entire* input it has, so vagueness comes back as confidently wrong code. Same
skill either way; the AI just makes the consequences arrive faster.

## The four parts

Every issue answers four questions. Two sentences each is usually enough.

**1. What** — the behaviour you want, in plain language.

**2. Done when** — how we will know it's finished, stated as something someone
can check. "Works well" is not checkable. "Replying `!roll 2d6` in the group chat
returns two numbers between 1 and 6 and their total" is.

**3. Don't** — the boundaries. What this change must not touch, break, or
introduce. This is the part everyone skips and the part that prevents the most
rework.

**4. Where** — which file or function, if you already know. Saves a guessing
round.

## Bad and good

Bad:

> Add a leaderboard command

That's a title, not an issue. Every one of these has to be guessed at: leaderboard
of what, how many entries, sorted how, where does the data come from, what
happens when there's no data, what's it called.

Good:

> **What:** A `!top` command that replies with the five highest scores from the
> running tally, highest first.
>
> **Done when:** Texting `!top` in the group chat returns five lines in the form
> `1. Name — 42`. Ties are broken by whoever reached the score first. With fewer
> than five scores it lists however many exist. With none it replies
> `No scores yet.`
>
> **Don't:** Don't add a database or write to disk — read from the in-memory
> tally that already exists. Don't change the scoring logic itself.
>
> **Where:** `commands.py`, plus a row in the README command table.

The second one can be handed to a teammate, to Copilot, or to yourself in three
weeks, and you get the same thing back.

## Size

One issue should be one pull request, and one pull request should be reviewable
in about ten minutes. If the "done when" section has more than about three
checkable items, it's two issues.

Splitting is cheap. Untangling a 400-line pull request that did four things at
once is not.

## Habits

- **Write the issue before you start building**, not after. If you're building
  something with no issue number, either write the issue or stop building.
- **Assign it to a person.** An unassigned issue is a wish. Our board has an
  Assignees column for exactly this.
- **Link it from the pull request** with `Closes #12` in the description. That
  auto-closes the issue and moves the board card when the PR merges.
- **If the issue turns out to be wrong, edit it.** Discovering halfway through
  that the "done when" was unrealistic is normal and worth recording — change the
  issue and say why in a comment, rather than silently building something else.
- **Decisions are not issues.** "Should we use a database?" belongs in
  `docs/OPEN-QUESTIONS.md`, and its answer belongs in `docs/decisions/`. Issues
  are for work with a definite finish line.

## Template

Copy this into a new issue:

```markdown
**What:**

**Done when:**

**Don't:**

**Where:**
```
