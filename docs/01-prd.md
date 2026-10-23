# Create.WitUS — v1 PRD

**Status:** under revision, not built; section 0 overrides the rest where they conflict · **Owner:** BAM · **Last updated:** 2026-10-23

> Tracked in `docs/` rather than `plans/` because `plans/` is gitignored ecosystem-wide and
> this document must survive a fresh clone. Local working notes still live in `plans/`.

---

## 0. Direction under review (2026-10-23)

BAM changed the product's direction on 2026-10-23. This section records the new direction, the warnings from
best-in-class research, the open questions, and the mockups that come next. **No code is written until BAM picks a
mockup direction and this PRD is rewritten.** Where sections 1 to 11 conflict with this section, this section
wins; the conflicts are listed in 0.5.

### 0.1 The new direction

1. **No in-app messaging.** People talk outside the app. A member either shows contact details up front (to
   members, or to everyone) or receives a **contact request** and approves or declines it. On approval both sides
   get the contact details each chose to share, by email. This replaces threads and messages.
2. **Public profiles** that non-members can see, so members win business. The pilot keeps `noindex` and revisits.
3. **People and organizations:** studios, facilities, vendors and union locals are first-class profiles.
4. **Part co-op, part union, part community.** Partner with craft unions (for example IATSE and IBEW locals) so
   artists get work done with help from skilled union crafts people. Create.WitUS shows union members' work in
   public and sends a hire to the local's own office or referral process; it never acts as a hiring hall.
5. **Visual, project-first showcase** (Instagram or Behance quality): images and video of real work.
6. **Credits:** a project lists everyone who worked on it, people and organizations; each confirms, and the project
   then appears on every credited profile.
7. **Posts that generate work:** RFPs, crew calls, gigs and collaborations, posted beside the work.
8. **Chapters run by partners:** BAM assigns chapter operators who run their chapter; suspension and bans across
   chapters stay with BAM. Only BAM and chapter leaders may bulk upload members who opted in; uploaded members
   claim their profile by signing in with WitUS.
9. **Notifications by email** (Mailgun): interest, accept, contact request, approval, being credited. One opt-out.
10. **English only** until a partner can translate correctly.

### 0.2 Best-in-class examples and what to copy

From research on about 30 products, done 2026-10-23 (the full table and 76 references are kept outside the repo
in the local planning notes):

- **Showcase:** Instagram (App Store 4.7, about 30M ratings) and Behance (4.6). Project-first portfolios.
- **Credits:** Behance's two levels (a co-owner accepts and the project shows on both pages; a credited person is
  named and can be invited by email), Instagram Collabs' acceptance rule, and IMDb's evidence list (call sheets,
  contracts, end credits) used only when a credit is disputed. Every credit sent to a non-member is an invitation
  backed by real work.
- **Calls:** Backstage reviews best of the crew-call sites; keep responding free (its top complaint is paying to
  apply).
- **RFPs:** Dribbble project briefs and Behance proposals, structured and next to the portfolio.
- **Spaces and vendors:** Giggster (4.7), praised for fast human support.
- **Co-op economics:** Up & Go (a published 5% fee; members choose members) and Stocksy (member ownership).
- **The gap:** no product checked combines a visual showcase, accepted credits for people and organizations,
  calls and RFPs beside the work, union-friendly chapters, and contact by consent. Indiana's state production
  directory (Reel-Scout) has no portfolios, reviews or credits.

### 0.3 Warnings (from the reviews of products that got it wrong)

- **Contact by consent is untested.** No product checked uses "ask, owner approves". Nobody knows how often owners
  approve, how fast, or whether hirers wait. Measure approval rate and time to approve from day one and make them
  kill criteria.
- **Scams move to the victim's phone** when there is no in-app messaging. Verify anyone who posts calls or RFPs
  before their posts go live (Dribbble and Stage 32 reviews describe fake job posters).
- **Never charge to apply or to reveal contact details.** Bark and Thumbtack reviews describe fake leads and
  refused refunds; Backstage's worst reviews are about paying to apply.
- **No auto-renew traps and no email that cannot be stopped** (Mandy, Stage 32 reviews).
- **Human support, not AI-only replies** (Behance and Upwork reviews).
- **Credits need history and an appeal path**; IMDbPro reviews describe credits removed without notice.
- **Stay out of payments** until there is staff to handle disputes (Peerspace and Contra payout complaints).
- **Volunteer-only operations fail:** Ampled closed in 2023 citing burnout; Polywork closed in 2025 after raising
  $44.5M. Chapter operators need a light, paid or partner-backed load.
- **No AI filler work** in the showcase (ArtStation reviews).

### 0.4 Open questions for BAM

1. **Credits:** does a credit show only after the credited person confirms? (Recommended: yes, with a "pending"
   state visible only to the poster.)
2. **Union partnerships in the product:** a union local as an organization profile, plus "union craft" tags on
   members? (Recommended.) What does the local get: a members' showcase page, a referral link, reporting?
3. **Contact requests:** may non-members send one, or members only? What may each side share (email, phone,
   website)? Rate limits? Expiry of an unanswered request?
4. **Poster verification:** who verifies call and RFP posters (chapter operators?), and how (WitUS sign-in plus
   a reviewed profile, a business record, a reference)?
5. **RFP fields:** budget range, dates, location, union or non-union, deliverables, deadline, attachments?
6. **Co-op economics:** is Create.WitUS free, a membership fee, or a published fee on work won? Who owns it if it
   is a co-op?
7. **Bulk upload of opted-in members:** proof of consent per row; unclaimed profiles hidden until claimed; one
   claim email and one reminder; deletion after 60 days unclaimed; fields limited to name, email, disciplines,
   city and links; adults only. (All recommended.)
8. **Chapter operators:** paid, volunteer, or staffed by the partner organization? What is the smallest load that
   keeps a chapter healthy?
9. **Metrics:** what counts as "work got done" (a hire, a credit on a finished project, an RFP awarded)? The
   anonymous analytics cannot measure it, so it needs database events.

### 0.5 Mockups next, then the rewrite

Three directions, each shown on the same five screens (a profile with credits, a project page, a call or RFP, the
contact request and approval, an organization listing), plus a short DX panel (the data model and screens each
direction needs):

1. **Portfolio-first:** a visual feed of projects with credits; calls and RFPs one tap away.
2. **Work-first:** calls, gigs and RFPs on the front page; portfolios as proof.
3. **Directory-first:** a chapter directory of people, studios, vendors and union locals, with filters and the
   contact request up front.

BAM picks one or a mix. Then sections 1 to 11 are rewritten. Known conflicts to resolve in that rewrite: threads
and messages (sections 5, 6.3, 7, CLAUDE.md invariant 2); "no studio or group directories" (4.2) versus
organization profiles; "no import" versus bulk upload of opted-in members; "BAM decides alone" in
`docs/trust-and-safety.md` versus chapter operators; `noindex` everywhere (4.2, 8) versus public profiles; the
work-sample rule (three samples) versus crew, vendors and union crafts people.

### 0.6 Sources for this section

Adobe. (2025, August 29). *Instagram collab posts*. https://www.adobe.com/express/learn/blog/instagram-collab-posts
Backstage. (n.d.). *Backstage: Casting calls* [Mobile app]. App Store. https://apps.apple.com/us/app/backstage-casting/id1215380527
Giggster. (n.d.). *How much commission does Giggster take?* https://help.giggster.com/en/articles/2832062-how-much-commission-does-giggster-take
Houghton, B. (2023, October 23). Indie music fan funding platform Ampled to shut down. *Hypebot*. https://www.hypebot.com/indie-music-fan-funding-platform-ampled-to-shut-down/
IATSE Local 849. (n.d.). *Availability lists*. https://iatse849.com/membership/resources/tutorials/availability-lists
IMDb. (2026, April 28). *Adding filmography credits*. https://help.imdb.com/article/contribution/filmography-credits/adding-filmography-credits/GAB5B33MBLEG73CT
Instagram, Inc. (n.d.). *Instagram* [Mobile app]. App Store. https://apps.apple.com/us/app/instagram/id389801252
Trustpilot. (n.d.). *Bark reviews*. https://www.trustpilot.com/review/bark.com
Trustpilot. (n.d.). *Behance reviews*. https://www.trustpilot.com/review/behance.net
Trustpilot. (n.d.). *Dribbble reviews*. https://www.trustpilot.com/review/dribbble.com

All retrieved 2026-10-23.

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
   email via Resend from `noreply@create.witus.online`.
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
