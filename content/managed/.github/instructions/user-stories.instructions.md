---
name: 'User Story Standards'
description: 'Template and quality bar for writing user stories: INVEST, acceptance criteria, Gherkin, and Definition of Ready.'
applyTo: 'docs/userstories/**'
---

# How to Write a Bullet-Proof User Story

Source inspiration: https://dev.to/brownio/bullet-proof-user-stories-for-your-team-with-ia-prompts-to-do-them-17g3

## 1) Standard User Story Format

As a [type of user], I want [an action/capability], so that [a business or user benefit].

Rules:
- Write from the end-user perspective.
- Keep it short and clear.
- Focus on outcome, not implementation details.

## 2) Validate With INVEST

- Independent: Can this be built and delivered without blocking on another story?
- Negotiable: Is it open for team discussion and refinement?
- Valuable: Does it deliver clear value to user or business?
- Estimable: Can engineering estimate effort with current details?
- Small: Can it fit in one sprint/iteration?
- Testable: Can QA objectively confirm completion?

If any item fails, rewrite the story.

## 3) Strong Acceptance Criteria

Keep each criterion:
- Specific: Exact expected behavior is described.
- Measurable: Pass/fail can be verified.
- Achievable: Realistic inside current constraints.
- Relevant: Directly linked to the story goal.
- Time-bound: Includes timing when timing matters.

Good practice:
- Cover main path and edge cases.
- Include negative behavior (what should not happen).
- Include accessibility, legal/compliance, and cross-device behavior when applicable.

## 4) Use Gherkin for Complex or Technical Stories

Use Gherkin when behavior must be unambiguous across PM, dev, and QA.

```
Feature: [Feature name]
    As a [user]
    I want [capability]
    So that [benefit]

    Scenario: [Scenario name]
        Given [initial context]
        When [action]
        Then [expected outcome]
        And [additional expected outcome]
```

This is for expressing an acceptance criterion itself unambiguously. It's distinct from the optional `Test Case Requirements` section (see below), which enumerates additional edge/negative/boundary scenarios beyond what the acceptance criteria state.

## 5) Test Case Requirements (Optional)

Use this section to call out specific edge-case, negative-path, or boundary scenarios worth flagging for QA — beyond the happy path already covered by Acceptance Criteria. Not every story needs one:

- Include it when the story has business logic, calculations, conditional branching, integrations, or security-sensitive flows where edge cases matter.
- Omit it entirely for simple, low-risk, or copy/UI-only stories — don't pad a story with a token scenario just to have one.

Format each scenario in Given-When-Then:

```
Test Case Requirements:
- Scenario: [edge case name]
  Given [initial context]
  When [action]
  Then [expected outcome]
```

## 6) Tasks (Optional)

Use this section to break a story down into concrete implementation steps for the developer(s) doing the work — distinct from Acceptance Criteria (which state *what* must be true when done) and Test Case Requirements (which state *what QA verifies*). Not every story needs one:

- Include it for technical/enabler stories (e.g. infrastructure setup, integration work) or any story where the implementation path benefits from an explicit checklist.
- Omit it for simple stories where the Acceptance Criteria alone are enough to plan the work.

Write tasks as a markdown checklist:

```
Tasks:
- [ ] [task 1]
- [ ] [task 2]
```

Checking off individual tasks as work progresses is expected and does not count as altering a locked story (see Story Status & Locking) — only adding, removing, or rewriting tasks after the fact does.

## 7) Story File Template

Copy this into each story file under `docs/userstories/`:

```
Title:

Status: Draft

User Story:
As a [user], I want [capability], so that [benefit].

Acceptance Criteria:
- [criterion 1]
- [criterion 2]
- [criterion 3]

Tasks: (optional — omit for simple stories)
- [ ] [task 1]
- [ ] [task 2]

Test Case Requirements: (optional — omit if the story doesn't merit edge-case testing)
- Scenario: [edge case name]
  Given [initial context]
  When [action]
  Then [expected outcome]

Out of Scope:
- [optional]

Notes:
- [optional assumptions/dependencies]

Estimate:
- [t-shirt size]
```

## 8) Assumptions Must Be Documented, Not Silent

If any detail needed to complete a story is missing or ambiguous in the source requirements, do not silently guess:
- Flag the assumption in your chat response.
- Record it explicitly under the story's `Notes:` section, prefixed with `Assumption:` (for example, `Assumption: Password reset links expire after 24 hours — not specified in requirements.`).

## 9) Definition of Ready

A story can move from `Draft` to `Ready` (see the `Status` lifecycle below) when:
- The story statement is complete.
- Acceptance criteria are testable and unambiguous.
- If a `Test Case Requirements` section is present, each scenario is specific, unambiguous, and genuinely testable (not just a restatement of the AC happy path) — a sloppy Test Case Requirements section blocks Ready the same way sloppy acceptance criteria would. Its absence never blocks Ready.
- If a `Tasks` section is present, each task is concrete and actionable (not vague). Its absence never blocks Ready.
- INVEST check passes.
- Unknowns and dependencies are captured (including any documented assumptions).
- Team can estimate without major unresolved assumptions.

Set `Status: Ready` only if all the above are true. Otherwise leave/set it as `Status: Draft` and list what's missing in `Notes:`.

## 10) Story Status & Locking

Every story file has a single `Status:` field near the top, one of five values. Four move forward through this lifecycle:

`Draft` → `Ready` → `In Progress` → `Done`

- **Draft / Ready** — pre-work states, gated by the Definition of Ready above. The story content is freely editable.
- **Deferred** — a non-actionable status, outside the lifecycle above, for a `Draft` or `Ready` story taken out of the current scope by a decision. The story is kept for reference, not deleted, and is not refined, estimated, or synced to the ticket tracker (`ticket-sync` skips it). Its `Notes:` must cite the decision and its date, and where the scope change is recorded (e.g. the `Out of Scope` section of `docs/reqs/<domain>.md`). When the scope is reopened, set it back to `Draft` and re-check it against the current requirements and architecture before it can be `Ready`.
- **In Progress / Done** — once a story reaches either of these, its content is **locked**: `User Story`, `Acceptance Criteria`, `Test Case Requirements`, `Out of Scope`, and `Estimate` must not be altered. `Tasks` is locked in the same sense (no adding/removing/rewriting tasks), except that checking off individual tasks as work completes is expected and allowed. This reflects that work has started or shipped against that exact spec — changing it out from under development or after delivery would make the story an inaccurate record of what was built.
  - **Allowed exceptions** to the lock: (1) moving `Status` forward to the next value, (2) checking off `Tasks` checklist items, and (3) appending a one-line backlink note (see below) — all additive, never a rewrite of existing content.
- If new work is needed on a locked story (a change of scope, a bug found later, a follow-up), do not edit it. Instead, create a **continuation story**:
  1. New file at `docs/userstories/<domain>/<slug>-2.md` (increment the numeric suffix again for further continuations: `-3.md`, etc.).
  2. Set `Status: Draft` on the new file, and add `Continuation of: docs/userstories/<domain>/<slug>.md` to its `Notes:`.
  3. Append a single line to the *locked* story's `Notes:` section: `Continued by: docs/userstories/<domain>/<slug>-2.md` — this is the one permitted addition to a locked file's Notes.
- Moving a story backward (e.g. `Done` back to `In Progress`) is a high-impact/hard-to-reverse decision — don't do it silently; ask via #tool:vscode/askQuestions.

## 11) Flagging Removed Content for the Ticket Tracker

`ticket-sync` only ever *adds* to the tracker on its own — it never decides something should be cancelled or removed. If you remove content from a story that already has a ticket key attached (its own `Ticket: <KEY>` line, or a task's `(Ticket: <KEY>)` suffix), you must leave a marker so `ticket-sync` knows what to do next time it runs:

- **Removing a `Tasks:` line** (not just unchecking it) from a story that's already synced: add `Ticket sync action needed: cancel sub-task (Ticket: <KEY>) — <reason>` to that story's `Notes:`, using the key from the removed task's `(Ticket: <KEY>)` suffix.
- **Removing/replacing a `Design:` reference** whose image was already attached in the tracker: add `Ticket sync action needed: remove attachment <filename> from (Ticket: <KEY>) — <reason>` to `Notes:`.
- **Deleting an entire story file** that has its own `Ticket: <KEY>` line: before deleting it, append an entry to `docs/tickets/retirements.md` listing the story's ticket key, every Sub-task key its `Tasks:` lines held, and the reason — then delete the file.

`ticket-sync` clears each marker once it's acted on it (replacing it with a past-tense `Retired: ...` note, or marking the `docs/tickets/retirements.md` entry processed) — don't remove a marker yourself before that happens.
