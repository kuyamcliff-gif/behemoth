# Legal pages: boring on purpose, specific in fact

Every product with users needs them: Terms of Service, Privacy Policy, and usually Cookie notice and Refund policy. Their register is dry, formal, and thorough. That is correct; do not make legal pages fun. But dry does not mean generic: every clause must describe this product's actual behavior.

Behemoth is not a law firm and must say so: include a line in HANDOFF.md advising the user to have a local lawyer review before relying on these pages, especially for regulated niches (health, finance, kids).

## Ground rules

- Long and thorough beats short and cute. Standard section ordering, numbered clauses, defined terms.
- Zero em dashes, like everything else.
- Every clause must be true. If the Terms say "we may suspend accounts that violate section 4", the admin dashboard must actually have suspension. If the Privacy page says "you can delete your data", deletion must work. Write the pages after the features exist, or write them as the spec the features must meet; never let them drift apart.
- Fill in real specifics: the product's legal name, contact email, jurisdiction (asked in the interview or inferred from the user's region and confirmed), the actual data collected, the actual third parties used (payment provider, analytics, hosting), the actual retention behavior.
- Search current requirements for the user's market before writing: GDPR if serving the EU, CCPA/CPRA for California traffic, local consumer law for the user's country (for example OHADA-region consumer rules for Central/West African businesses). Note in the page which regimes it addresses.

## Terms of Service must cover

1. Definitions and acceptance.
2. The service description (what this product actually does, in formal register).
3. Accounts: eligibility, age minimum, accuracy of information, one account per person if applicable, credential responsibility.
4. Acceptable use: the forbidden-behavior list from interview Batch E, written formally.
5. Enforcement: the exact escalation ladder the user chose (warning, suspension, ban, content removal, payout hold), who decides, and whether appeal exists. This section and the admin tools must match exactly.
6. Payments: prices, billing cycle, taxes, the chosen refund posture, chargebacks.
7. User content: ownership stays with the user, license granted to the product to host and display, takedown process.
8. Intellectual property of the product itself.
9. Third-party services disclaimer.
10. Termination by either side and what happens to data on termination.
11. Warranty disclaimer and limitation of liability (standard formal language).
12. Governing law and dispute venue (the confirmed jurisdiction).
13. Changes to terms and how users are notified.
14. Contact information.

## Privacy Policy must cover

1. Who the data controller is (real name, real contact).
2. Exactly what is collected, listed concretely: account email, order history, uploaded images, payment metadata (noting card numbers are held by the payment provider, not the product), IP and device data if logged, cookies used.
3. Why each category is collected (purpose limitation).
4. Third parties who receive data, by name: the actual hosting provider, payment provider, email provider, analytics tool. Link their policies. Keep this list current as a living subprocessor list; update it in the same commit that adds or removes a service.
5. Retention: how long, and what deletion does.
6. User rights for the applicable regimes (access, correction, deletion, export, objection) and the concrete way to exercise them (the settings page or the contact email).
7. Cookies: what is set, what is essential vs optional, and the consent mechanism if optional cookies exist for EU visitors.
8. Children: the age minimum and what happens if underage use is discovered.
9. Security statement (honest, modest: encryption in transit, restricted access, no absolute guarantees).
10. Changes and contact.

## Refund policy (if the product sells)

State the chosen posture from interview Batch D plainly, with the legally required minimums for the jurisdiction (searched, not assumed), the process to request, and the timeline to receive.

## Accessibility and disclosure

WCAG 2.1 AA (see design-rules.md) is a legal requirement in several markets, not a nice-to-have: the ADA in the US has been used in thousands of website lawsuits, and the EU's European Accessibility Act applies from 2025 to many digital products serving EU consumers. For any product with a real user base, mention this plainly to the user rather than treating accessibility as purely a design preference. A `security.txt` file (see security.md) for responsible vulnerability disclosure sits alongside the legal pages, same footer, same seriousness.

## Placement

Footer links on every page. Checkbox or clear notice at signup ("By creating an account you agree to the Terms and Privacy Policy" with links). Payment pages link the refund policy. Cookie banner only if non-essential cookies actually exist; do not add a consent banner to a site that sets none.
