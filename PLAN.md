# PLAN.md — Two weeks to a shipped bot

## How to use this

This is a schedule with **gates**, not a spec. A gate is a yes/no test with no
room to argue — either it happened or it didn't. "Made good progress on the
webhook" is not a gate. "Someone texted `!ping` in the real group chat and got
`pong` back" is.

Gates exist because "are we on track?" is a question groups reliably answer
wrong. A gate answers it for you.

**Deliberately not in this document:** which commands to build, where to host,
how to structure the code. Those are your decisions — see
`docs/decisions/OPEN-QUESTIONS.md`. This document only says *when things must be
true by*.

---

## Phase 0 — Everyone is set up (Days 1–2)

Nobody writes bot code yet. The only goal is that four people can operate the
repo.

Each person, on their own branch, via a pull request:
- clones the repo
- adds their name and role to the team table in `README.md`
- opens a PR, gets one approval, merges

That's it. It's a trivial change on purpose — the point is that everyone has
survived the full loop once (branch → commit → push → PR → review → merge) on a
change where nothing can break. The first time someone learns this, you want it
to be on a line of text, not on code they care about.

**Gate:** four commits in `main` history, one authored by each person, each
having gone through a PR.

If someone can't clear this gate in two days, that's information. Better to know
now than in week two.

---

## Phase 1 — The skeleton is live (Days 3–5)

One or two people. Everyone else is still on Phase 0 or reading.

Build the smallest possible thing that is genuinely deployed: a bot that replies
`pong` to `!ping`. No features. No cleverness.

This phase is front-loaded on purpose. Deployment is where the unpredictable
problems live — environment variables that aren't set, callback URLs that are
wrong, free tiers that sleep. Find them now, while there are eleven days of
slack, not on day thirteen.

**Gate:** someone texts `!ping` in the real group chat, from their phone, with
every laptop closed, and gets `pong`.

Do not start Phase 2 before this gate passes. Commands written against a bot
that isn't deployed are commands you can't actually test.

---

## Phase 2 — Everyone builds their command (Days 6–11)

Now it parallelizes. One command per person, each on its own branch, each merged
by PR.

The rule that makes this work: a command is a function that takes message text
and returns reply text. No command touches another command's code. Nobody can
break anybody.

Suggested rhythm — one 90-minute session together in a room mid-phase. Not to
divide work, just to be in the same place when people get stuck. This is the
single highest-value hour of the two weeks.

**Gate:** four commands merged into `main`, each authored by a different person,
each working in the live group chat.

---

## Phase 3 — Harden and ship (Days 12–14)

No new features. This phase is entirely buffer, and it will get used.

- Fix what's broken
- Make sure the bot doesn't crash the whole app when one command throws
- Update `README.md`'s command table so it matches reality
- Write down what you'd do differently (`docs/retro.md`)

**Gate:** the bot runs for 48 hours without anyone touching it, and someone
outside the four of you uses a command without being told how.

That second half matters. "It works when I use it" and "someone else figured it
out" are different products.

---

## Rules for the two weeks

**Cut scope, never move the deadline.** The date is the only fixed thing here.
When you're behind — and you will be at some point — the answer is three commands
instead of four, not three extra days. A group that has shipped something small
will ship again. A group that has extended twice stops believing in dates.

**A missed gate is a decision point, not a failure.** When a gate doesn't pass on
schedule, meet and pick one: cut something, or accept a later ship date and say
so out loud. What kills projects is drifting past a gate without anyone naming
it.

**One person owns each phase.** Not a manager — just whoever is responsible for
noticing whether the gate passed and saying something if it didn't.

**Anything that isn't in an issue doesn't exist.** If you're building something
that has no issue number, either write the issue or stop building it.

## Where the decisions live

Real choices belong to you, not to this document. Record them in
`docs/decisions/` as you make them, one file per decision. In three weeks,
somebody will ask "why did we do it this way," and the answer should exist
instead of being argued again.
