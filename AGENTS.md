# AGENTS.md

Instructions for AI coding assistants working in this repo.

Tools like GitHub Copilot, Claude, and Cursor read this file automatically before
they write anything, so the conventions below get applied without any of us
having to remember to type them into a prompt. It is also a fine thing for a
human to read — it's the shortest description of how this project is built.

**If you are a person:** when you find yourself correcting your assistant on the
same thing twice, that correction belongs in this file. Add it in a pull request
like any other change.

## What this project is

A GroupMe chat bot. Someone texts a command in our group chat, GroupMe sends the
message to our server as a webhook (an HTTP POST), the server decides whether it's
a command, and if so posts a reply back to GroupMe's API.

Four people are building this as their first shipped project. Most of them are
new to git and to Python. Optimize every suggestion for *being understood by a
beginner*, not for being clever or concise.

## Stack

- Python 3.11+
- Flask — receives the webhook
- requests — posts replies back to GroupMe
- No database. No ORM. No async framework. No frontend.

Do not add a dependency without being asked. If a task seems to need one, say so
and explain the trade-off instead of adding it silently.

## Layout

```
bot.py             # Flask app, webhook endpoint, sends replies to GroupMe
commands.py        # every command's logic — most work happens here
requirements.txt   # dependencies
.env.example       # template; the real .env is never committed
docs/              # decisions and notes worth keeping
```

## Hard rules

1. **Never write a real secret into a file.** No bot IDs, tokens, keys, or
   passwords in code, tests, examples, or commit messages. Read them with
   `os.environ` and document them in `.env.example` with a placeholder value.
   This repo is public.
2. **Never edit or create `.env`.** It is gitignored and belongs only on the
   machine running the bot.
3. **`commands.py` contains no GroupMe-specific code.** A command is a plain
   function: it takes the message text as a string and returns the reply as a
   string. No HTTP calls, no request objects, no framework imports. This is what
   lets us move to Discord or iMessage later without rewriting anyone's work, and
   it's what lets four people work in the same file without breaking each other.
4. **One crashing command must not take down the bot.** Handle failure at the
   dispatch layer in `bot.py`, not by wrapping every command body in
   `try`/`except`.
5. **No database, no persistent files.** If a task appears to need stored state,
   stop and say so — that's a decision for the team to record in
   `docs/decisions/`, not something to introduce in a pull request.
6. **Don't restructure code you weren't asked to touch.** No renaming, no
   reformatting unrelated files, no "while I was in here" cleanups. It makes
   review impossible for beginners.

## Style

- Plain, obvious Python. Prefer a `for` loop a beginner can read over a nested
  comprehension.
- Standard library where it does the job.
- Type hints on function signatures; skip them inside function bodies.
- Docstring on every command function saying what it replies with.
- Comments explain *why*, not *what*. Assume the reader can read Python but does
  not know our reasoning.
- No emoji in code or commit messages.

## Adding a command

1. A function in `commands.py` taking message text, returning reply text.
2. Registered in the dispatch table (the lookup mapping a command name to its
   function).
3. Testable by calling the function directly, with no server and no network.
4. A row added to the command table in `README.md`.

## Testing

There is no test framework yet. Verify a command by calling it directly:

```python
from commands import handle
print(handle("!ping"))
```

Never claim something is tested because it looks correct. If you could not run
it, say plainly that it is untested and what a human should check.

## Pull requests

- One issue per pull request. Keep the diff small enough that a beginner can
  review it in ten minutes.
- Description says what changed and how it was tested, and includes
  `Closes #<issue number>`.
- Every change goes through a branch and a pull request with one approval —
  including yours. Direct pushes to `main` are blocked.
- Branch names: `feature/...`, `fix/...`, `docs/...`.

## Still undecided

Do not assume an answer to these. If a task depends on one, ask. When the team
decides, the answer gets recorded in `docs/decisions/` and moved into this file.

- **Command prefix** — `!`, `/`, or something else (issue #12).
- **Host** — where the bot is deployed (issue #11).
- **Which four commands we're building** (issue #13).
- **What a broken command replies with**, if anything (`docs/OPEN-QUESTIONS.md`).
