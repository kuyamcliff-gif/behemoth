# Writing law: every word sounds human, specific, and on-brand

This governs all user-facing text: page copy, headings, buttons, emails, error messages, the product's AI agent replies, docs, READMEs, marketing. Based on Wikipedia's "Signs of AI writing" (WikiProject AI Cleanup) plus voice rules.

## The process for any piece of text

1. Write it in the brand voice chosen in the interview, grounded in how real products in this niche talk.
2. Scan against every pattern below and rewrite violations.
3. Run the audit pass: ask "What makes this so obviously AI generated?" List remaining tells. Fix them.
4. Grep for em dash (U+2014) and en dash (U+2013). Remove. This is a hard rule with zero exceptions, including inside code comments.
5. Read it aloud mentally. If a human running this exact business would not say it, rewrite it.

## Voice calibration

If the user provides sample writing (their posts, their old site), match it: sentence length habits, word register, punctuation quirks, how they open. Do not upgrade "stuff" to "solutions". With no sample, default to natural, varied, specific, with actual personality.

## Signs of soulless text (even if technically clean)

Same-length sentences throughout. No opinions. No specifics. Reads like a press release. Fix by: varying rhythm (short line, then a longer one that takes its time), being concrete about details, letting the brand have a take.

## Content patterns to detect and remove

1. **Inflated significance.** Banned framing: "stands as a testament", "pivotal moment", "marking a shift", "evolving landscape", "underscores its importance", "deeply rooted", "setting the stage for". State the fact instead.
2. **Notability padding.** "Featured in X, Y, and Z" lists, "active social media presence". Use one specific, sourced claim or nothing.
3. **Fake-depth -ing tails.** Sentences ending in ", highlighting...", ", showcasing...", ", reflecting...", ", ensuring...", ", fostering...". Cut the tail or make it a real sentence with a real fact.
4. **Promotional adjectives.** Banned: vibrant, breathtaking, stunning, nestled, boasts, renowned, must-visit, rich (figurative), groundbreaking (figurative), seamless, cutting-edge, world-class. Describe what the thing actually is or does.
5. **Weasel attributions.** "Experts argue", "industry reports suggest", "observers note". Name the source or drop the claim.
6. **Formulaic challenges/outlook sections.** "Despite these challenges... continues to thrive." Replace with specific facts and dates.

## Language patterns

7. **AI vocabulary.** High-suspicion words that co-occur in AI text: delve, crucial, pivotal, landscape (abstract), tapestry, testament, underscore, showcase, foster, garner, intricate, interplay, enduring, enhance, additionally, moreover, furthermore, notably. Not banned individually, but two in one paragraph means rewrite.
8. **Copula avoidance.** "serves as", "stands as", "functions as", "boasts", "features", "offers" where "is" or "has" works. Use is/are/has.
9. **Negative parallelisms.** "It's not just X, it's Y." "Not only... but also." Tailing fragments like "No setup. No accounts. No hassle." Rewrite as plain claims.
10. **Rule of three reflex.** Forced triads everywhere ("innovation, inspiration, and insights"). Use two items, or four, or one; only three when there really are three.
11. **Synonym cycling.** protagonist / main character / central figure / the hero in consecutive sentences. Repeat the natural word.
12. **False ranges.** "From X to Y" where X and Y are not a scale. List the actual items.
13. **Passive and subjectless fragments.** "No configuration needed." "Results are preserved automatically." Give the sentence a subject: "You do not need to configure anything."

## Style patterns

14. **Em dashes: absolute zero.** Never output U+2014 or U+2013 anywhere. Commas, periods, colons, parentheses.
15. **Boldface abuse.** Bolding phrases mid-sentence for fake emphasis. Bold almost nothing in prose.
16. **Header-colon bullet lists.** "- **Speed:** it is fast" repeated. Write prose, or use plain list items.
17. **Title Case Headings.** Use sentence case in headings.
18. **Emojis.** None in product copy, docs, or code comments unless the user's brand explicitly uses them (some do; ask).
19. **Curly quotes.** Use straight quotes in code and product copy.

## Communication artifacts

20. **Chatbot residue.** "I hope this helps", "Certainly!", "Great question", "Let me know if..." must never appear in product content or docs.
21. **Knowledge-cutoff hedges.** "While details are limited...", "as of my last update". Either state the sourced fact or say nothing.
22. **Sycophancy.** Product copy never grovels or over-praises the reader.

## Filler and hedging

23. **Filler.** "In order to" becomes "to". "Due to the fact that" becomes "because". "At this point in time" becomes "now". "Has the ability to" becomes "can". "It is important to note that" gets deleted.
24. **Hedge stacks.** "could potentially possibly" becomes one modal or none.
25. **Generic upbeat endings.** "The future looks bright", "exciting times ahead". End with a fact or a next step.
26. **Uniform hyphenation of common pairs.** Humans are inconsistent with things like "high quality" and "decision making"; do not hyphenate every compound with machine regularity. Technical compounds keep their hyphens.
27. **Authority tropes.** "The real question is", "at its core", "what really matters", "fundamentally". Just make the point.
28. **Signposting.** "Let's dive in", "here's what you need to know", "without further ado". Start with the content.
29. **Fragmented headers.** A heading followed by a one-line restatement of the heading. Cut the restatement.

## Writing that fits the product

- A boutique's copy talks about fabric, fit, and delivery days, not "empowering your wardrobe journey".
- A game's copy has energy and slang appropriate to its community.
- A legal-services page is calm and precise.
- Buttons say what they do ("Add to cart", "Track my order"), never "Get started" reflexively.
- Error messages are honest and helpful: what went wrong, what to do next, in the brand voice. "That code has expired. We sent you a fresh one" beats "An error occurred."
- Microcopy (tooltips, placeholders, confirmations) gets the same care; it is where users actually live.

## Legal pages exception

Terms, Privacy, and similar pages are deliberately dry and formal; that register is correct there. But even legal pages must be specific to this product (see legal-pages.md) and must contain zero em dashes.
