# Create.WitUS — v1 PRD

**Status:** approved, not yet built · **Owner:** BAM · **Last updated:** 2026-07-22

> Tracked in `docs/` rather than `plans/` because `plans/` is gitignored ecosystem-wide and
> this document must survive a fresh clone. Local working notes still live in `plans/`.

---

## 1. Context

Creators, makers, STEAM folks, and artists want to find each other and **actually make things
together** — last-minute, planned, and everything in between.

The seed idea was "Tinder/Bumble for creatives," working title *mytenderoni*: profiles showing
*what you make* and *how you talk about it* instead of photos of your face, plus a self-declared
level (professional / hobby / student / when-I-feel-like-it). The longer-term ambition is a
**co-op** — community moderation, member access to tools and studio time at member rates, and a
map of studios, event spaces, and galleries.

Four critiques reshaped the scope before anything was written:

**1. Swipe-on-people is the wrong mechanic.** "Am I attracted to this face" is a half-second
binary judgement. "Do I want to spend six weekends building a thing with this person" is not the
same cognitive task, and a card stack cannot support it. A swipe deck of creatives produces a
high match count and near-zero collaborations — a portfolio-browsing app with a vanity counter.
→ **The atomic unit is the project call, not the person.**

**2. The co-op is a different company than the app.** Member rates on tools and studio time
require a legal entity in a specific jurisdiction, membership economics, general liability
insurance, participant waivers, and a signed agreement with *every* space whose discount is
advertised. You cannot promise a member rate at a studio that has not agreed — that is the
ecosystem authoritative-values rule applied to the physical world. → **v1 is app-only. The co-op
is phase 2: designed for, not built.**

**3. Cold start kills matching products.** A collaborator-matching app with forty users in one
city is dead on arrival. Density is the product. → **Waitlist plus manual batch approval**, so
the first cohort is coherent rather than one person per discipline.

**4. Safety is a requirement, not a feature.** Strangers meeting to make things — often in
isolated spaces, often at night, sometimes around power tools, sometimes with money involved —
is materially higher exposure than a coffee date. "Show your work, not your face" reduces
appearance-based *selection*; it does **not** reduce safety exposure, and a show-and-tell video
reveals face, voice, and often location anyway. → **v1 brokers no space and no tools, and a
stranger cannot initiate contact.**

**On the name:** "tenderoni" is romantic/sexual slang. Given that the top product risk is
*becoming a dating app that occasionally makes art*, the working title was actively recruiting
the failure mode. Dropped.

## 2. The one job

**Help creators find other creators to make a specific thing with, and get out of the way.**

## 3. Decisions locked

| Decision | Choice |
|---|---|
| Core loop | **Project calls** — time-bound asks. Profiles support calls, not the reverse. |
| Build order | **App first, co-op later.** |
| Geography | **Remote-first.** No physical access, tools, or space brokered. |
| Placement | WitUS product, own repo + subdomain, OIDC client of `accounts.witus.online`. |
| Name | **Create.WitUS** · `create.witus.online` · repo `create-witus` |
| Call scope | **Location-tagged, WitUS brokers nothing.** `mode: remote / in_person / hybrid`, free-text place. No venue records, no booking, no discounts. |
| Map | **Not in v1.** `place` table defined but unpopulated and unexposed. |
| Trust floor | **Waitlist + manual approval** by BAM, in batches. |
| Minimum profile | Text + **3 work samples**. Video optional. |
| Level semantics | **Social signal only.** Gates nothing. |
| Age | **18+ only.** |
| Ban authority | **BAM alone** in v1. |
| Monetization | **None in v1.** No payments, no dues, no fees. |

### 3.1 Decisions made without being asked — flagged for override

These were judgement calls, not requirements from BAM. Each is defensible; each is reversible
by saying so.

- **18+ only.** "Student" as a level strongly implies people under 18. Mixing minors with adult
  strangers who meet to make things is a categorically different product carrying safeguarding
  obligations, and it is not a config flag. v1 attests 18+ at application.
- **Level gates nothing.** The moment "professional" unlocks unsupervised access to a CNC or a
  kiln, self-declaration becomes a liability event. Stated in UI copy, not just in docs.
- **BAM holds sole ban authority in v1.** Community moderation is the phase-2 target with named
  roles, but volunteer moderators do not absorb legal liability, and there is no community to
  moderate yet.
- **Video optional.** Demanding a 60-second show-and-tell before a user has a single match is
  where signups die. A photo is one tap; a video is a production.

## 4. Scope

### 4.1 In scope for v1

Application + manual approval · member profiles with work samples · post / browse / filter
calls · express interest · author accepts, which opens a thread · in-thread messaging · report ·
block · admin triage queue · Sign in with WitUS.

### 4.2 Explicitly out of scope

A swipe deck of people · a booking system · payments or escrow · a studio/gallery/space
directory or map · tools, space, or insurance · employment or agency services · identity
verification or background checks · public profiles indexed by search engines · under-18 access ·
group/team accounts · reviews or ratings of members.

**Create.WitUS is not a party to any collaboration.** This is stated in the ToS in plain
language, not buried.

### 4.3 Ecosystem redundancy check

Per the Redundancy Test in `gemini/witus/plans/ecosystem/README.md`:

| Adjacent platform | Owns | Why Create.WitUS is not it |
|---|---|---|
| Work.WitUS | Paid contractor jobs, invoices, venues, contacts | Create.WitUS has no invoicing and no venue records; peers, not client work |
| Tour Manager OS | Touring crew, shows, advances | Different domain, different lifecycle |
| RideWitUS | Bike-mechanic community + curriculum | Single-discipline and curriculum-led |
| Learn.WitUS | Multi-tenant LMS | Create.WitUS teaches nothing |
| Stream.WitUS | Media tracking + podcasts | No overlap |

If a call turns into paid client work, the right answer is a **link to Work.WitUS**, not an
invoicing feature here.

## 5. The two design rules that carry the safety model

**Rule 1 — No unsolicited messages, ever.** There is no path to message a stranger. A `thread`
row may be created **only** as a consequence of a `call_interest` transitioning to `accepted`,
performed by the author of that call. This single constraint removes most of the harassment and
dating-drift vector without any moderation labor. Any "message this person" button is a defect.

**Rule 2 — Calls expire.** Every call carries `expires_at`. Expired calls leave the feed and
reject new interest. The feed self-cleans, urgency is real, and a dead product looks dead
instead of looking stale-but-alive.

Profile browsing is a **search with filters**, never a card stack — deliberately less compelling
than swiping, because compelling person-browsing *is* the failure mode.

## 6. Data model

Neon Postgres (own instance, never shared) + Drizzle ORM.

### 6.1 Auth
`user` · `session` · `account` · `verification` — Better Auth standard tables. No local password
store; identity comes from `accounts.witus.online`.

### 6.2 Membership

**`application`** — the waitlist.
`id` · `user_id` · `answers` (jsonb) · `attests_18_plus` (bool, must be true) ·
`coc_version_accepted` (text) · `status` (`pending|approved|rejected`) · `reviewed_by` ·
`reviewed_at` · `reviewer_note` · `created_at`

**`profile`**
`id` · `user_id` (unique) · `handle` (unique, citext) · `display_name` · `bio` ·
`disciplines` (text[]) · `level` (`professional|hobby|student|when_i_feel_like_it`) ·
`place_label` (free text, e.g. "Osu, Accra") · `timezone` · `availability` · `links` (jsonb) ·
`avatar_url` (nullable, optional and de-emphasized in UI) ·
`status` (`pending|approved|suspended`) · `created_at` · `updated_at`

**`work_sample`**
`id` · `profile_id` · `kind` (`image|video|audio|link|text`) · `cloudinary_public_id` (nullable
for `link`/`text`) · `url` (nullable) · `body` (nullable, for `text`) · `caption` · `position`

### 6.3 The core loop

**`call`** — the atomic unit.
`id` · `author_id` · `title` · `pitch` · `mode` (`remote|in_person|hybrid`) ·
`place_label` (free text, nullable when `remote`) · `disciplines_needed` (text[]) ·
`commitment` (`one_off|short|ongoing`) · `comp_type` (`unpaid|paid|split|barter`) ·
`starts_at` (nullable) · `ends_at` (nullable) · `expires_at` (**not null**) ·
`status` (`draft|open|filled|expired|removed`) · `created_at` · `updated_at`

> **No `venue_id`, no `place_id`, no booking, no price field.** Invariant 4.

**`call_interest`**
`id` · `call_id` · `user_id` · `note` · `status` (`sent|shortlisted|declined|accepted`) ·
`created_at` · **unique (`call_id`, `user_id`)**

**`thread`** — `id` · `call_id` · `call_interest_id` (unique, **not null**) · `created_at`
The not-null unique FK to `call_interest` is what makes Rule 1 enforceable in the schema, not
just in application code.

**`message`** — `id` · `thread_id` · `sender_id` · `body` · `created_at` · `redacted_at`

### 6.4 Safety

**`report`** — `id` · `reporter_id` · `subject_type` (`profile|call|message`) · `subject_id` ·
`reason` · `detail` · `status` (`open|actioned|dismissed`) · `resolution` · `handled_by` ·
`created_at` · `handled_at`

**`block`** — `id` · `blocker_id` · `blocked_id` · `created_at` · unique pair.
Effect is **symmetric**: each disappears from the other's feeds, search results, and call lists,
regardless of who blocked whom.

**`moderation_action`** — append-only audit. `id` · `actor_id` · `subject_type` · `subject_id` ·
`action` (`warn|remove_content|suspend|ban`) · `reason` · `notes` · `created_at`. Never updated,
never deleted.

### 6.5 Reserved — defined, unpopulated, unexposed in v1

**`place`** — `id` · `name` · `kind` (`studio|gallery|event_space|makerspace|shop`) · `address` ·
`geo` · `source` · `verified_at`

Exists so the phase-2 map does not force a migration of `call`. **Do not populate it, do not
expose it, do not link it from `call` in v1.** See `docs/coop-phase-2.md`.

## 7. Core flows

1. **Apply** — public, Turnstile-gated form. Profile draft + **≥3 work samples** + 18+
   attestation + Code of Conduct acceptance (version recorded) → `application.status = pending`.
2. **Approve** — BAM reviews a **batch** in `/admin` → `profile.status = approved` → welcome
   email via Mailgun from `noreply@create.witus.online`.
3. **Post a call** — approved members only. Rate-limited. Requires `expires_at`.
4. **Browse** — filter by discipline, mode, commitment, comp type, starts-within. Blocked users
   filtered at the **query layer**, not in the UI.
5. **Express interest** — "I'm in" plus a note. Rate-limited. **No thread is created.**
6. **Accept** — the call author accepts an interest → thread opens → both can message. On an
   `in_person` or `hybrid` call, a one-time meeting-safety interstitial is shown before contact
   details can be exchanged.
7. **Report / block** — on every profile, call, and message. Lands in `/admin` triage.

## 8. Trust, safety, and community rules

Non-negotiable at launch. Detail in [`trust-and-safety.md`](trust-and-safety.md) and
[`code-of-conduct.md`](code-of-conduct.md).

- Report + block on every surface, ≤2 taps, from the first deploy.
- Rate limits on application, call creation, interest sending, and messaging.
- Versioned Code of Conduct accepted at application; re-acceptance on major version bump.
- Trust & safety playbook: triage SLA, escalation ladder, decision authority, incident response.
- ToS states plainly that WitUS provides no space, tools, insurance, employment, payment, or
  vetting, and is not a party to any collaboration.
- Work samples reviewed at application. Post-approval uploads are flagged-on-report rather than
  pre-moderated — correct at v1 volume, with the revisit threshold named in the playbook.

### Security

OIDC via `accounts.witus.online` (no local password store) · Zod on every server action and
route handler · authorization checked server-side **per row**, never client-gated · Cloudinary
**signed** uploads only · Turnstile on the public application form · no PII solicited in call
bodies · `noindex` on profiles and threads in v1.

## 9. Success measures

Vanity metrics are actively misleading here. Track the ones that describe whether people
**made things**:

| Measure | Why |
|---|---|
| Calls that reach ≥1 accepted interest | The actual conversion event |
| Median time from call posted → first interest | Density proxy; the number that dies first when the product is too small |
| Self-reported "we made the thing" on call close | The only real outcome. Ask on expiry/close. |
| Repeat call authors | Whether it worked well enough to come back |
| Reports per 100 active members | Safety load, and the leading indicator of dating drift |
| Approval queue latency | BAM's own bottleneck |

Explicitly **not** tracked as a success measure: total signups, total matches, DAU, session
length. Session length going *up* would be a warning sign, not a win.

### Kill criteria

If, 90 days after the first approved cohort, the median time from call posted → first interest
exceeds one week, the density problem is unsolved and the answer is a narrower seed cohort — not
more features.

## 10. Phase 2 and beyond — not being built

The co-op, member rates, tool and studio access, the studios/galleries/event-spaces map, and
community moderation with elected roles. See [`coop-phase-2.md`](coop-phase-2.md), which is
mostly a list of questions humans must answer before any code is written.

## 11. Open questions for BAM

1. **Seed cohort** — *which specific ~30 people* get the first invitations? Density is the
   product, and this is the highest-leverage unanswered question in the document.
2. **Discipline taxonomy** — fixed list, freeform tags, or fixed list plus "other"? A fixed list
   makes filtering work and makes the product feel narrow; freeform tags do the opposite.
3. **Public surface** — should calls be readable while logged out for discovery and SEO, or is
   everything behind approval? Current assumption: **everything behind approval, `noindex`**.
4. **Handle vs. real name** — pseudonymous handles are safer for members; real names are better
   for accountability. Current assumption: **handle required, real name optional and private**.
5. **Call expiry default** — 30 days? Shorter for `one_off`?
