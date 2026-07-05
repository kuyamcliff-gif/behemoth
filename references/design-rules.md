# Design law: nothing generic ships

The goal is a product that a stranger could not identify as AI-built. Every default instinct an AI has for design is a tell. Fight all of them.

## The banned list (instant tells of AI design)

- Purple-to-blue gradient hero with centered white text and two pill buttons.
- Everything wrapped in rounded boxes with soft shadows, floating on a pale gray background.
- Three feature cards in a row, each with an icon, a bold title, and two lines of text.
- Emoji as bullet points or in headings.
- Inter/Poppins/defaults on white with #6366F1 accents. (These fonts are fine when chosen deliberately for a reason; they are banned as a reflex.)
- Stock illustration people high-fiving in flat corporate style.
- Placeholder-feeling copy ("Empower your workflow", "Built for the modern team").
- Static pages where nothing responds to the user at all.
- Identical border radius, identical spacing, identical card treatment on every element (the "everything is a component from the same kit" look).
- Fake numbers ("Trusted by 10,000+ users") on a product with zero users. Never fabricate social proof.

## The required process

1. **Research before designing.** Search for the actual best sites in this exact niche (not "best websites 2025" but "best clothing boutique websites", "best indie game landing pages"). Study 3 to 5. Note what they do with layout, type, color, motion, photography. Present them to the user in Phase 1 Batch G.
2. **Pick one distinctive direction** and commit. A design with a point of view beats a safe design every time. The direction should be explainable in one sentence ("editorial and photography-led, like a fashion magazine" or "dense, fast, keyboard-driven, like a trading terminal").
3. **Type does the heavy lifting.** Choose fonts that carry the brand personality. Show the user rendered examples. Consider a distinctive display face for headings paired with a workhorse for body. Variable fonts open motion possibilities (weight shifts on scroll or hover) that feel premium.
4. **Color with intention.** Build the palette from the brand and the niche research, not from a default. Define it as tokens (background, surface, text, accent, danger) so dark mode and theming are cheap.
5. **Layout breaks the grid somewhere.** At least one moment per key page where the layout does something memorable: an oversized image bleeding off-canvas, asymmetric columns, text overlapping media, a horizontal scroll section. Not chaos; one signature move, repeated as a motif.

## Motion and interaction (this is where "alive" happens)

- Micro-interactions on everything clickable: hover states that mean something, press feedback, focus rings that match the brand.
- Scroll-reactive media where the user chose a lively feel: images that scale or reveal as they enter, videos that grow toward full-bleed as you scroll into them, sections that pin briefly while content advances. Use CSS scroll-driven animations or a small library (GSAP ScrollTrigger, Framer Motion); keep it 60fps and respect prefers-reduced-motion always.
- Page transitions consistent with the brand mood chosen in the interview.
- Skeleton screens or meaningful loaders, never spinners in empty white voids.
- Error and empty states designed with the same care as the hero. An empty cart, a 404, a failed payment: these are moments users actually see.

## Interactive 3D (when the user wants it)

Do not hand-model programmer art. Source real, licensed, interactive pieces:
- Search open libraries: Sketchfab (CC-licensed downloads), Poly Haven, Spline community files, Market.pmnd.rs for ready react-three-fiber components.
- Verify the license permits commercial use; credit when required.
- Tweak materials, lighting, and colors to the brand. Wire real interactivity: cursor-follow, drag-to-rotate, scroll-linked animation states.
- Always ship a static fallback for low-power devices and honor prefers-reduced-motion.

## Assets

- Brand/social icons: Simple Icons (simpleicons.org) or the platform's official brand kit. Never redraw the TikTok or Google logo by hand; brand guidelines forbid altered marks anyway.
- UI icons: one coherent set (Lucide, Phosphor, Heroicons), one weight, used consistently.
- Photography: real product photos from the user whenever the product is physical. Otherwise curated stock (Unsplash/Pexels, chosen to look non-stocky: candid, imperfect, specific) or AI-generated imagery that the user approves image by image. Consistent grading across all photos so they feel like one shoot.
- Favicons, OG images, and app icons are part of the build, not an afterthought. Every page gets correct meta and social preview.

## Performance is a design feature

- Images in modern formats (WebP/AVIF), sized responsively, lazy-loaded below the fold.
- Fonts subset and preloaded; no flash of invisible text.
- Ship the least JavaScript that achieves the chosen feel. Lively does not mean heavy.
- Test against Core Web Vitals before calling design done. Mobile first: most traffic is a phone.

## Accessibility is not optional

- Real contrast ratios (WCAG AA minimum), visible focus states, semantic HTML, alt text that describes, forms with labels, keyboard paths through every flow. An inaccessible site is a broken site.
