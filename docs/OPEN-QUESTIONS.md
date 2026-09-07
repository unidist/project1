# Open questions

Decisions this team has to make. Each one lists what the choice trades off —
deliberately without an answer, because making these calls yourselves is most of
what you'll actually learn here.

When you decide something, write it up in `docs/decisions/` using the template
and delete it from this list.

---

## 1. Which four commands?

The one decision that determines whether the bot survives past week one.

Ask of each candidate: *would someone in the chat use this unprompted, a week
after the novelty wore off?* Most bot commands fail that test. The ones that pass
are usually tied to something the group already does — an argument you already
have, information someone already asks for, a decision you already make badly.

Trade-off to watch: a command that's fun once and a command that's useful weekly
look identical when you're brainstorming and completely different in practice.

## 2. Where does it get hosted?

Free tiers generally sleep after inactivity, which means the first message after
a quiet period gets a delayed reply or none at all. Options range from "free and
sleeps" to "a few dollars a month and doesn't."

Trade-off: paying removes a whole category of confusing bug reports ("the bot
ignored me"), but it's money, and someone has to own the account.

Decide who holds the account and the credentials before you deploy, not after.

## 3. Does the bot need to remember anything?

Some commands are pure — same input, same output, no memory. Others need state:
scores, streaks, who said what.

- **No state** — simplest thing that works, and enough for a lot of commands.
- **A file on disk** — easy, but most hosts wipe the disk on every redeploy, so
  your data quietly vanishes. Know this before you rely on it.
- **A real database** — durable, and a meaningful jump in setup and concepts.

Trade-off: picking a database on day two because you *might* need it is the
classic way a two-week project becomes a two-month project. Picking one on day
twelve because you actually need it is fine.

## 4. What's the command prefix?

`!ping`, `/ping`, `@bot ping`, or no prefix at all.

Trade-off: no prefix means the bot has to guess whether a message was meant for
it, and it will guess wrong in a live group chat. A prefix is uglier and
unambiguous.

## 5. What happens when a command breaks?

It will. Someone will pass an argument nobody anticipated.

- Silent failure — the bot ignores it. Clean, but users think the bot is dead.
- Error reply — the bot says something went wrong. Helpful, but a bug in a loop
  can spam the chat.

Trade-off: whichever you pick, one crashing command must not take the whole bot
down with it. That's the actual requirement; the reply behavior is taste.

## 6. Public or private repo?

Branch protection — the setting that enforces "no direct pushes to `main`" — is
only available on public repos under a free plan. Private repos on the free plan
silently get no enforcement at all.

Trade-off: public gets you real enforcement and a portfolio piece; private keeps
your unfinished ideas unfinished in peace. Either is defensible. Choosing private
means the workflow is enforced by agreement instead of by the platform, which
works only if everyone actually follows it.

Note that if you go public, secrets in the git history are visible to the entire
internet, permanently. Check `.gitignore` before flipping the switch.

## 7. Who reviews whose PRs?

Options: anyone reviews anything, fixed pairs, or one person reviews everything.

Trade-off: one reviewer is consistent but becomes a bottleneck the moment they
have an exam. Anyone-reviews-anything spreads knowledge but means a beginner may
approve something they didn't understand. Fixed pairs land in between.

## 8. What happens after this ships?

Worth deciding before you're in the post-ship lull rather than during it. A
group that finishes and then goes quiet for three weeks usually doesn't restart.
