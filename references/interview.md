# Phase 1 question bank

Ask in batches by topic. Skip anything already answered by context. Offer concrete options wherever possible; open questions exhaust users. For non-technical users, attach a one-line plain explanation to any term that could confuse.

## Batch A: identity and purpose

1. What is this product's one-sentence job? (What problem does it solve, for whom?)
2. What should it be called? Do you have a logo, want one generated, or want to pick from a few directions I mock up?
3. Who exactly uses it? (Age range, region, tech comfort, language(s). This drives voice, design, and payment options.)
4. How should the brand sound? Offer 3 or 4 voices with a sample sentence each, tuned to the niche. Example set: warm and personal / confident and direct / playful / premium and quiet.
5. What existing product should this feel closest to, and what about it do you want to avoid?

## Batch B: pages and features

1. List the pages you know you want. I will propose the ones you have not thought of (404, empty states, settings, legal pages) and you approve.
2. For each page: what is its single main action? What features does it carry? (Walk the user page by page; never assume features.)
3. What should the very first visit feel like? Onboarding tour, straight to content, signup wall, or something else?
4. Homepage specifics: what does a visitor see vs what does a signed-in user see? (This question repeats for every major page. State-awareness is asked explicitly, per page.)
5. What must the admin dashboard show and control? Offer a checklist: user management, content moderation, sales/analytics, feature flags, support inbox, AI agent escalations.

## Batch C: users, accounts, and state

1. How do people sign up and sign in? Offer: email+password, magic link, Google, phone/OTP, social providers relevant to the region. Explain the tradeoff of each in one line.
2. Email verification, phone verification, or both? Present provider options with rough cost.
3. What can a visitor do without an account? Where exactly is the account wall?
4. Are there user tiers (free/pro, buyer/seller, student/teacher)? What can each tier do?
5. What limits apply to users? (Uploads per day, listings, message rate, storage.) Propose sensible defaults for the niche and let the user adjust.

## Batch D: money

1. Does this product take payment? For what, and how (one-time, subscription, commission, credits)?
2. Which payment methods fit the audience? Search current options for the user's region and present them (for example: Stripe, Paddle, Lemon Squeezy, and for Central/West Africa: Mobile Money via Notchpay or CinetPay). One-line tradeoff each.
3. Refund policy: offer 3 standard postures (no refunds on digital goods / 14-day no questions / case-by-case) and note the legal minimums for their jurisdiction after a quick search.
4. Payouts (marketplaces): how and when do sellers get paid?

## Batch E: rules, safety, and enforcement

1. What behavior is forbidden on this product? Propose a niche-appropriate list; the user edits.
2. What happens to rule-breakers? Offer an escalation ladder: warning, temporary suspension, ban, content removal, payout hold. The chosen ladder is written into the Terms and actually enforced in the admin tools.
3. Who moderates, and with what tools? (Report button, admin review queue, automated flags.)
4. Any age restrictions or regional restrictions?

## Batch F: the product's AI agent (if wanted)

1. Do you want an AI assistant inside the product? What is its job in one sentence?
2. What may it do? (Answer FAQs, track orders, recommend items, handle returns up to X amount.)
3. What must it never do? (Issue refunds, change account data, discuss competitors, give legal/medical advice.)
4. When it cannot help, where does it escalate, and how fast should a human respond?
5. What personality does it have? (Same brand voice by default.)

## Batch G: design and feel

1. Show 3 to 5 real reference sites from research in this niche. Ask which direction resonates and why.
2. Main theme color and mood: offer palettes, not a color wheel. Each palette gets a one-line personality description.
3. Typography: present 3 font pairings as rendered examples or links, never just names. State what each pairing signals.
4. Motion: how should the site feel when scrolling and navigating? Offer: calm and minimal / lively with scroll-reactive media / cinematic with big transitions. Explain the performance cost of each honestly.
5. Dark mode: yes, no, or toggle?
6. Any interactive 3D or signature visual moment wanted? If yes, explain sourcing (curated open-source pieces, tweaked to fit) and where it would live.
7. Custom cursor, page transitions, hover behaviors: propose a coherent set; do not decorate randomly.

## Batch H: images and media

1. Real photos (user provides), stock (sourced and credited), or AI-generated (user approves each)? Product photos are always real if the product is physical.
2. Brand and social icons come from real icon sets (Simple Icons or the platforms' official kits), never redrawn approximations.

## Batch I: scale, growth, and platforms

1. Honest expectation: dozens of users, thousands, or aiming for millions? (This drives architecture cost; answer changes the plan.)
2. Web only, mobile app too, or both eventually? (Architecture differs if mobile is coming.)
3. Which regions and languages at launch? Multi-language later or now?
4. SEO: does discovery through Google matter for this product? (Marketplaces and content sites: yes. Internal tools: no.)
5. Personalization: should returning visitors or specific locations see different content?
6. Social proof: live activity counters, reviews, user photo uploads? Only if honest data will exist to back them.
7. Exit-intent or behavioral prompts (offer help after 30 idle seconds, capture before leaving)? These convert but can annoy; state both sides.

## Batch J: operations

1. Where should this be hosted? Present options that fit budget and region, with monthly cost.
2. Domain: owned already, or need one?
3. Deployment credentials and any test accounts for third-party services (needed for end-to-end verification; explain exactly what each credential is used for and that they live only in env files, never in code or context).
4. Analytics: privacy-friendly option (Plausible, Umami) vs Google Analytics. One-line tradeoff.

## The playback

After all batches, write the full plain-English playback: the product, page by page, state by state, flow by flow, the design direction, the enforcement ladder, the agent's boundaries, the stack implications of the scale answer, and what is explicitly out of scope for v1. Ask for approval or edits. Loop until approved. The approved playback becomes plan.md.
