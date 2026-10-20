# Create.WitUS scaffold brief (for cloud sessions)

**Written:** 2026-10-15. **Why this file exists:** cloud sessions see only this repository. Every fact a session would otherwise read from a sibling repo or from gitignored `plans/` is restated here. Facts marked *(verify)* are to be checked against the named source before use; do not guess them.

## Stack (mirror of the newest ecosystem scaffold, `vogoat`, 2026-10-15)

- Next.js 16.2.x App Router, React 19.2.x, TypeScript strict, Tailwind. Package manager **pnpm** (`packageManager` field pinned).
- Drizzle ORM `^0.45` with `@neondatabase/serverless` `^1`; Neon Postgres, own instance, never shared (`DATABASE_URL`).
- `better-auth` `^1.6` with the `genericOAuth` plugin as the **only** sign-in method: `providerId: "witus"`, `discoveryUrl` from env `WITUS_OIDC_DISCOVERY_URL` (value: `https://accounts.witus.online/api/idp/.well-known/openid-configuration`), `clientId` from `WITUS_OIDC_CLIENT_ID` (value `witus-create`), `clientSecret` from `WITUS_OIDC_CLIENT_SECRET`. Callback path: `/api/auth/oauth2/callback/witus`. No local password store, no magic link in production.
- Zod `^4` on every server action and route handler. Authorization checked server-side per row.
- Mail: **Mailgun** (the ecosystem mailer; the PRD's "Resend" is superseded). Env names as used by witus-inbox: `MAILGUN_API_KEY`, `MAILGUN_DOMAIN`. Sender `noreply@create.witus.online`.
- Uploads: Cloudinary **signed** uploads only: `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`, folder `create-witus/`.
- Bot gate: Cloudflare Turnstile on the public application form: `TURNSTILE_SITE_KEY`, `TURNSTILE_SECRET_KEY`.
- Analytics: PostHog per the ecosystem standard (copy `lib/analytics/` from the witus-online repo unchanged; `autocapture: false`, `persistence: "memory"`, no `identify()`, `register({ app: "create" })`, `api_host: "/ingest"` with the next.config rewrite to `https://us.i.posthog.com`, `NEXT_PUBLIC_POSTHOG_HOST` set explicitly, no session replay).
- Errors: Sentry with the ecosystem scrubber pattern *(verify against a sibling repo's `lib/sentry` before copying)*.
- CI: `.github/workflows/ci.yml` modeled on witus-inbox (checkout@v4, setup-node@v4 Node 24, install from lockfile, `tsc --noEmit`, lint, test, build; stub env vars only where `next build` needs them, each commented as a CI stub). `.githooks/pre-commit` refuses commits on `main`; each clone activates it with `git config core.hooksPath .githooks`.

## Schema additions to PRD section 6 (decided 2026-10-15)

- `chapter`: `id`, `slug` (unique), `name`, `region`, `country`, `timezone`, `operator_org_id` (nullable in v1), `status` (`active|paused`), `created_at`.
- `chapter_membership`: `user_id`, `chapter_id`, `role` (`member|moderator|operator`), `created_at`; unique (`user_id`, `chapter_id`).
- `chapter_id` (not null) on `profile`, `call`, `application`, `report`. Browse and calls are chapter-scoped by default with an "all chapters" toggle for `remote` calls.
- `profile.kind` (`person|organization`), `profile.is_public_listing` (bool, default false), `profile.accepts_inquiries` (bool, default false). An inquiry may create a `thread` only when `accepts_inquiries` is true; otherwise `thread` still requires an accepted `call_interest` (PRD rule 1). Extend `thread` so that exactly one of `call_interest_id` or `inquiry_id` is set, with a check constraint.
- Chapter 1 seed row: slug `indiana`, name `Indiana`, region `Indiana`, country `US`, timezone `America/Indiana/Indianapolis`.

## Discipline taxonomy (fixed list plus `other`), from the IFN directory's departments

`full_service_production`, `production`, `camera`, `art`, `lighting_electric`, `sound`, `special_effects`, `transportation`, `equipment`, `unions`, `support_services`, `talent`, `post_production`, `music`, `resources_facilities`, `other`. Store as a Postgres enum used by `profile.disciplines` (array) and `call.disciplines_needed` (array). Labels for the UI use the directory's wording ("Lighting & Electric", "Post-Production Service", "Resource/Facilities").

## Invariants a test must assert

1. `thread.call_interest_id` is unique and, when `inquiry_id` is null, not null.
2. `call.expires_at` is not null; expired calls reject new interest.
3. Every `profile`, `call`, `application`, `report` row has a `chapter_id`.
4. A blocked pair never appears in each other's queries (filtering at the query layer).
5. No row in `call` carries a price, venue id, or place id.

## Out of scope for every session unless the brief changes

Importing any listing from the printed directory; a map; booking; payments; ratings; under-18 access; per-chapter custom domains.
