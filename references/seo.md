# SEO and GEO law: findable by search engines and by AI

This applies to any product where discovery matters (marketplaces, content sites, blogs, local businesses). Internal tools and logged-in-only apps can skip most of this; ask in Phase 1 Batch I. Two audiences now find sites: search engines (Google) and AI answer engines (ChatGPT, Perplexity, Google AI Overviews, Claude). Build for both from Phase 4, not bolted on after launch.

## Technical foundation (build into every page, not a Phase 7 afterthought)

- [ ] Semantic HTML: one `<h1>` per page, a real heading hierarchy (h2 under h1, h3 under h2), landmark elements (`<nav>`, `<main>`, `<footer>`). This is also an accessibility requirement, the two overlap.
- [ ] Every page has a unique, specific `<title>` and meta description written for a human, not stuffed with keywords.
- [ ] Canonical URLs set, especially where the same content is reachable multiple ways (with/without trailing slash, query params, filters).
- [ ] `sitemap.xml` generated and submitted; `robots.txt` present and correct, allowing the pages meant to be found and blocking admin/internal routes.
- [ ] Structured data (schema.org via JSON-LD) for what the page actually is: Product, Article, LocalBusiness, FAQ, Review. This is what earns rich results, not a guess, it must match the visible content exactly.
- [ ] Open Graph and Twitter Card meta tags on every page so shared links preview correctly (title, description, image). Covered in design-rules.md for the asset side; this is the tag side.
- [ ] Core Web Vitals pass on mobile (see design-rules.md performance section and scale.md); a slow site is a demoted site regardless of content quality.
- [ ] Content is server-rendered or statically generated where discovery matters. Content that only appears after client-side JavaScript runs is invisible to crawlers that do not execute JS well, including most AI crawlers.
- [ ] No content behind a login wall that is meant to be discovered publicly.

## Generative engine optimization (being found by AI answers, not just search results)

- [ ] `robots.txt` explicitly allows the AI crawlers this product wants to be discovered by (GPTBot, ClaudeBot, PerplexityBot, Google-Extended), unless the user deliberately wants to opt out; ask which in Phase 1, do not decide silently either way.
- [ ] Content leads with a direct, quotable answer before elaborating. AI answer engines pull the clearest sentence, not the most elegant paragraph; write the first line of each section as something that could stand alone as a citation.
- [ ] Consider an `llms.txt` file at the site root: a short markdown map of the site's most important pages with one-line descriptions, grouped under headings like `## Docs`, `## Product`, `## Blog`. This is an emerging, not universally adopted, convention; mention to the user that major AI crawlers do not officially confirm using it yet, but it costs almost nothing to add and helps agentic browsers that do respect it.
- [ ] Keep cornerstone content dated and actually updated; AI engines weigh recency, and a page that visibly has not changed since launch loses ground to fresher competitors over time.

## Content and structure

- [ ] Internal linking between related pages (product to category, article to related articles) so both crawlers and users can navigate the site's actual structure.
- [ ] Image alt text that describes the image, doubling as accessibility and image-search discovery.
- [ ] URL slugs are readable words, not database IDs, where the page is meant to be found publicly.
- [ ] For local businesses: NAP (name, address, phone) consistent across the site and matched to any Google Business Profile the user has.

## What to tell the user honestly

State plainly that no build guarantees ranking; search and AI-answer placement depend on ongoing content, backlinks, and competition, none of which a one-time build controls. The honest claim is "the technical foundation is correct, nothing here will hold discovery back", not "this will rank on page one."
