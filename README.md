# App Store review skill

A skill for Claude Code (and other agents that read `SKILL.md` files) that walks an iOS app through Apple's App Review: the first submission, answering Apple, resubmitting, and shipping updates after approval.

I wrote it after taking two apps, Unposed and Bundle, through their first reviews in October 2026.
It combines what Apple actually sent us with Frederick James' post [How to get your iOS app approved in under 48hrs](https://x.com/frederickjames/status/2086419927548809285).
Advice from the post is marked "the post reports", and things that happened to our apps are marked "seen", so you can tell which is which.

## What happened to us

Both apps got "Guideline 2.1 Information Needed" within a day of the first submission, asking for a screen recording on a real device and answers to seven questions.
After we answered, each waited 12 to 14 days for the next review.
Bundle's reviewer tapped "Not now" on the AI consent sheet, the AI features disappeared, and Apple asked again because it couldn't find them.
Unposed was then rejected on an iPad Air for three things: Android named in the description, an age rating that ignored the wine glasses in its art, and a required name field right after Sign in with Apple.
While checking that iPad I also found that iPadOS 26 draws its window buttons over the back button of an iPhone-only app, so Back can't be tapped.

The skill turns each of these into a check you run before Apple does.

## What's in it

- `SKILL.md`: the guide, in the order you use it. It covers scoping the first version, walking the reviewer's path (declined prompts, Sign in with Apple, the iPad window, sandbox purchases, account deletion, reporting and blocking), metadata and age rating, review notes and the recording, waiting and expedite requests, rejections and resubmitting, and updates after approval.
- `references/history.md`: every message Apple sent both apps and what fixed it.
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

App Review changes, and one developer's experience isn't a rule.
When Apple sends you something new, add it to `references/history.md` with what fixed it.
Pull requests with your own review history are welcome.
