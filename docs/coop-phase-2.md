# Phase 2 — the co-op

> ## ⛔ Do not build any of this yet.
>
> This document exists so v1 doesn't paint us into a corner, and so the questions that must be
> answered by **humans — a lawyer, an insurance broker, and the members themselves** — are
> written down instead of discovered late. It is mostly a list of questions, and that is the
> honest state of it.
>
> Nothing here is scheduled. Nothing here is designed. If you are an agent reading this while
> looking for work to do, **the answer is no** — go read [`01-prd.md`](01-prd.md).

---

## 1. The ambition

Create.WitUS becomes a **co-op**: members own it, members moderate it, and membership carries
real benefits — access to tools, studio time, and space at member rates, plus a map of studios,
event spaces, galleries, and makerspaces.

That is a good ambition. It is also **a different company than the app.**

## 2. Why it isn't v1

Advertising a member rate at a studio requires a signed agreement with that studio. Offering
access to tools puts us in the chain of liability when someone is injured. Selling membership
may create securities questions. Collecting dues creates tax and accounting obligations.
Electing moderators does not transfer the legal consequences of their decisions.

None of that is a software problem, and **none of it gets easier by shipping the software
first.** What shipping the app first *does* buy is evidence: whether people actually want to
collaborate through this thing at all. If v1 fails on density, the co-op questions never need
answering.

The build order was chosen deliberately: **app first, co-op second.**

## 3. Questions that must be answered before any code

Grouped by who answers them. Every one of these is an operator task, not an engineering task.

### 3.1 For a lawyer

1. **Which entity form, in which jurisdiction?** Worker co-op, consumer co-op, multi-stakeholder
   co-op, or a nonprofit with a membership program? Ghana and US state law differ substantially,
   and US co-op statutes are state-by-state. This answer determines nearly every other answer.
2. **Do member shares create a securities question?** If members buy in, in many jurisdictions
   that is a security unless an exemption applies.
3. **What is our relationship to a partner studio** — broker, reseller, affiliate, or referrer?
   Each carries different liability and different tax treatment.
4. **What waiver and assumption-of-risk language is actually enforceable** in the operating
   jurisdiction, and does it survive if we're seen as having arranged the meeting?
5. **Does brokering access to tools make us liable for injuries** on equipment we don't own,
   don't maintain, and don't supervise?
6. **What changes when money moves between members** through or alongside the platform?
7. **Member agreement, ToS, and privacy policy** — reviewed, not templated.

### 3.2 For an insurance broker

1. General liability for the entity.
2. Whether partner spaces' existing policies cover our members, or whether we need our own.
3. Whether directors-and-officers coverage is needed once there is an elected board.
4. What coverage, if any, exists for harm arising from a connection made on the platform.
5. What the insurer requires of us — waivers, incident logs, training records — because that
   drives schema and product requirements, not the other way around.

### 3.3 For BAM and the members

1. **Dues** — flat, sliding scale, or patronage-based? What does a student pay?
2. **What does membership actually get you** on day one, before any studio has signed?
3. **Governance** — one member one vote? A board? How are decisions made and how are they
   contested?
4. **How does moderation authority transfer** from BAM to members without transferring liability
   that cannot legally be delegated? (See §5.)
5. **What happens to the app if the co-op dissolves?** Who owns the data, the domain, the code?
6. **Is the co-op scoped to one city?** Member benefits are intensely local; a co-op spanning
   Accra and a US city is two co-ops.

### 3.4 For partner studios

1. What do they get? Fill rate on empty hours is the obvious pitch — is it enough?
2. Who sets the member rate, and who eats the discount?
3. Insurance, waivers, and supervision — theirs or ours?
4. Who is liable when a member damages equipment?
5. How is a listing corrected or removed, and how fast?

## 4. The map — and why it isn't v1

Mapping studios, event spaces, galleries, and makerspaces was part of the original ask. It is
deferred for three reasons:

1. **A directory rots.** Hours change, prices change, places close. Someone owns maintenance or
   it becomes confidently-stated misinformation within a year — which is worse than an empty
   map, because members will act on it.
2. **It's worthless without density.** A map's value is a function of how many members are near
   the pins. At forty members it is a decoration.
3. **A listing implies a relationship we don't have.** Members will reasonably read a pin as a
   recommendation or an arrangement. Until there is a signed agreement, there is neither.

**What v1 does to stay ready:** the `place` table is defined in the schema —
`name`, `kind` (`studio|gallery|event_space|makerspace|shop`), `address`, `geo`, `source`,
`verified_at` — and left **unpopulated, unexposed, and unlinked from `call`**. `call.mode`
already distinguishes `remote` / `in_person` / `hybrid`, and `call.place_label` already carries
free text. Adding real places later is an additive migration, not a rewrite.

**When it does get built,** the only version that supports a member-rate claim is a **partner
network with signed agreements**. A scraped or member-submitted directory can never carry a
discount, only an address.

## 5. Community moderation — the transition

v1 gives BAM sole ban authority ([`trust-and-safety.md`](trust-and-safety.md) §3). Moving to
community moderation is a governance change, not a feature flag.

**Don't start until all of these are true:**

- The entity exists and carries insurance.
- There are enough members that moderators are *peers* rather than strangers — a few hundred,
  not a few dozen.
- Reports arrive faster than one person can triage within the P1 target.
- There are members with enough tenure to have earned the role.

**Design constraints when it does happen:**

- **Moderators are compensated.** Free membership, studio credits, a stipend — something.
  Unpaid moderation is a burnout pipeline, and a burnt-out moderator is an unread queue.
- **Final ban authority stays with the entity**, not with individual moderators. Moderators
  recommend and act on P2/P3; P0 escalates.
- **Every action stays in the append-only `moderation_action` log**, including moderators'.
- **Moderators can be removed**, and the process for it is written down before the first one is
  appointed.
- **Two-moderator rule** for suspensions and bans, once there are enough moderators to have one.
- **Conflict-of-interest rule**: you don't moderate a report involving someone you're
  collaborating with. The schema knows who has an accepted `call_interest` with whom — enforce
  it in code, not on the honor system.

## 6. Design decisions already made that keep this door open

| v1 decision | Why it matters for phase 2 |
|---|---|
| `place` table defined but unused | Map is an additive migration |
| `call.mode` distinguishes remote / in-person / hybrid | In-person brokerage has a home |
| `moderation_action` is append-only with an `actor_id` | Multi-moderator audit works unchanged |
| Membership state on `profile.status` | Adding a dues/tier dimension is additive |
| No payments anywhere in v1 | We pick a payment model after we know the jurisdiction, not before |
| Level gates nothing | We never built an access-control system on self-declared skill |

## 7. Related

- [`01-prd.md`](01-prd.md) — v1 scope and the decisions this defers.
- [`trust-and-safety.md`](trust-and-safety.md) §3 — current moderation authority.
- `gemini/witus/plans/user-tasks/50-coop-legal-and-insurance-track.md` — the operator task
  tracking §3.1 and §3.2. **Marked non-blocking for v1**, deliberately.
