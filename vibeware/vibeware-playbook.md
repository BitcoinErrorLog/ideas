---
title: "The Vibeware Playbook"
subtitle: "A practical guide for teams that want software to learn from real use without turning into an uncontrolled agent pipeline."
author: "John Carvalho"
date: "2026-08-30"
---

# The Vibeware Playbook

*A practical guide for teams that want software to learn from real use without turning into an uncontrolled agent pipeline.*

Vibeware is software that can use evidence from real use to help produce and select its own subsequent implementations. The goal is not autonomous software for its own sake. The goal is to exploit the falling cost of implementation without allowing cheap generation to create expensive chaos.

The canonical loop is:

> **use → qualify → generate → expose → select → use**

This playbook is intentionally lighter than a production-governance framework. A first deployment should teach the team how the loop behaves before the team turns its guesses into permanent policy. The controls below are principles and starting defaults, not a substitute for judgment.

## 1. Choose a surface where being wrong is useful

Do not begin with the most important repository. Begin where ordinary failure is cheap enough to become information.

Good first surfaces include read-only tools, visualizations, navigation, search, ranking, discovery, onboarding, copy, dashboards, experimental workflows, and existing product features that can be exposed to an appropriate cohort. A money-related interface can participate when the actual signing, custody, and value movement remain behind hardened primitives.

Avoid making the first autonomous loop responsible for key handling, signing, custody, value movement, protocol compatibility, secrets, privileged infrastructure, irreversible public state, or other places where reverting the deployment does not undo the consequence.

This is not a rule against using AI on critical systems. AI can help specify, analyze, test, review, simulate, and implement them. It is a rule against letting noisy product evidence casually rewrite an invariant because the implementation happens to be cheap.

**The assurance burden follows the consequence of failure, not whether a human or an agent wrote the code.**

## 2. Start from direction, not an empty prompt

Vibeware is not a replacement for a roadmap. The roadmap becomes a statement of direction, goals, constraints, and known boundaries rather than an exhaustive queue of predetermined implementations.

Before starting a cycle, the owner should be able to explain why the experiment belongs to the product the team is trying to build. The people responsible for materially affected systems should know what is being attempted and should have rough alignment on the question and blast radius. This does not require a committee to agree on the final implementation. The purpose is to avoid a fast experiment accidentally becoming somebody else's surprise architecture.

A good cycle begins because the team wants to learn something that matters, not because an agent has spare tokens.

## 3. Write a one-page cycle brief

The brief constrains the loop without specifying the answer.

Write down:

**Surface.** What running product or experiment is allowed to move?

**Owner.** One person owns the cycle and can stop it.

**Question.** What are we trying to improve or learn? State the user problem rather than prescribing the implementation.

**Evidence.** What signals may qualify a change: diagnostics, repeated friction, explicit feedback, task completion, abandonment, usage patterns, controlled experiments, or other bounded evidence?

**Forbidden evidence.** What may not enter agent context: secrets, seed/key material, unnecessary identifiers, payment details, unrestricted session replay, private content, or anything else the team cannot justify collecting for this purpose?

**Boundary.** What files, systems, records, or capabilities must not be changed by this loop?

**Decision rule.** What would make a candidate worth keeping? If there is enough traffic for a meaningful experiment, state the outcome before exposure. If there is not enough traffic, say explicitly that the decision remains judgment-led.

**Exit.** How can the candidate be disabled, rolled back, redirected, or archived?

**Affected owners.** Who should be informed because the experiment touches their API, data, design surface, infrastructure, or maintenance responsibility?

If this takes weeks, you have recreated the process Vibeware is supposed to escape.

## 4. Capture broadly enough to see reality, narrowly enough to defend it

Use should create evidence, but the system does not need to ingest everything a user does.

Prefer evidence that is specific to the question. Counts, error classes, task results, paths, diffs, explicit preference, and reproducible fixtures are usually safer and easier to reason about than raw content or complete session histories. Existing diagnostics are preferable to building a new surveillance platform for the sake of an experiment.

Every collected field should have an answer to one question:

**What user-serving decision is this field allowed to influence?**

If the answer is “the model might find it useful,” do not collect it.

Treat all user-provided text as untrusted input. A crash string, support message, public post, or user-supplied document can contain instructions that an agent should not obey. The evidence layer should separate data from instructions, constrain tools independently of model context, and require human judgment when a requested code change is supported only by arbitrary free text.

The agent should never be allowed to expand its own telemetry contract.

## 5. Qualification is the most important stage

Sensitive software should have a threshold. Otherwise it is merely reactive software with an agent attached.

Most captured evidence should produce no code.

Before turning a signal into a candidate, ask whether it is repeated, bounded, plausibly caused by the product rather than an incident or deployment, relevant to the direction of the product, and consequential enough to deserve another implementation. Check whether a single power user or surviving cohort is dominating the signal. Check whether the evidence could be an artifact of the measurement system itself.

Most importantly, distinguish evidence from objective. “People use this a lot” does not necessarily mean “we should make more of it.” “People ignore this” does not necessarily mean “delete it.” “This generated engagement” does not mean “this improved the product.”

Users are a sensor, not an oracle.

A healthy Vibeware pipeline kills weak signals before generation becomes involved.

## 6. Generate candidates, not commitments

Once a signal survives qualification, generation should be cheap.

Give the agent the qualified problem, the relevant evidence, the allowed surface, and the boundaries. Let it produce the smallest running candidate that can answer the question. Do not require the prototype to carry the architecture it would need if it were already permanent.

This is where teams should resist a familiar instinct: forcing every experiment to use the final architecture before the experiment has proved that the behavior is worth keeping.

Reuse existing platform primitives when they are safe and sufficient. Fork, isolate, duplicate, or create a sidecar when doing so materially accelerates learning or protects production. Neither choice is morally superior. Architecture is part of the experiment.

The important requirement is that the team knows which shortcuts it took.

A successful experiment does not automatically make those shortcuts permanent.

## 7. Earn autonomy by stage

Do not give “the agent” a global authority level. Give specific stages more authority after they have established a boring track record.

A practical progression is:

1. **Observe.** The agent reads approved evidence and summarizes opportunities. It cannot change source.
2. **Propose.** It describes a candidate and why the evidence justifies trying it.
3. **Generate.** It opens a branch or pull request. A human reviews and merges.
4. **Expose.** It may deploy or merge into an experimental namespace, cohort, or flag after the team trusts its behavior and has a tested kill switch.
5. **Select within an envelope.** On sufficiently low-consequence surfaces, the system may eventually be allowed to keep, remove, or expose narrowly defined changes without a human approving every iteration.

A team does not need to reach the final stage for Vibeware to be useful.

Move authority backward when review load rises, corrective work rises, security findings appear, the telemetry contract is breached, or the team can no longer explain why the system is creating the work it is creating.

High-consequence boundaries should retain independent review even if the rest of the loop becomes highly autonomous. An agent should not approve its own changes to deployment security, secrets handling, infrastructure, payment primitives, key handling, protocol behavior, or equivalent critical paths.

## 8. Expose the candidate to reality

A candidate only becomes part of the loop when somebody can actually use it.

Exposure may mean a single owner, a willing experimental cohort, a public experimental surface, a feature flag, a temporary deployment, or a controlled experiment. Match the blast radius to the consequence of being wrong.

For high-traffic surfaces with a clean outcome, use real experimental discipline. Decide what result matters before looking at the result. Do not keep peeking until the graph produces the answer the team wanted.

For low-traffic surfaces, admit that statistics cannot decide. Small products, settings pages, edge flows, and specialized tools often do not have enough usage to distinguish small effects reliably. Use the evidence to inform judgment instead of laundering judgment through a dashboard.

Avoid objectives that a dumb harmful change can improve. Raw dwell, click volume, ticket count, event count, or merge count are usually bad selection criteria. Prefer task completion, error reduction, specific friction removed, successful return behavior, explicit preference, or another outcome connected to the actual purpose of the product.

## 9. Select, then decide whether graduation is warranted

Selection is not the same as promotion.

At review, a candidate should have a small number of possible states:

**keep vibing, redirect, archive, graduate, remain standalone, or extract a shared primitive.**

A selected candidate can continue living experimentally. It does not need to enter the permanent application because it “won” one iteration. Archive should remain normal and cheap.

If the behavior deserves permanence, graduation becomes a different engineering job. The team should ask what the experiment really proved, who will maintain the result, which shortcuts should be removed, whether an existing platform primitive should replace the prototype implementation, and whether something learned by several experiments should become a shared capability instead.

A prototype may have used a forked indexer, duplicated service, temporary schema, ugly query, or strange local architecture. Sometimes that code should be merged. Sometimes it should be rewritten. Sometimes the right output is a new primitive. Sometimes the code should be deleted while the insight survives.

**Do not confuse successful prototype code with the thing that must become production code.**

## 10. Close the loop in one existing review

Do not build an organization around the method before the method has earned one.

Use an existing product or experimental review when possible. Open the running software, show the evidence that qualified the change, show the candidate, show what happened after exposure, identify any boundary or maintenance issue, and make the selection decision.

The review should be about the running object and its consequences, not slides about the project.

Someone should be able to stop the loop immediately. Practice the kill switch before it matters.

If nobody will advocate for continuing an experiment, archive it.

## 11. A five-week first cycle

A first cycle should be short enough that the team learns about Vibeware itself.

**Week 1 — Direction and boundary.** Choose a suitable surface, name the owner, write the brief, confirm affected-team alignment, define the allowed evidence, and make the agent read-only. Exercise the kill switch.

**Week 2 — Qualification.** Let the agent summarize possible opportunities from real evidence. Humans inspect what it notices and, more importantly, what should be rejected. Tune the qualification threshold rather than trying to maximize candidate volume.

**Week 3 — Generation.** Allow the agent to create candidates for qualified problems. Keep human merge authority. Track whether generated work actually corresponds to the evidence and whether review or corrective work begins to rise.

**Week 4 — Exposure.** Put selected candidates in front of appropriate real users. Keep the blast radius proportionate to the consequence. Feed the resulting evidence back into the same qualification process.

**Week 5 — Selection.** Archive weak candidates, redirect ambiguous ones, continue promising experiments, and nominate only genuine survivors for graduation or extraction into shared primitives. Write down what the team learned about the product, the telemetry, the boundaries, and the loop before granting more autonomy or adding more surfaces.

The purpose of the first cycle is not to demonstrate that an agent can write code. That question is already settled. The purpose is to learn whether the team can turn real use into qualified generated variation without creating more noise, maintenance, capture, or risk than the loop removes.

## 12. Know when not to use Vibeware

Do not use this method merely because it is available.

Use deliberate R&D when the job is to invent something users cannot yet ask for. Use protocol design when compatibility and invariants are the product. Keep high-consequence paths heavily constrained. Do not pretend low-volume evidence is statistically decisive. Do not create a telemetry platform merely to give an agent something to read. Do not start a cycle if nobody will own a successful result.

Most importantly, do not allow sensitivity to replace taste.

Vibeware is valuable because reality can participate earlier and more continuously in software development. It does not make reality sovereign over the roadmap. The team still has to decide what product it is trying to build, which consequences it refuses to accept, which evidence matters, and what must remain fixed even when the users or the metrics are loud.

The generation of software is becoming cheap, which makes the judgment around that generation more important rather than less. This playbook exists to keep that judgment concentrated on direction, evidence, boundaries, selection, and permanence instead of spending it on manually translating every useful observation into another ticket.
