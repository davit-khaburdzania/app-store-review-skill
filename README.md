# App Store review skill

A skill for Claude Code (and other agents that read `SKILL.md` files) that walks an iOS app through Apple's App Review: the first submission, answering Apple, resubmitting, and shipping updates after approval.

## What it catches

A first submission from a new developer account gets "Guideline 2.1 Information Needed" within a day, asking for a screen recording on a real device and answers to seven questions.
After any answer or rejection, the next review can take two weeks.
Reviewers decline consent prompts, so a feature hidden behind a "Not now" looks missing to them.
They test iPhone-only apps on an iPad, where iPadOS 26 draws its window buttons over the back button.
And they reject for small things: another platform named in the description, an age rating that ignores alcohol in the artwork, or a required name field right after Sign in with Apple.

The skill turns each of these into a check you run before Apple does.

## What's in it

- `SKILL.md`: the guide, in the order you use it. It covers scoping the first version, walking the reviewer's path (declined prompts, Sign in with Apple, the iPad window, sandbox purchases, account deletion, reporting and blocking), metadata and age rating, review notes and the recording, waiting and expedite requests, rejections and resubmitting, and updates after approval.
- `references/history.md`: every message Apple sent and what fixed it.
- `references/app-store-connect.md`: the [asc](https://asccli.sh) commands and App Store Connect clicks that worked, including the ones that look wrong but aren't (a reply doesn't resubmit, and Resubmit stays grey until you press Update Review).

## Install

Copy the `app-store-review` folder into your skills folder:

```bash
git clone https://github.com/davit-khaburdzania/app-store-review-skill.git
cp -r app-store-review-skill/app-store-review ~/.claude/skills/
```

Claude Code loads it whenever work touches App Store Connect, a submission, a message from Apple or a new build.
You can also call it by name: `/app-store-review`.

## Keep it current

App Review changes.
When Apple sends you something new, add it to `references/history.md` with what fixed it.
Pull requests with your own review history are welcome.
