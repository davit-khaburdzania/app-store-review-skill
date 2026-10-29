---
name: app-store-review
description: Guideline for getting an iOS app through Apple's App Review, from the first App Store submission to every later update. Covers scoping the first version, walking the reviewer's path (iPad, declined prompts, Sign in with Apple), App Store Connect metadata (description, age rating, App Privacy, in-app purchases, subscriptions, App Clip), review notes and the device screen recording, waiting and expedite requests, answering "Guideline 2.1 Information Needed" and rejections, resubmitting, and planning and shipping new features and builds after approval. Use it whenever work touches App Store Connect, App Review, a submission, review notes, an Apple message or rejection, TestFlight builds headed for the App Store, or deciding what goes into the next version of Unposed, Bundle or any other iOS app, even when the user only says "submit", "Apple replied", "rejected", "resubmit", "new version" or "is it ready for review".
---

# App Store review

This is what Unposed's and Bundle's first App Reviews taught us in October 2026, plus a post by Frederick James ("How to get your iOS app approved in under 48hrs", X, August 2026).
Lines marked "seen" happened to our apps.
Lines marked "the post reports" are one developer's experience that we have not confirmed.

Read these when you need them:

- `references/history.md`: every message Apple sent each app and what fixed it. Add new ones there.
- `references/app-store-connect.md`: asc commands, App Store Connect and Chrome steps, what to leave to a person.

## How review behaves

- Seen: a first submission from a new developer account gets "Guideline 2.1 Information Needed" within a day.
  It asks for a screen recording on a real device and seven written answers.
  Both apps got it, so answer it in the notes before Apple asks (section 4).
- Seen: after any answer or rejection, the next review is much slower.
  Bundle waited 12 days and Unposed 14.
  The post reports the cause: a submission with questions or a rejection goes to a senior reviewer, where two weeks is normal, while a plain first review takes about 48 hours.
- Seen: an expedite request is accepted on the spot and asks for no reason.
  The review then came one day later for Unposed and eight days later for Bundle.
  A version rejected during an expedited review goes back to the expedited queue on resubmit.
- Seen: an App Review Status email got an answer the next day.
- Seen: each review can raise issues the earlier one did not mention.
  Unposed's second review raised three guidelines the first had not.
- Seen: reviewers test iPhone-only apps on an iPad (Unposed: iPad Air 11-inch (M3)).
- Seen: reviewers take the privacy-safe answer.
  Bundle's reviewer tapped "Not now" on "Use AI on your articles?", the AI features disappeared, and Apple asked again under 2.1.
  The server logs showed it, because no AI request arrived during the review.

The aim for a first submission is a review that is easy and boring, so it stays in the fast queue.

## 1. Scope the first version

Ship the fewest features that make the app useful, and add the rest in updates.
Every extra feature, and every gate in front of one (a consent sheet, a paywall, a permission), is another place a reviewer can get lost.
The post reports that questions are what move a review to the slow queue.

- Use plain Apple in-app purchase: consumables or subscriptions, Restore Purchases, prices shown before buying.
- Keep gray areas for a later update, explained in its notes: offers when someone abandons a purchase, offers inside Manage subscription, paywall experiments controlled from the server, review prompts during onboarding.
- Leave out "coming soon" screens, placeholders and anything that does not work yet.
- Make no claim the app cannot prove, and show no invented social proof.
- Build the screens natively in SwiftUI, so sheets, keyboards, safe areas and Dynamic Type behave on every iPhone and on an iPad.

## 2. Walk the reviewer's path

Do this on a real iPhone and on an "iPad Air 11-inch (M3)" simulator, with the build you will submit, signed out, as a brand-new user.

1. Every first-run path.
   Start from "Create" and also from "Sign in" on the welcome screen with a new account, because an empty account can land where the "Create" path never goes (Unposed's names question).
2. Decline every prompt: notifications, camera, photos, tracking, AI consent, the paywall's close button where there is one.
   Every feature must stay visible with a way to turn it on, whatever the reviewer answered.
   Bundle added "AI is off" cards with Turn on AI for this.
3. Sign in with Apple.
   After it, the app may not require a name or an email, because Apple already gave them.
   Any required field that reads like a name counts, even one meant for something else: Unposed's "Whose wedding is it?" names had to become optional.
   Prefill from what Apple gave, or make the field optional.
   Offer Sign in with Apple wherever another third-party sign-in is offered.
4. The iPad window.
   iPadOS 26 and later run an iPhone app in a window and draw the close, minimise and resize buttons in its top-leading corner, outside the safe area, over back buttons and logos.
   Check every screen and every full-screen cover.
   Unposed's fix gives each presentation's view controller extra top safe area from `view.edgeInsets(for: .safeArea(cornerAdaptation: .vertical))`.
   A SwiftUI padding at the root does not reach through a NavigationStack.
   On the simulator the first tap after launch only focuses the window.
5. Purchases in the sandbox.
   Products load, the purchase finishes, the feature unlocks at once, and Restore works.
   StoreKit returns no products while the Paid Apps Agreement, tax or bank is not active, so check Agreements, Tax and Banking before submitting.
   The production server must accept sandbox receipts, because that is what reviewers buy with.
6. Account deletion inside the app, easy to find.
   If deletion waits (Unposed keeps events 30 days), the app says how long.
   Revoke the Sign in with Apple token.
7. Content from users: a way to report content, to block or remove a user, to hide content and to reach support, with reports acted on within 24 hours.
8. Third-party AI: ask before sending someone's data to an outside AI, and name the service.
9. Links: privacy policy and terms open from the app and from the description, and the support URL works.
10. Demo content: a reviewer needs no credentials or gets working ones, and any demo event or account stays live through review (Unposed keeps a demo wedding live).

Fix what this finds before submitting.
After a rejection, walk it again on the device Apple named.
That is how Unposed found the iPad window buttons before the reviewer did.

## 3. App Store Connect metadata

- Description, subtitle, keywords, promotional text and screenshots name no other platform.
  Write "other phones" or "a browser", never Android or Google Play (Unposed, guideline 2.3.10).
- Screenshots show the current app.
- The age rating covers everything a user can see, including illustrations, sample photos, covers and what users upload.
  Wine glasses at a wedding count as alcohol: Apple made Unposed set "Alcohol, Tobacco, or Drug Use or References" to Frequent (guideline 2.3.6).
  Answer yes to user-generated content when users share anything.
- App Privacy matches every SDK and server that touches user data.
  RevenueCat means Purchase History, and analytics count only when they actually send.
  Each target carries a privacy manifest.
  Change the answers and the privacy policy in the same change that adds a data use.
- In-app purchases each have a review screenshot and go into the first submission with the version.
  Subscriptions need the subscription group and each subscription attached to the submission too.
- An App Clip needs its default experience, an invocation URL in App Review Information, a live apple-app-site-association file, and a way to work when opened from its App Store card with no link (Unposed's Clip asks for the event code).
- Release type manual, so approval does not publish before you are ready.

## 4. Review notes and the recording

The Notes field holds 4000 characters.
Write it for someone who has never seen the app: short sections, the exact happy path with exact button names ("tap Allow", not "Allow or Not now"), and any demo codes.
Answer the seven questions Apple sends new accounts, in this order, so it has nothing to ask:

1. Screen recording: what it shows, and on which device and build.
2. Purpose and audience: what the app does and who it is for, in two or three sentences.
3. Setup and access: whether credentials are needed, the steps to the main features, where Settings and account deletion are.
4. External services: every outside service the app or server uses, and what for.
5. Regional differences: whether the app behaves the same everywhere it is sold.
6. Regulated industry and protected material: whether either applies.
7. In-app purchase: each product with its ID and price, where it is offered, and where Restore is.

Add moderation (report, block, hide, response time) and account deletion when they apply.
Keep it honest: never hide a feature, and never describe something the build does not do.
Write the notes and every reply to Apple with the humanize-writing skill, in the first person singular ("I", not "we").

Record the walkthrough on a real iPhone with the submitted build, starting from the first launch.
Show sign-up, the main flow, a sandbox purchase with its receipt, moderation and account deletion, and cut out private screens.
Captions naming each part help, and the post reports that notes plus a recording are close to required for a first submission.
Attach it under App Review Information (one file) before the first submission.

## 5. Submit and wait

If an AI agent prepares the submission, a person presses Submit for Review and Resubmit to App Review.
Check progress with `asc review status --app <app-id>`.
Right after a resubmit the version reads Ready for Review for a while before Waiting for Review, which is normal.

While a version waits:

- Do not cancel the submission, remove items or swap the build.
  Each one puts it at the back of the queue and loses an expedite.
- Builds made meanwhile wait for the next version, or go in if Apple rejects this one.
- After about four days of Waiting for Review, or when a date matters, request an expedited review (steps in `references/app-store-connect.md`).
  Apple warns that too many requests get later ones refused, so send another only with a new reason, and stop at two per version.
- If an expedited review still does not move, send an App Review Status question through Contact Us and keep its case ID.
- Watch the server logs during the review: they show what the reviewer did and did not reach.
- Anything sent to Apple (a form, an email, a reply) speaks for the developer, so an agent asks before sending.

## 6. When Apple answers

Information Needed (2.1): answer every numbered point, attach the recordings or an annotated screenshot, and copy the answers into the Notes field so the next reviewer sees them too.

Rejection:

1. Fix every guideline listed, then walk section 2 again on the device Apple used, so the next review finds nothing new.
2. A rejected version takes a new build at no cost to its place in the queue.
   Attach the newest build with all fixes and everything finished since.
3. Description, age rating and other metadata change without a build.
4. Reply in App Store Connect, per guideline, with what changed and in which build.
   An agent saves it as a draft for the developer to send.
5. Resubmit.
   A reply alone does not resubmit: open the version with Edit in the submission, press Update Review, then press Resubmit to App Review.
6. When Apple misread the app, say so plainly with steps and the guideline's own words, or appeal to the App Review Board.
   The post reports that complaining does nothing and a calm, specific answer does.
7. When Apple writes again after a rejection, ask in the reply for every remaining issue at once, and for a phone call if the back and forth continues.
   The post reports that a call request moves the app to a shorter queue.

## 7. After approval: features and new builds

- Release 1.0, then send the builds that waited, as 1.0.1, right after.
- Keep updates small and frequent, with one risky change per update.
  The post reports that a run of approved small updates gets faster review.
- For each new feature, walk the parts of section 2 it touches.
  Update App Privacy, the privacy policy and the age rating when its data or content needs it.
  Add its steps to the review notes, and record a short clip when a reviewer cannot reach it easily.
- Gray-area features (offers, paywall tests, user content, AI) get a paragraph and a demo in the notes.
  Never change reviewed behavior from the server after approval.
- Deploy the backend before the build that needs it, and keep the backend working for the build in review and the build on the App Store.
  A reviewer runs a build from days ago against today's server.
- Every build goes to TestFlight first, and the build of a version waiting for review never changes.
- What's New says plainly what changed for the person using the app.
- Add every new message from Apple to `references/history.md` with its fix.
