# Mobile law: iOS and Android are a different lifecycle, not a reskin

A mobile build does not end at "deploy"; it ends at store approval, which has its own rules, timelines, and rejection traps. Read this in Phase 2 (stack choice) and Phase 7 (submission), whenever Phase 1 says mobile is in scope.

## Stack choice (Phase 2)

- **Cross-platform (React Native/Expo, Flutter):** one codebase, both stores, faster to build and to change later. Correct default unless the product needs something a cross-platform framework genuinely cannot do well (deep OS-specific integration, heavy native graphics).
- **Native (Swift/Kotlin):** justified when the product is platform-specific by nature, needs the newest OS APIs immediately, or performance is the entire point (games, camera-heavy apps).
- Explain the tradeoff in one plain sentence and let the user choose; do not default to native out of habit when cross-platform fits.
- If web already exists, evaluate whether a PWA (installable web app) satisfies the actual need before committing to two store submissions; ask this explicitly, many "we need an app" requests are really "we need it on the home screen."

## Build discipline (Phase 4, same rules as web, plus)

- Offline and poor-connectivity states are real states, not edge cases: design what the app shows with no network, and what happens to actions taken offline (queue and retry, or block with a clear message).
- Push notification permission is requested with context (explain why, right before the relevant action), never as a cold prompt on first launch; cold prompts are a top cause of permission denial and bad first impressions.
- Deep linking and universal links tested for every place the app is meant to open from a link (email, share, notification).
- App icons, splash screens, and screenshots for every required device size are part of the build, not a last-minute export.

## Pre-submission checklist (Phase 6/7)

- [ ] No crashes on the core flows; Apple's single largest rejection reason is crashes or bugs on first review, and it is entirely avoidable with a real test pass (see testing.md).
- [ ] Privacy policy URL live and accurate; privacy nutrition labels (iOS) and the Play Data Safety form filled out to match what the app actually collects, exactly, not optimistically.
- [ ] Screenshots and store description match what the app actually does and looks like; mismatched metadata is a common, avoidable rejection.
- [ ] Target the current required OS/API level for each store (this changes yearly; search for the current minimum before submitting, do not assume last year's number still applies).
- [ ] TestFlight (iOS) or a closed testing track (Android) run with real testers before the public submission; budget for feedback and fixes, not a same-day flip to production.
- [ ] New Google Play developer accounts need a closed testing period with a minimum tester count before a production release is allowed; check the current requirement, it has changed before and will again.
- [ ] Signing certificates and provisioning profiles (iOS) or the app signing key (Android) are generated, backed up, and documented in HANDOFF.md; losing these later means the user cannot ship updates to their own app.

## App store optimization (ASO)

Same instinct as web SEO, different surface: a specific, keyword-aware title and subtitle, an accurate description that leads with the core value in the first two lines (store listings truncate), and screenshots that show real functionality, not just pretty mockups. Research the top 3 comparable apps' listings in Phase 0 the same way competitor websites get researched.

## Review timelines (state honestly, they change)

Search for the current expected review time for each store before promising a launch date to the user; historically Apple has run roughly one to two days and Google a few hours for established accounts, both much longer for first-time developer accounts, but these numbers move and must be confirmed, not assumed from memory.

## Over-the-air updates

For cross-platform builds, note whether the framework supports OTA updates for JS/content changes (Expo EAS Update, CodePush-style tools) without a full store resubmission, and explain to the user which kinds of changes need a new store review versus which can ship instantly. This materially changes how "how to make simple changes themselves" reads in HANDOFF.md for a mobile product.
