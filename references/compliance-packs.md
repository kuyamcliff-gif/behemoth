# Compliance packs: the right extra checklist, attached automatically

references/security.md is the generic audit every product gets. Some products carry extra legal and regulatory weight because of what they do or who they serve. Detect the archetype from the Phase 1 answers in plan.md and attach every pack that applies before Phase 5 runs; do not wait for the user to know to ask for it by name.

## How detection works

Read plan.md for signals, not just an explicit "we are a fintech" statement:

- Handles payments beyond a standard checkout (holds balances, issues payouts, processes cards directly rather than through provider-hosted fields) → **Payments pack**.
- Collects or stores health information, symptoms, diagnoses, or connects to a clinical workflow → **Health pack**.
- Serves children, or an underage user is plausible for this product's audience → **Minors pack**.
- Has users in the EU → **GDPR**. Has users in California → **CCPA/CPRA**. Both can apply at once.
- Includes an embedded AI agent making decisions that affect a user (recommendations, approvals, moderation, hiring-adjacent screening) → **AI-agent pack**.
- Automates a decision about employment, housing, credit, or another consequential life outcome → **Automated-decision pack**.

A product can carry more than one pack; attach every one that applies.

## Payments pack

Beyond security.md's webhook/idempotency items: PCI DSS scope (stay in SAQ-A by using provider-hosted card fields, never touch raw card numbers server-side), clear settlement and payout timing disclosure, chargeback and dispute handling documented, and SOC 2-style access controls if the user's own customers will ask about it (common once B2B customers appear).

## Health pack

HIPAA-equivalent handling if serving US users: business associate agreements with every vendor that touches the data (hosting, email, analytics must all be checked, not assumed compliant), encryption at rest and in transit treated as mandatory not optional, audit logs on every access to a health record, and a plain flag to the user that actual medical device functionality (diagnosis, treatment recommendations) may trigger FDA Software as a Medical Device (SaMD) requirements, which need a lawyer, not this skill.

## Minors pack

Age-appropriate design: no behavioral advertising targeting a known-minor account, parental consent flow for any account below the applicable regime's threshold (COPPA-style in the US), no dark patterns steering minors toward purchases, and heavier moderation defaults on anything user-generated they can see.

## GDPR / CCPA (data protection packs)

Already covered in legal-pages.md's Privacy Policy section; the pack here means the security.md data-care items get read literally as legal obligations, not best practice: deletion must actually delete, export must actually export, and the subprocessor list must be current.

## AI-agent pack

Beyond security.md's product-AI-agent checklist: log every consequential decision the agent makes with enough detail to explain it after the fact, a visible human escalation path tested end to end, and a plain disclosure to the user-facing audience that they are talking to an AI where the applicable region requires it (several jurisdictions now do).

## Automated-decision pack

Document the decision logic in plain terms (what inputs drive the outcome), give affected people a way to ask for human review, and check whether the user's jurisdiction has a specific automated-decision audit law (for example, employment-decision tools facing New York City users trigger a bias-audit requirement under local law); search for the current requirement, do not assume it does not apply.

## Closing note

None of this substitutes for a real lawyer once a pack applies; say so plainly, the same way legal-pages.md already does. The pack's job is to make sure the technical and product work does not contradict a legal obligation the user did not know they had.
