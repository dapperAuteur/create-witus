# Create.WitUS

**A collaboration call board for creators, makers, STEAM folks, and artists.**

Post what you want to make and who you need. Answer someone else's call. Go make the thing.

`create.witus.online` · part of the [WitUS](https://witus.online) ecosystem.

---

## Status

**Build brief. No application code yet.**

Read in this order:

1. [`docs/01-prd.md`](docs/01-prd.md) — the PRD. What v1 is, what it explicitly is not, the data
   model, and the flows.
2. [`docs/code-of-conduct.md`](docs/code-of-conduct.md) — the community rules members accept.
3. [`docs/trust-and-safety.md`](docs/trust-and-safety.md) — the moderation playbook.
4. [`docs/coop-phase-2.md`](docs/coop-phase-2.md) — the co-op, **not being built yet**.
5. [`CLAUDE.md`](CLAUDE.md) — repo identity, invariants, stack.

Operator actions BAM needs to take live in
[`plans/user-tasks/00-descriptions.md`](plans/user-tasks/00-descriptions.md).

## What it is

The unit is the **call**, not the person:

> *"Need a sign painter. 3-week mural, Osu. Starts Aug 4. Paid."*
> *"Welder, ~4 hours, this Saturday. Steel armature. Barter."*
> *"Co-writing a track. Remote, ongoing. Split."*

Approved members browse and filter calls, express interest with a note, and the author picks
who to talk to. Profiles show **work samples** — what you make and how you talk about it — not
photos of your face.

## What it is not

Not a swipe deck of people. Not a dating app. Not a booking system. Not a payments platform.
Not a directory of studios. Not a source of tools, space, or insurance. Not an employer or
agency. Not a party to any collaboration.

## Stack

Next.js 16 (App Router) · Neon Postgres + Drizzle · Better Auth as a "Sign in with WitUS" OIDC
client · Tailwind v4 · Cloudinary (signed uploads) · Resend · Turnstile.

## Contributing

Branch → commit → push → stop. BAM merges. Activate the guard once per clone:

```sh
git config core.hooksPath .githooks
```
