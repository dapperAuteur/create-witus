# Trust & Safety playbook — Create.WitUS

**Internal.** Not member-facing. The member-facing rules are
[`code-of-conduct.md`](code-of-conduct.md).

**Version 1.0** · for v1 (single operator, waitlist-gated, pre-co-op)

---

## 1. The posture

Create.WitUS is a place where **strangers arrange to meet and make things**, sometimes in
private spaces, sometimes at night, sometimes around dangerous equipment, sometimes with money
involved. That is a higher exposure profile than a coffee-date app, and it is much higher than a
portfolio site.

Two facts shape everything below:

1. **We verify nobody.** No ID checks, no background checks, no skill verification. Every safety
   claim we make must be consistent with that.
2. **We broker nothing.** No space, no tools, no insurance, no payment. We are not a party to any
   collaboration. Copy that implies otherwise creates a duty of care we cannot fulfill — treat
   it as a bug, not a marketing choice.

## 2. What the product design already does for us

Most trust-and-safety load is avoided in the architecture rather than absorbed by moderators:

| Design rule | Threat it removes |
|---|---|
| No unsolicited messages — a thread requires an **accepted** `call_interest` | Cold-DM harassment, mass solicitation, dating-app drift |
| Calls expire | Stale bait listings; abandoned surface area |
| Profile browsing is filtered search, not a card stack | Appearance-driven selection and compulsive person-browsing |
| Waitlist + manual approval | Bulk spam accounts; incoherent first cohort |
| No payments, no booking, no venue records | Payment fraud, chargeback abuse, premises liability |
| Blocks are symmetric and applied at the **query layer** | Block evasion by direct URL or search |
| `noindex` on profiles and threads | Off-platform harassment sourced from indexed profiles |
| Level gates nothing | "Certified professional" as a route to unsupervised machine access |

**When someone proposes a feature, ask which row it weakens.** Most safety incidents will trace
back to a relaxation of one of these, not to a moderator's judgement call.

## 3. Decision authority — v1

**BAM decides. Alone.** Warnings, content removal, suspensions, and bans are all operator
decisions in v1.

This is deliberate, and it's worth being explicit about why, because "community moderation" was
part of the original ambition:

- **Liability cannot be delegated to volunteers.** Handing a member the ban button does not
  transfer the consequences of that ban away from the operator.
- **There is no community yet.** Moderation roles require a community with norms, tenure, and
  people who have earned trust. Appointing moderators on day one appoints strangers.
- **Unpaid moderators quit**, and moderator burnout is itself a safety incident — the queue
  stops being read.

Phase-2 community moderation is designed in [`coop-phase-2.md`](coop-phase-2.md) §Moderation.
The transition criteria are named there. Until those are met, this section does not change.

## 4. Report triage

### Targets

| Severity | Examples | First response | Resolution |
|---|---|---|---|
| **P0 — danger** | Threats, stalking, doxxing, sexual content involving minors, anything suggesting immediate physical risk | **Same day** | Immediate suspension pending review; permanent removal is the default outcome |
| **P1 — harm** | Harassment, repeat contact after a decline, hate content, sexual advances | **48 hours** | Warning or removal per §5 |
| **P2 — conduct** | No-shows, misrepresented level, credit disputes, spam, off-topic recruiting | **1 week** | Warning; repeat pattern escalates |
| **P3 — quality** | Miscategorized calls, low-effort profiles, wrong discipline tags | Best effort | Edit request or content removal |

If the queue cannot be read within the P1 target, **approvals pause** before triage slips.
Growing the member count while the report queue is unread is the specific failure that turns a
small problem into a large one.

### Handling a report

1. Read the report and the reported content. Check `moderation_action` for prior history on
   **both** parties — retaliatory reporting is common.
2. Decide per §5. Do not negotiate with either party mid-decision.
3. Write the `moderation_action` row. **Every action gets a row, including "no action."** The
   audit log is append-only; it is never edited or deleted.
4. Close the `report` with a `resolution`.
5. Notify the reporter that it was handled. Do **not** disclose what action was taken against
   the other person.
6. Notify the actioned member with the rule cited and the appeal path.

**Never disclose the reporter's identity to the reported member.** Not in the notice, not in an
appeal, not in a follow-up. This is the single most common way a moderation system gets someone
hurt.

## 5. Escalation ladder

**Warn** → **Remove content** → **Suspend** (time-boxed, profile hidden, calls closed, threads
frozen) → **Ban** (permanent, `profile.status = suspended`, all calls `removed`).

**Straight to ban, no ladder:** threats · stalking · doxxing · sexual content involving minors ·
anything placing someone in physical danger · ban evasion via a new account.

**Straight to suspension pending review:** any P0 while the facts are still being established.
It is better to suspend a member wrongly for two days than to leave a credible threat active for
two days.

## 6. Appeals

Members appeal by replying to the notice they receive. BAM reviews. A ban issued for a P0 is
reviewed but the bar for reversal is high. Log the appeal outcome as a `moderation_action` row
so the history stays complete.

## 7. Incident response

**A safety incident is any report where someone may have been physically harmed, threatened
with harm, or had personal information exposed.**

1. **Member safety first.** If someone is in danger, tell them to contact local emergency
   services. We are a website — we can close an account, and that is the extent of our power.
   Do not imply otherwise, and do not attempt to mediate a situation involving physical danger.
2. **Preserve evidence before acting.** Threads, calls, profiles, and work samples — capture
   them *before* suspension or deletion changes what is visible. Suspension must not hard-delete
   content.
3. **Suspend** the accounts involved.
4. **Record** it: what happened, when, what was done, what was preserved.
5. **Law enforcement:** respond to valid legal process. Do not volunteer member data absent
   valid process or a genuine emergency-disclosure request. When in doubt, ask counsel — see
   `gemini/witus/plans/user-tasks/` for the legal-track task.
6. **Post-incident:** ask which §2 design rule, if any, would have prevented it. If one would
   have, the fix is architectural, not a moderation policy edit.

## 8. Media moderation

Work samples are reviewed by BAM **at application**. Post-approval uploads are
**flagged-on-report**, not pre-moderated.

This is correct at v1 volume and wrong later. **Revisit threshold: 250 approved members, or
whenever post-approval uploads exceed roughly 50 per week.** At that point evaluate automated
scanning before adding human reviewers, and write the decision down here.

## 9. Rate limits

Present at launch, tuned after real traffic. Enforced **server-side**; a client-side limit is
not a limit.

| Action | Starting limit |
|---|---|
| Application submission | 1 per email; Turnstile-gated |
| Call creation | 3 per day, 10 per week |
| Interest sent | 10 per day |
| Messages | 60 per hour per thread |
| Report submission | 10 per day (higher is itself a signal worth reviewing) |

## 10. Data handling

- Suspension and ban **hide** content; they do not hard-delete it. Evidence must survive the
  enforcement action.
- Member-initiated deletion removes the profile and work samples; `moderation_action` rows
  persist with the user reference retained, because an append-only audit log that can be erased
  by the person it describes is not an audit log.
- Do not solicit PII in call bodies. If a member posts an address or phone number publicly,
  redact it and tell them why.
- `redacted_at` on `message` supports removing content from a thread without destroying the
  thread's evidentiary shape.

## 11. Review cadence

Read every open report weekly, even the P3s. Re-read this document quarterly, and after any P0.
When it stops matching what actually happens, change the document — a playbook nobody follows is
worse than none, because it reads like a commitment that was made and then quietly dropped.
