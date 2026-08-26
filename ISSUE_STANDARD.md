# Florida Poly issue standard

**Applies to:** every repository in the `FloridaPoly` organization with issues enabled.
**Owner:** Florida Polytechnic University ITS.

This document is the standard the inherited issue forms implement. It is
deliberately short: the forms do most of the enforcing.

---

## 1. The rule

**One fact, one place.** Every attribute of an issue has exactly one home. When a
fact has two homes, the two disagree within a month.

| Fact | Where it lives | Set by |
| --- | --- | --- |
| What kind of work this is | Native **issue type** | The issue form, at creation |
| How urgent it is | `priority:` label | Triage |
| What part of the system | `area:` label | Triage |
| Who must act next | `owner:` label | Triage |
| Regulatory posture | `flag:` label | Triage |
| Where it is stuck | `needs-*` / `blocked` label | Anyone, any time |
| Which release it must ship in | **Milestone** | Triage or planning |
| What it belongs to | **Sub-issue** of a parent | Whoever opens it |
| Everything else | The body | The author |

Consequences worth stating plainly:

- **The title carries no classification.** No `[Bug]`, no `[P1]`, no `[NOW]`.
- **No `bug` or `enhancement` label.** The type field replaces them.
- **No stage labels.** Milestones replace them.
- **No hand-written "Parent: #123" heading.** Sub-issues replace it.
- **Every label is prefixed**, except the four Dependabot creates.

---

## 2. Types

Five types, defined once at the organization level and applied by the forms. The
type answers one question: *what has to be true for this to be finished?*

| Type | Finished when | Use it for |
| --- | --- | --- |
| **Bug** | Behavior matches what was documented or intended | A defect in shipped behavior, wrong data, a crash, a broken deploy |
| **Feature** | A user can do something they could not do before | New capability, or a real change to how one behaves |
| **Chore** | The system is built, run, or documented differently and behaves the same | Refactor, tech debt, dependencies, CI, deploys, runbooks, docs, ops |
| **Research** | A question is answered and written down | Spike, benchmark, vendor evaluation, or a rule an office must confirm |
| **Epic** | Its children are done and the stated end state holds | An outcome needing more than one issue, reported on as a unit |

### Boundary calls, decided in advance

- **A doc change is a Chore**, with `area:docs`. Documentation is not its own type.
- **A missing feature is not a Bug.** If it never worked and was never promised, it is a Feature.
- **A defect in unreleased work is still a Bug.** The Environment field records where.
- **"Confirm the institutional rule for X" is Research**, owned by the office that knows, not by engineering.
- **A release is a Chore**, plus the milestone. Not a type.
- **A placeholder or stub to fill in is a Chore.** If filling it needs an answer from an office, it is Research.
- **Never write code against an Epic.** If a commit would reference it, it needed a child.
- **A question is not an issue.** Questions go to Discussions, or the support form on public repos.

An issue's type may change at triage. That is normal and costs nothing.

---

## 3. Priority

Four values. Each is a commitment about what happens next, not a feeling about
importance. If nothing changes when you apply it, apply a different one.

| Label | Meaning | Commitment |
| --- | --- | --- |
| `priority:blocker` | Production is wrong, unsafe, or down; or a dated external obligation will be missed | Other work stops. Someone is on it today. |
| `priority:high` | Committed to a named release or an external date | It is in the current milestone and has an assignee |
| `priority:normal` | Agreed work, scheduled normally | The default. Sits in a milestone or the backlog |
| `priority:low` | Real and agreed, but unscheduled | May be closed as not planned if it ages out |

- Open issues carry exactly one priority. Closed issues need none.
- **`priority:blocker` requires a body sentence naming what is broken and who is affected.** A blocker with no impact statement is downgraded at triage.
- Priority is set at triage by whoever owns the work. The reporter states impact; impact sets priority.
- **Wrong data outranks missing data.** A screen showing an incorrect student status is a blocker; a screen showing nothing is usually high.

Named values rather than P1/P2/P3, because numbered scales fork into competing
spellings and carry no written obligation.

---

## 4. Area

At least one `area:` label per issue.

`area:api` `area:ui` `area:data` `area:infra` `area:security`
`area:integrations` `area:agent` `area:docs` `area:tests`

A repository may add area labels for its own surfaces as long as they keep the
`area:` prefix. Repo-local areas are expected. Unprefixed ones are not, because
they break every cross-repo query.

---

## 5. Owner

`owner:` names **who must act next**, which at a university is usually the whole
question. `owner:dev` is the default and covers most issues. Offices are added as
needed, always prefixed, never a person's name — assignees are for people.

When an issue waits on an office it carries that office's `owner:` label plus
`blocked` or `needs-decision`, and the body names the specific ask.

"Waiting on Athletics" is not an ask. "Athletics must confirm whether a
mid-season transfer keeps the prior term's eligibility clock" is.

---

## 6. Flags

`flag:ferpa` `flag:pii` `flag:security` `flag:compliance`

Flags are additive and never removed silently. Any issue touching education
records carries `flag:ferpa`, including Chores that only move the data around.

**Vulnerabilities are never issues.** They go to private security advisories —
see [`SECURITY.md`](SECURITY.md). `flag:security` marks work we control that
needs review before it ships.

---

## 7. Status

| Label | Meaning |
| --- | --- |
| `needs-triage` | Not yet classified. Removed when triage completes. |
| `needs-decision` | Waiting on a named human decision. The body says whose. |
| `blocked` | Cannot proceed. The body names the blocker and the specific ask. |
| `needs-answer` | Public repos only: an unanswered question. Carries no type, because it is not work. |

`blocked` and `needs-decision` require an `owner:` label pointing at whoever can
clear them. A blocked issue with `owner:dev` and no named external party is not
blocked; it is unstarted.

---

## 8. Release: milestones, not labels

The release an issue must ship in is a **milestone**. Milestones carry a due date,
a progress count, and a burndown; labels carry none of those.

- One milestone per release or pilot.
- No milestone means not scheduled. That is a legitimate state and needs no label.
- For work spanning repositories, use a **Project** for the cross-repo view and keep milestones per repo. Projects cross repository boundaries; milestones do not.

---

## 9. Hierarchy: sub-issues, not prose

Use the native **sub-issue** relationship rather than a written "Parent" heading:
it renders progress on the parent, survives renumbering, and is queryable.

- An Epic holds sub-issues. A Feature may hold sub-issues if it grew.
- The parent's body lists the *planned* children as a record of intent. The real relationship is the sub-issue link.
- A child carries its own type, priority, and area. It does not inherit them.

---

## 10. Titles

**Format:** `area or component: imperative summary`

```
worklist: student name is not a link to the student profile
deploy: package locked Node dependencies for Flex deployment
eligibility: coaches cannot see why a student is flagged
```

Not this:

```
[Bug] [P1] Worklist broken
[NOW] fix deploy
Feature: add stuff to the dashboard
```

No type prefix, no priority prefix, no milestone prefix — all three are fields.
Lowercase after the colon, no trailing period, aim for 72 characters.

---

## 11. Bodies

The forms enforce this. What each type must contain:

| Type | Required sections |
| --- | --- |
| **Bug** | Defect, Reproduction, Expected vs actual, Failure scenario and impact, Environment |
| **Feature** | Problem, Outcome, Scope, Out of scope, Acceptance criteria, Data/roles/actions, Classification |
| **Chore** | Problem, Required change, Scope and out of scope, Acceptance criteria |
| **Research** | Question, Why it matters now, Output, Timebox, Who decides |
| **Epic** | Goal, Non-goals, Done when, Planned children |

Two requirements apply everywhere:

1. **Acceptance criteria must be verifiable by someone other than the author.**
   "Works correctly" is not a criterion. "A screen-reader user reaches every
   control in the worklist and hears its state" is.
2. **Nothing sensitive in the body.** No credentials, tokens, JWTs, tenant IDs,
   private hostnames, institutional IDs, or real student or employee data. Every
   form makes this a required checkbox, because these repositories touch
   education records and some are headed for public release.

`Out of scope` is required on Features and Chores. An unbounded issue cannot be
reviewed, estimated, or closed.

---

## 12. Triage

Weekly, per active repository. Ten minutes for most.

**Definition of triaged.** An issue leaves triage with all five, or it is closed:

1. A type, corrected here if the form got it wrong
2. Exactly one `priority:` label
3. At least one `area:` label
4. Exactly one `owner:` label
5. `needs-triage` removed

**Definition of ready**, required before anyone starts work: acceptance criteria a
second person can verify; no open `needs-decision`; data classification stated if
it touches data; scope bounded with out-of-scope filled in; an assignee.

**Service targets:**

| Condition | Target |
| --- | --- |
| `needs-triage` on an open issue | Cleared within 5 business days |
| `priority:blocker` | Acknowledged same day, in progress within 1 |
| `blocked` with no update | Reviewed at 14 days, closed or escalated at 30 |
| `needs-decision` with no update | Escalated to the named owner at 14 days |
| Open, no milestone, `priority:low`, untouched | Closed as not planned at 180 days |

---

## 13. Closing

Use the close reason, not a label:

- **Completed** — the acceptance criteria hold. Say where it shipped.
- **Not planned** — a real decision, with one sentence on why. Replaces `wontfix` and `invalid`.
- **Duplicate** — mark the survivor. Replaces the `duplicate` label.

A Research issue closes with its answer written down and a link to any follow-up
work. Research that closes with no recorded answer was wasted. An Epic closes
when its `Done when` holds; all children closed is necessary, not sufficient.

---

## 14. What a repository may change

Fixed org-wide: the five types and their meanings; the four priorities and their
commitments; the `area:` / `owner:` / `flag:` prefixes; the five core forms; the
redaction checkbox.

A repository may: add `area:` values for its own surfaces; add `owner:` values for
offices it deals with; add domain forms beyond the core five; keep Dependabot's
labels; choose its own milestone names.

A repository may not reintroduce a type label, a stage label, an unprefixed label,
or a title-prefix convention.

---

## 15. How the forms reach a repository

The core forms live in [`FloridaPoly/.github`](https://github.com/FloridaPoly/.github)
and are inherited automatically by every repository that has **no
`.github/ISSUE_TEMPLATE` folder of its own**.

That inheritance is **all or nothing**: if a repository has any file in its own
`.github/ISSUE_TEMPLATE` folder, none of the defaults apply there. A repository
needing its own intake form therefore carries the full set locally, kept in step
by script rather than by hand.

### Adoption status

The standard is being adopted in stages, so that no existing repository has to be
rewritten to benefit.

| Stage | State |
| --- | --- |
| The five issue types exist org-wide | **Done** |
| The core forms are inherited org-wide | **Done** |
| A private security-reporting route exists org-wide | **Done** |
| `priority:` / `area:` / `owner:` / `flag:` labels created per repo | Not started |
| Existing issues classified retroactively | Not started |
| Legacy labels retired | Not started |

Until the label stage lands, **the forms deliberately apply no labels.** A form
that names a label the repository does not have has that label silently dropped
by GitHub, which is how a template can appear to classify while classifying
nothing. Sections 3 through 7 describe the target state; they are not yet
enforced automatically.
