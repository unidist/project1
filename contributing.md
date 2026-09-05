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

## The loop

### 1. Claim an issue

Look at the project board, pick something in Todo, assign it to yourself, drag it
to In Progress. If nothing there fits, write a new issue first.

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

### 3. Write code, commit as you go

```bash
git add commands.py                   # stage the specific files you changed
git commit -m "Add leaderboard command"
```

Commit when a piece works, not once at the very end. Small commits are easier to
review and easier to undo.

Write messages as a command: "Add leaderboard command", not "added stuff" or
"asdf". Six months from now the messages are the only explanation of why
anything changed.

Avoid `git add .` unless you've checked `git status` first — it sweeps up
everything, which is how `.env` files and 200MB of junk end up committed.

### 4. Push and open a PR

```bash
git push -u origin feature/your-command
```

Then go to the repo on GitHub — it'll show a banner offering to open a pull
request. In the description, write what you changed and how you tested it, and
include `Closes #12` (with your issue number). That link auto-closes the issue
and moves the board card when the PR merges.

### 5. Get it reviewed

Tag someone. One approval is enough. Don't approve your own.

### 6. Merge, then clean up

Merge through GitHub's button, delete the branch when it offers, then locally:

```bash
git checkout main
git pull
```

Now start the next one from a fresh branch.

## Reviewing someone's PR

You're checking three things, in this order:

1. Does it work?
2. Will it break anything else?
3. Would a stranger understand it in three months?

Style opinions are not blockers. Leave them as comments and approve anyway.
"This works, one small suggestion" is a good review. Sitting on a PR for two days
is not — if you can't get to it, say so and pass it to someone else.

Say what you'd change and why, not just that you don't like it. And when
something's good, say that too. The reviewer's job isn't to find fault, it's to
make sure two people have seen every line that reaches `main`.

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

The `.gitignore` catches most of this. Check `git status` before committing and
you'll catch the rest.

## Adding a command

The design is deliberately simple so that this is a self-contained job:

1. Write a function in `commands.py` that takes the message text and returns the
   reply string.
2. Register it in the command dispatch table.
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

AI assistants are fine and encouraged. Just don't paste in code you can't
explain — you'll be the one fixing it when it breaks, and reviewers will ask.

## Glossary

| Term | Meaning |
| --- | --- |
| **repo** | The project folder plus its full history |
| **`main`** | The working version everyone shares |
| **branch** | Your own copy to work in without affecting others |
| **commit** | A saved snapshot of your changes, with a message |
| **push** | Send your commits up to GitHub |
| **pull** | Bring down changes other people pushed |
| **PR** | Pull request — "review my branch and merge it into `main`" |
| **issue** | A numbered task or bug |
| **merge conflict** | Two people edited the same lines; a human decides |
| **remote / `origin`** | The GitHub copy your local one syncs with |
| **webhook** | A URL an outside service POSTs to when something happens |
