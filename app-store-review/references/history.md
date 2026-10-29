# What Apple sent our apps, and what fixed it

Newest last.
Add every new message from Apple here, with what fixed it, so the next submission starts from it.

## Bundle 1.0

Submitted 2026-10-08 with build 12 and both subscriptions.

**2026-10-09, Guideline 2.1 Information Needed.**
The standard questions for a new developer account: a screen recording on a real device and seven written answers.
Fix: review notes in Apple's numbered sections (purpose, setup and access, external services, regions, regulated content, In-App Purchase) and a demo video, then a resubmit the same day.

The wait after that was 12 days.
An expedite request on 2026-10-13 was accepted at once, and the review still came only on 2026-10-21.
An App Review Status email on 2026-10-20 got an answer the next day.

**2026-10-21, Guideline 2.1 Information Needed again.**
The reviewer could not find summaries, questions, Ask or topics.
They had tapped "Not now" on "Use AI on your articles?", and with AI off those features disappeared.
The server logs showed it: no AI request arrived during the review.
Fix: a reply with numbered steps (how to turn AI on, then where each feature is) and one annotated screenshot, and a resubmit on 2026-10-22.
Product fix for 1.0.1 (TestFlight 19, commit 8e52f30): with AI off, the summary and quick check cards say "AI is off" with Turn on AI, and Ask asks first.
Lesson: a reviewer takes the privacy-safe choice, so a feature hidden behind a "no" looks missing.

A second expedite on 2026-10-25 was accepted.
Still Waiting for Review on 2026-10-29.

## Unposed 1.0

Submitted 2026-10-09 with build 21 and six consumable purchases.

**2026-10-10, Guideline 2.1 Information Needed.**
The same seven questions and a request for a recording on a real device.
Fix on 2026-10-12: a reply with the seven answers and two recordings from an iPhone 15 Pro Max (the main walkthrough, 5 minutes with 27 seconds of private screens cut, and a 25 second clip of reporting a photo), the main recording also attached to App Review Information, the same text in the Notes field, then Edit, Update Review and Resubmit.
For the report clip the demo event got sample photos from two web guests.

The wait after that was 14 days.
An expedite request on 2026-10-25 was accepted at once, and the review came the next day.

**2026-10-26, rejected on an iPad Air 11-inch (M3), build 21.**

- Guideline 2.3.10, accurate metadata: the description said "On Android and everything else it opens in the browser".
  Fix: "on other phones it opens in the browser".
- Guideline 2.3.6, age rating: the app shows alcohol (wine glasses in the art and in sample photos).
  Fix: "Alcohol, Tobacco, or Drug Use or References" set to Frequent.
- Guideline 4, Sign in with Apple: a new account that signed in from the welcome screen went on to "Whose wedding is it?", and Continue stayed off until a name was typed, which reads as asking for the user's name.
  Fix (commit 3fe0577): the names are optional in onboarding and in New event, and an event with no names takes its type's name, "Our wedding".

Found while checking the same iPad before resubmitting: the window's close, minimise and resize buttons covered every back button and the logo, so Back could not be tapped.
Fix (commit 62e1d22): every presentation gets extra top safe area for those buttons.

Build 25 with both fixes, the reply, and a resubmit on 2026-10-26.
The App Store Connect app tile kept the old icon afterwards, while build 25 itself had the new one.
