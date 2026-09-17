# Contributing

How we work. Read this once before your first change — it's short, and following
it prevents nearly every way a group project falls apart.

If you've never used GitHub before, do
[Introduction to GitHub](https://skills.github.com) first. This document assumes
you've seen the words *branch* and *pull request* at least once.

## The two rules

1. **Every piece of work is an issue.** If it isn't an issue, nobody knows you're
   doing it, and two people end up building the same thing.
2. **Nothing reaches `main` without a pull request someone else approved.**
   No exceptions, including for whoever set up the repo.

Everything below is detail on those two.

## Where you work

You do not need to install anything to contribute to this repo. Pick the path
that matches what you're doing. The loop in the next section is identical on all
three — only the place you type changes.

A **terminal** (also called a shell, a command line, or bash) is the black window
where you type commands instead of clicking buttons. `git push` is a terminal
command. If you've never opened one, that's fine — path A gives you one that
already works, and path C doesn't need one at all.

### A. Codespaces — recommended for writing code

A **codespace** is a computer GitHub rents you for free, running in a browser
tab. It already has Python, git, and this project's files on it. Nothing installs
on your laptop, nothing to break, and it looks and works like a real code editor
(it's VS Code).

To start one: on the repo's front page click the green **Code** button →
**Codespaces** tab → **Create codespace on main**. First launch takes about a
minute.

Inside you get:

- a file tree on the left,
- an editor in the middle,
- a **terminal** at the bottom (if it's hidden: menu → Terminal → New Terminal),
- a **Source Control** panel on the left rail (the branch icon) that does commit
  and push with buttons, if you'd rather not type git commands yet.

Setup inside a fresh codespace, typed into its terminal:

```bash
pip install -r requirements.txt
cp .env.example .env                  # then paste the bot ID into .env
python bot.py
```

No `.venv` needed — a codespace is already isolated, because it's a separate
machine.

Two things to know so you don't burn your free hours:

- Codespaces are billed to **your own** account's monthly free allowance, not to
  Brandon's. Check what you've used at github.com/settings/billing.
- A codespace stops itself after 30 minutes of inactivity, but **delete it when
  you're done with a task** (github.com/codespaces → `...` → Delete). Your work
  is safe once it's pushed to GitHub; the codespace is disposable.

### B. Your own laptop — if you want to learn the setup

The traditional path. You install Python and git yourself, clone the repo, and
run everything locally. Setup steps are in `README.md`. It's more work up front
and it's genuinely worth doing at some point — just don't let it be the thing
that blocks your first contribution.

### C. The browser, no editor at all — for docs and small edits

For anything that isn't code you need to run:

- **Editing one file:** open it on GitHub, click the pencil icon, make the
  change, click **Commit changes…**. GitHub will tell you it can't commit to
  `main` (correct, it's protected) and offer to create a branch and open a pull
  request for you. Say yes. That's the whole loop, no terminal.
- **Editing several files:** press the `.` key while looking at the repo on
  GitHub. A full editor opens in the browser at `github.dev`. Free, instant, no
  limits — but nothing *runs* there, so it's for text, not for testing code.

Path C is how the README and this file get fixed. It's a real contribution and it
goes through the same review as everything else.

## The loop

### 1. Claim an issue

Look at the project board, pick something in Todo, assign it to yourself, drag it
to In Progress. If nothing there fits, write a new issue first — see
[docs/writing-issues.md](docs/writing-issues.md) for how to write one that's
actually actionable.

### 2. Make a branch

Never work directly on `main`. `main` is the version that's supposed to always
work; if you break it, you break it for all four of us.

```bash
git checkout main
git pull                              # get everyone else's merged work first
git checkout -b feature/your-command
```

`checkout -b` creates a branch and switches you onto it. Naming convention:

- `feature/leaderboard` — new functionality
- `fix/ping-crash` — something broken
- `docs/readme-setup` — documentation only

> **Without the terminal:** in a codespace, click the branch name in the blue bar
> at the bottom-left → **Create new branch**. In path C, GitHub creates the
> branch for you when you commit; you just type the name into the box.

### 3. Write code, commit as you go

```bash
git add commands.py                   # stage the specific files you changed
git commit -m "Add leaderboard command"
```

To **stage** a file means to mark it as part of the next commit. `git add` stages,
`git commit` saves the staged set as a snapshot with a message.

Commit when a piece works, not once at the very end. Small commits are easier to
review and easier to undo.

Write messages as a command: "Add leaderboard command", not "added stuff" or
"asdf". Six months from now the messages are the only explanation of why
anything changed.

Avoid `git add .` unless you've checked `git status` first — it sweeps up
everything, which is how `.env` files and 200MB of junk end up committed.

> **Without the terminal:** the Source Control panel lists your changed files.
> The `+` next to a file stages it, the message box plus the checkmark commits.
> Same two steps, different buttons.

### 4. Push and open a PR

```bash
git push -u origin feature/your-command
```

Then go to the repo on GitHub — it'll show a banner offering to open a pull
request. In the description, write what you changed and how you tested it, and
include `Closes #12` (with your issue number). That link auto-closes the issue
and moves the board card when the PR merges.

> **Without the terminal:** the Source Control panel's **Sync**/**Publish
> Branch** button is `git push`.

### 5. Get it reviewed

Tag someone. One approval is enough. Don't approve your own.

This is enforced by the repo, not just by agreement — GitHub will refuse the
merge until someone else approves.

### 6. Merge, then clean up

Merge through GitHub's button, delete the branch when it offers, then locally:

```bash
git checkout main
git pull
```

Now start the next one from a fresh branch. If you were in a codespace and the
task is done, delete the codespace too.

## Working with AI

All four of us are using Copilot and other AI assistants, and that's the point —
learning to work with these tools well is half of why this project exists. The
rules below are what keep it from turning into four piles of code nobody
understands.

**Everything an AI writes goes through the same loop.** Same branch, same pull
request, same one approval. An AI-written change is not a special case and does
not skip review.

**Don't merge code you can't explain.** This is the one non-negotiable. If a
reviewer asks "what does this line do" and the answer is "I don't know, Copilot
wrote it," that's a request for changes, not a nitpick. You will be the person
fixing it at 11pm when it breaks.

**Keep pull requests small.** AI will happily hand you 400 lines. A 400-line PR
gets rubber-stamped, which means it got zero review, which means the rule above
was pointless. Rough test: if you can't describe the change in three sentences,
split it into two PRs.

**Read `AGENTS.md` before you start, and keep it current.** `AGENTS.md` is a file
in the root of this repo that AI tools read automatically before writing code —
it tells them our conventions so all four of us get consistent output without
anyone having to remember to say it. If you find yourself correcting your
assistant on the same thing twice, that correction belongs in `AGENTS.md` as a
PR.

**Test it for real, don't trust that it looks right.** This is the specific way
generated code fails: it is confident, well-formatted, and wrong. It will invent
function names that don't exist and API fields GroupMe never sends. Running the
command and seeing the right reply in the actual group chat is the only proof
that counts.

**Assigning work to Copilot directly is allowed.** If you assign an issue to
Copilot, it opens a pull request like anyone else — and then *you* are the human
on the hook for it. Review it as if you'd written it, because as far as the rest
of us are concerned, you did.

## Reviewing someone's PR

You're checking four things, in this order:

1. **Does the author understand it?** Especially for generated code. If something
   looks unexplained, ask — "what does this part do?" is a normal, friendly
   review comment, not an accusation.
2. Does it work?
3. Will it break anything else?
4. Would a stranger understand it in three months?

Style opinions are not blockers. Leave them as comments and approve anyway.
"This works, one small suggestion" is a good review. Sitting on a PR for two days
is not — if you can't get to it, say so and pass it to someone else.

Say what you'd change and why, not just that you don't like it. And when
something's good, say that too. The reviewer's job isn't to find fault, it's to
make sure two people have seen every line that reaches `main`.

Reviewing is done entirely in the browser: open the PR, click **Files changed**,
click a line number to leave a comment, then **Review changes** → Approve or
Request changes. You never need an editor to review.

## Merge conflicts

Two people edited the same lines, and git won't guess who's right. Completely
normal — not a sign you did anything wrong.

Git marks the disputed section in the file like this:

```
<<<<<<< HEAD
their version
=======
your version
>>>>>>> your-branch
```

Delete the marker lines, keep whichever code is correct (sometimes both), save,
commit. If you're unsure which is right, ask the other person — they wrote it.

Conflicts get rarer if you `git pull` from `main` often instead of letting your
branch drift for a week.

## Things that break the repo for everyone

- Committing `.env` or any token, key, or password
- Force pushing to `main` (`git push --force`)
- Committing your virtual environment (`.venv/`) or `__pycache__/`
- Merging your own PR without review
- Pasting a real token into a prompt, an issue, or a PR description — it's public
  the moment you hit enter, same as committing it

The `.gitignore` catches most of this. Check `git status` before committing and
you'll catch the rest. This repo is **public**, so anything committed is visible
to anyone on the internet, permanently, even if the next commit deletes it. If it
happens, say so immediately — we rotate the token and move on. It's a five-minute
fix, but only if you tell someone.

## Adding a command

The design is deliberately simple so that this is a self-contained job:

1. Write a function in `commands.py` that takes the message text and returns the
   reply string.
2. Register it in the command dispatch table. A **dispatch table** is just the
   lookup that maps a command name to the function that handles it, so the bot
   knows `!ping` should call `handle_ping`.
3. Test it by calling the function directly — no server needed.
4. Add a row to the command table in `README.md`.

Your command can't break anyone else's. That's the point of the structure.

## Getting unstuck

Ask in the group chat. Two rules on that:

- **There are no dumb questions here.** Half this team is learning GitHub from
  scratch, which is expected and fine. A question asked at 20 minutes costs the
  group nothing; the same question asked after three hours of silent struggle
  costs us three hours.
- **Say what you tried.** "I ran X, expected Y, got Z" gets you an answer in
  minutes. "it doesn't work" gets you twenty questions first.

AI assistants are fine and encouraged — see **Working with AI** above for how we
use them here.

## Glossary

| Term | Meaning |
| --- | --- |
| **repo** | The project folder plus its full history |
| **`main`** | The working version everyone shares |
| **branch** | Your own copy to work in without affecting others |
| **commit** | A saved snapshot of your changes, with a message |
| **stage** | Mark a file as part of the next commit (`git add`) |
| **push** | Send your commits up to GitHub |
| **pull** | Bring down changes other people pushed |
| **PR** | Pull request — "review my branch and merge it into `main`" |
| **diff** | The list of exactly which lines a change added and removed |
| **issue** | A numbered task or bug |
| **merge conflict** | Two people edited the same lines; a human decides |
| **remote / `origin`** | The GitHub copy your local one syncs with |
| **webhook** | A URL an outside service POSTs to when something happens |
| **terminal / shell / bash** | The window where you type commands instead of clicking |
| **codespace** | A free computer GitHub runs for you in a browser tab, already set up |
| **`github.dev`** | The browser editor you get by pressing `.` on a repo — edits only, nothing runs |
| **branch protection / ruleset** | The setting that makes GitHub refuse a direct push to `main` |
| **`AGENTS.md`** | The file in this repo that AI tools read before writing code |
| **environment variable** | A setting passed to a program from outside it, like `GROUPME_BOT_ID`, so secrets stay out of the code |
| **dependency** | Someone else's code our project needs, listed in `requirements.txt` |
