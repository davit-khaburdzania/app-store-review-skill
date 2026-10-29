# App Store Connect and the asc CLI

Commands and clicks that worked in October 2026, with asc 5.7.

## Reading state

```bash
ASC_TIMEOUT=120s asc review status --app <app-id>
ASC_TIMEOUT=90s asc review submissions list --app <app-id>
ASC_TIMEOUT=90s asc review items-list --submission <submission-id>
ASC_TIMEOUT=90s asc versions view --version-id <version-id> --include build
ASC_TIMEOUT=90s asc builds list --app <app-id> --limit 5
ASC_TIMEOUT=180s asc validate --app <app-id> --version 1.0
ASC_TIMEOUT=90s asc iap list --app <app-id>
ASC_TIMEOUT=90s asc review history --app <app-id>
ASC_TIMEOUT=90s asc review attachments-list --review-detail <review-detail-id>
```

`asc review history` lists each submission's items, including subscriptionGroupVersion and subscriptionVersion next to the appStoreVersion, which shows whether subscriptions went in with the version.

`asc validate` needs a long timeout.
It always warns that each in-app purchase is READY_TO_SUBMIT, even when the purchases are already items of the submission, so check `items-list` before acting on that warning.
`review status` names the blockers of a rejected version and the next action.

States you will meet: WAITING_FOR_REVIEW, IN_REVIEW, REJECTED with the submission in UNRESOLVED_ISSUES, PREPARE_FOR_SUBMISSION after a new build is attached to a rejected version, PENDING_DEVELOPER_RELEASE after approval with manual release.

## Changing metadata

```bash
# Review notes (4000 characters at most)
ASC_TIMEOUT=90s asc review details-update --id <review-detail-id> --notes "@file:notes.txt"
# One attachment for App Review Information (the device recording)
asc review attachments-upload ...
# Description and other version text
ASC_TIMEOUT=90s asc localizations list --version <version-id> --output json
ASC_TIMEOUT=90s asc localizations update --id <localization-id> --description "@file:description.txt"
# App name, subtitle, privacy URL
ASC_TIMEOUT=90s asc localizations list --app <app-id> --type app-info --output json
# Age rating
ASC_TIMEOUT=90s asc age-rating view --app <app-id>
ASC_TIMEOUT=90s asc age-rating edit --app <app-id> --alcohol-tobacco-drug-use FREQUENT_OR_INTENSE
# Screenshots
ASC_TIMEOUT=90s asc screenshots list --version-localization <localization-id>
asc screenshots upload --device-type IPHONE_69 ...
# Attach a build (5.x renamed --build to --build-id)
ASC_TIMEOUT=120s asc versions attach-build --version-id <version-id> --build-id <build-id>
```

Any flag value takes `@file:PATH`, which is the safe way to pass long text.
Metadata changes need no new build, and they reach the reviewer with the next resubmit.

## Clicks to leave to a person

Claude Code's auto-mode safety check blocked these for us, even after a yes in chat, so the agent prepares everything and a person presses them:

- Adding the version to a review submission, and Submit for Review.
- Resubmit to App Review.
- At times, asc writes for age rating, price and availability (they went through later on the same account).

## Chrome steps that work

These were done in Chrome while signed in to App Store Connect and developer.apple.com.

- The submission page is `https://appstoreconnect.apple.com/apps/<app-id>/distribution/reviewsubmissions/details/<submission-id>`.
  Messages from Apple and the "Reply to App Review" button are at the bottom.
- The reply box takes 4000 characters, has Save Draft, and takes attachments up to about 10 MB each (file_upload from the scratchpad).
  Set its text with form_input and check the character counter moved.
- After a rejection, "Resubmit to App Review" stays grey until you open the rejected version with Edit in the items table and press Update Review on the version page.
  The item then reads Ready for Review and Resubmit turns blue.
- In-app purchases join the first submission from each purchase's own page: Add for Review, then Draft Submission.
- Expedite: `https://developer.apple.com/contact/app-store/?topic=expedite`.
  Pick the Organization that owns the app (the default may be another team with no apps), type the app name and click the suggestion, set Platform to iOS, Send.
  There are two Platform selects in the page and only the second one shows, so set the visible one.
  It asks for no reason.
  The answer appears on the page at once, for example "We'll expedite review for <app name>."
- A status question goes through developer.apple.com/contact (App Review, App Review Status, Email) and gets a case ID.

## Things that looked wrong but were not

- The app tile and header in App Store Connect kept the old icon while the version in review carried a build with the new one.
  The icon of each build is in `asc builds list --output json` under `iconAssetToken.templateUrl`, which is how to check what Apple is reviewing.
- A reply in the Messages section does not put the app back in the queue.
  Only Resubmit does.
- After a rejection the in-app purchases stay in the submission as Ready for Review, so they need no re-adding.

## Uploading builds

A TestFlight lane can time out at the upload after the archive is done (both apps, twice).
Re-send the ipa the lane already built with `asc builds upload --app <app-id> --ipa <path> --wait` instead of running the lane again.
A 409 "bundle version must be higher" means the ipa's CFBundleVersion is wrong, so read it from the ipa first.

## Screen recordings

Record on a real iPhone with the submitted build.
Compress for upload with ffmpeg: 720 px wide, H.264, `-crf 27` to `-crf 30`, `-an` (no sound), which brought a 5 minute recording under 10 MB.
Cut private screens with trim and concat rather than blurring.
