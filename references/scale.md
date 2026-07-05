# Scale law: it works at 100 users and does not collapse at 100,000

The most common failure of AI-built products: perfect demo, dead on arrival at real traffic. Pages slow, APIs stall, the database chokes. Scale is decided by the choices made on day one, not by panic upgrades later.

## Ask the honest question first

Phase 1 asks: dozens, thousands, or millions? Build for one tier above the answer. A hobby tool does not need Kafka. A marketplace with ambitions must not be welded to a single server. Over-engineering wastes the user's money; under-engineering wastes their launch.

## Day-one rules regardless of tier

1. **Stateless application layer.** No session data, no uploaded files, no counters stored on the app server's disk or memory. Sessions in the database or a token; files in object storage (S3/R2/Supabase storage); cache in Redis or the platform cache. This single rule is what makes "add another server" possible later without a rewrite.
2. **Right database for the data shape.** Structured records with relations and money involved: SQL (Postgres via Supabase/Neon/RDS). Unstructured, document-ish, or extreme write volume: NoSQL where it genuinely fits. Money and orders are always transactional SQL.
3. **Indexes on every queried column.** Every WHERE, JOIN, and ORDER BY column used in a real query gets an index at creation time, not after the first slowdown.
4. **No N+1 queries.** Fetch related data in one query with joins or batched selects. Audit for this in Phase 6; it is the most common invisible scale killer.
5. **Cache from day one.** At minimum: HTTP caching headers on everything static, CDN in front of assets, and application-level caching for expensive repeated reads (popular listings, homepage data). Every cache needs an invalidation story; write it down in context.md.
6. **CDN for all static assets.** Images, fonts, JS, CSS never served from the origin. Vercel/Netlify/Cloudflare give this free; on a VPS put Cloudflare in front.
7. **Rate limiting on day one** for auth endpoints, search, and anything expensive. This is both scale protection and security.
8. **Background jobs for slow work.** Email sending, image processing, report generation, webhooks: queue them (even a simple database-backed queue) so requests return fast.
9. **Pagination everywhere a list can grow.** No endpoint ever returns an unbounded list.
10. **Design for failure.** Retry with backoff on external calls, timeouts on everything, a circuit-breaker or graceful fallback for third-party outages, and user-facing fallback messages in the brand voice. Monitor errors and latency (Sentry plus the host's metrics is enough to start).

## Architecture by tier

**Tier 1: up to low thousands of users.** One well-structured monolith on a managed platform (Vercel/Railway/Fly) or a single VPS with Cloudflare in front. Managed Postgres. Object storage. This is correct and cheap; do not add microservices.

**Tier 2: tens to hundreds of thousands.** Same monolith, scaled horizontally: multiple app instances behind the platform's load balancer (possible because the app is stateless). Add Redis for cache and sessions. Read replicas if the database strains on reads. A real job queue (BullMQ, Celery). This tier is where day-one discipline pays off: reaching it should require configuration, not rewrites.

**Tier 3: millions.** Split the hottest paths into separate services only when metrics prove the need. Database sharding or moving specific tables to purpose-built stores. Multi-region if latency demands it. Do not build Tier 3 speculatively; build a monolith with clean internal boundaries (modular folders, one module per business capability) so extraction is surgery, not demolition.

## The honest scale statement (goes in HANDOFF.md)

Template: "This setup comfortably handles roughly [N] concurrent users / [M] requests per minute. The first bottleneck will be [component]. You will notice it as [symptom]. When that happens, do [specific upgrade], which costs about [price] and takes [effort]. The next bottleneck after that is [component 2]."

Fill it with real reasoning from the actual stack, not boilerplate. If load testing was done, cite the numbers; if not, say the estimate is analytical.

## Load checking in Phase 6

- Run concurrent request loops against the key endpoints locally or against a staging deploy (autocannon, k6, or a simple script). Even 200 concurrent requests reveals N+1 disasters and missing indexes.
- Check Core Web Vitals on the deployed site (PageSpeed Insights).
- Verify the cache actually hits (second request faster and served from cache headers).
- State plainly in the handoff what was tested and what was not.
