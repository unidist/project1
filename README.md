# PROJECT1

A GroupMe bot for our group chat. Built by four of us as our first shipped project.

Replace `PROJECT-NAME`, the org name, and the team list below before your first commit.

## What this is

A bot that lives in our GroupMe. You text a command in the chat, the bot replies.
Each of us owns one command end to end.

This is a real, deployed thing — not a class assignment and not a demo that only
runs on somebody's laptop.

## Definition of done

We are finished when **four commands are live in the group chat and the bot
responds with everyone's laptops closed.**

That's the whole target. Anything else is a nice-to-have, and nice-to-haves get
cut before the deadline moves.

**Deadline:** two weeks from kickoff.

## How it works

GroupMe sends a POST request to our server every time someone posts in the group.
That's called a webhook. Our server reads the message, decides whether it's a
command, and if so POSTs a reply back to GroupMe's API.

```
someone texts "!ping"
        |
        v
GroupMe  -->  webhook  -->  our Flask app  -->  commands.py
                                                     |
                                  reply text  <------+
                                     |
                                     v
                        POST to GroupMe API --> message appears in chat
```

## Stack

- **Python 3.11+**
- **Flask** — the tiny web server that receives the webhook
- **requests** — how we send messages back to GroupMe
- **Render** (free tier) — where it's deployed

No database yet. If we need one, that's a conversation, not a commit.

## Project layout

```
bot.py             # Flask app, webhook endpoint, sends replies to GroupMe
commands.py        # every command's logic lives here — this is where you work
requirements.txt   # dependencies
.env.example       # copy to .env and fill in — never commit .env
docs/              # decisions and notes worth keeping
```

`commands.py` has no GroupMe-specific code in it on purpose. Commands take text
and return text. That keeps the door open for moving to iMessage or Discord
later without rewriting everyone's work.

## Running it locally

```bash
git clone https://github.com/unidist/PROJECT1.git
cd PROJECT-NAME

python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # Mac / Linux

pip install -r requirements.txt

cp .env.example .env          # then paste in the bot ID
python bot.py
```

A **virtual environment** (`.venv`) is a private folder of packages for this
project only, so installing something here can't break anything else on your
machine. Activate it every time you work — your prompt shows `(.venv)` when it's on.

### Testing without deploying

You don't need the internet involved to test your command. Import it and call it:

```python
from commands import handle
print(handle("!ping"))
```

To test the full loop against the real group chat, use **ngrok** to give your
laptop a temporary public address:

```bash
ngrok http 5000
```

Paste the `https://` URL it prints into the bot's callback URL field at
dev.groupme.com/bots, then text the group. Change it back when you're done.

## Environment variables

Copy `.env.example` to `.env` and fill in:

| Variable | What it is |
| --- | --- |
| `GROUPME_BOT_ID` | From dev.groupme.com/bots — identifies our bot when posting |

`.env` is gitignored. **Never commit it.** If a token lands in a commit it's
public forever, even if you delete it in the next commit — the history keeps it.
If that happens, say so immediately and we'll rotate the token. It's a
five-minute fix and not a big deal, but only if you tell someone.

## Commands

| Command | What it does | Owner |
| --- | --- | --- |
| `!ping` | Replies `pong`. Proves the bot is alive. | — |
| | | |
| | | |
| | | |

Fill this in as commands get merged.

## New to GitHub?

Start here — it takes under an hour and it's hands-on, not a video:

- https://skills.github.com — do "Introduction to GitHub" first
- Then read `CONTRIBUTING.md` in this repo, which is the actual workflow we use

You do not need to know how to code to contribute. You need to be able to
Google things and show up.

## Team

Roles not assigned yet ... update later 

- Brandon — role
- Dylan — role
- Eku — role
- Kidus — role
