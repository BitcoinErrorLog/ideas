---
title: "Vibeware: Sensitive Software"
subtitle: "Vibe coding made implementation cheap. The next step is software that can feel its own use, generate alternatives, and evolve under human judgment."
author: "John Carvalho"
date: "2026-08-30"
---

# Vibeware: The Age of Sensitive Software

*Vibe coding made implementation cheap. The next step is software that can feel its own use, generate alternatives, and evolve under human judgment.*

Vibe coding is still being discussed mainly as a faster way to write software, but I think that framing is already too small. The important change is not that an engineer can type less code. It is that implementation is becoming cheap enough to stop being the scarce phase of software development. Once that happens, the logic of the whole workflow starts to change. We can spend less effort trying to predict the exact software we should build before it exists, produce more running candidates instead, and move more of our judgment to the point where we have something real to react to.

A colleague of mine, Chris (SHAcollision), recently pushed this one step further without really setting out to make a theory about it. He vibe-coded a personal health application connected to his fitness ring and watches. The obvious input was his health data, but he also gave the application unusually aggressive telemetry about how he used the software itself. Features he used more could expose more functionality. Features he kept ignoring could disappear. New ideas could be inserted simply to see whether they survived contact with his actual behavior. He did not have enough time to keep writing prompts telling the application how to evolve, but he *was* using it every day, so the use itself became feedback for the next version. That is a much more interesting object than an AI-generated app. The software had begun to develop a kind of sensitivity: it could feel both the domain it represented and the way its owner interacted with it, then use those readings to help decide what to offer next. It was not autonomous in some science-fiction sense, and it did not possess taste or authority. It simply closed more of the loop between use and implementation than normal software does.

I think this is the next important step after vibe coding. I call it **Vibeware**, or sensitive software.[^0] In the sense I mean it, Vibeware is software whose cheap mutability is disciplined by real use, qualification, selection, and explicit boundaries.

## From vibe coding to the Running Spec

The first-order effect of AI coding is obvious: implementation gets cheaper. A reasonably clear description of a product surface can increasingly become running software in hours or days, rather than after months of specification, design, implementation, staging, and release work. The result may not yet be production-grade, but it is often good enough to answer the question that motivated the specification in the first place: *is this actually what we want?*

That gives us what we have been calling the **Running Spec**. When a running candidate becomes cheaper to produce than an exhaustive description of that candidate, the implementation itself can become part of the specification process. Instead of spending weeks resolving every ambiguity in prose or design files, you make a version, interact with it, show it to the people who care, and let the remaining disagreement become concrete.

This does not eliminate specifications. It changes what deserves to be specified in advance. Protocols, invariants, money paths, security properties, interoperability constraints, and anything whose failure is difficult to undo still benefit from deliberate human+AI design before implementation. But a huge amount of ordinary application work exists because implementation used to be too expensive to use as a thinking tool. That assumption is disappearing.

The old workflow was optimized around scarce implementation:

**specify → design → build → stage → release → learn**

A cheaper implementation world allows another loop:

**direction → running candidate → real use → selection → harden what survives**

The point is not to glorify incompleteness. The point is to stop paying the full cost of permanence before reality has given us any reason to believe the thing deserves to become permanent.

At Synonym we started calling the environment for this **Vibes**: a place where roadmap-oriented ideas can reach real users early, with clear warnings and sensible boundaries, rather than waiting until every experiment has already been turned into production software. The idea is deliberately architectural rather than architectural-*ist*. If the existing platform can safely support an experiment, use it. If a fork, sidecar, temporary indexer, or isolated deployment materially accelerates discovery or protects production, use that instead. Prototype architecture is evidence, not inheritance.

Vibeware begins when the running candidate does more than teach the humans. It starts participating in the production of its own successor.

## The loop

A useful way to describe the loop is:

**use → qualify → generate → expose → select → use**

**Use** produces evidence. That evidence can be explicit feedback, diagnostics, repeated friction, abandonment, task completion, public behavior, or any other signal the product is legitimately allowed to observe.

**Qualification** is what prevents sensitivity from becoming twitching. Most observations should produce nothing. A support complaint is not automatically demand. A spike is not automatically a problem. A power user is not automatically a population. A metric moving is not automatically an objective. Qualification decides whether there is enough specific evidence to justify creating another candidate.

**Generation** is now cheap. An agent can inspect the qualified problem, understand the relevant code, and produce a branch, patch, or experimental implementation. This is the part of the pipeline that will become increasingly commoditized, which is exactly why a good Vibeware system should not optimize for how much code it generates.

**Exposure** puts the candidate back in contact with reality. Sometimes that means one person. Sometimes an opted-in cohort. Sometimes a public experimental surface. Sometimes a controlled test. The candidate has to become running software again because the loop learns from consequences, not from the agent admiring its own diff.

**Selection** decides what survives. A candidate may be rejected, redirected, left experimental, rewritten properly, promoted into the main product, or used only as evidence that a missing platform primitive should exist. Selection is not the same as merge. It is the scarce act of deciding which tiny fraction of cheap variation deserves more attention.

Then use begins again.

None of the individual components is new. Telemetry is old. A/B testing is old. Progressive delivery is old. Automated remediation is old. Generated code is no longer new. The new object is the composition: real use can qualify a change, an agent can generate the running alternative, real use can evaluate it again, and the system can keep closing that loop without requiring a human organization to manually translate every observation into a ticket, every ticket into a specification, and every specification into a hand-shepherded implementation.

## Selection becomes more important as code becomes abundant

Cheap implementation creates a tempting but dangerous illusion: if code is cheap, ship more code.

That is almost exactly backwards. If generation becomes abundant, selection becomes more important.

Early empirical work on agent-assisted repositories already gives us a preview of the problem. Sakib, Banik, and Jadliwala found at least one security smell in 38.9% of 4,022 agent-generated pull requests in a corpus focused on high-impact file paths, with supply-chain integrity problems dominating the findings.[^1] Xia and Miller, studying the post-merge fate of agentic code across 182 repositories, found substantially higher corrective-maintenance rates for agentic contributions even though aggregate maintenance rates were similar.[^2] These results should not be read as proof that humans write safer code—the first study, notably, found humans responsible for most genuine leaked secrets in the agent-assisted workflows it examined. They show something more relevant here: making implementation cheap does not make review, maintenance, security, or responsibility cheap.

A software organization that celebrates generated patches, merges, or demos as the primary output will simply move its bottleneck downstream. It will produce code faster than it can decide what the code is for.

Vibeware should therefore be deliberately lossy. Most captured signals should die during qualification. Most generated candidates should never become permanent. Successful experiments should still be allowed to end with their code being thrown away. Sometimes the thing worth preserving is the behavior. Sometimes it is the interface. Sometimes three disposable experiments reveal the same missing primitive and the right answer is to implement that primitive properly. Sometimes the correct conclusion is simply that the idea was wrong.

The goal is not maximum mutation. It is better selection under conditions where mutation is cheap.

## Users are a sensor, not an oracle

This distinction matters because a sensitive product can easily become a product that chases its users.

People are often excellent at exposing the friction directly in front of them and terrible at requesting the thing that does not exist yet. A population can tell you that the current door is heavy. It will rarely ask you to invent a different building. If we treat observed use as authority, we will optimize the present indefinitely and call the result product discovery.

The usual product process has the opposite failure mode. A roadmap can acquire enough institutional weight that obvious evidence stops mattering. Teams can continue implementing, polishing, and launching something that users clearly do not want because the planned object has become more real inside the organization than the running product is outside it.

Sensitive software is useful because it gives reality a stronger vote. Taste remains necessary because reality is not the only voter.

The same warning applies to metrics. Dwell time, clicks, session length, ticket volume, feature frequency, and engagement can all be measured correctly while representing the wrong objective. A confusing feature can generate interaction. An addictive mechanism can generate retention. A minority can depend on something the majority ignores. A cohort can look satisfied because the people who hated the last change already left.

A strong Vibeware loop does not ask, “What did users do, therefore what should the software become?” It asks, “What does this use tell us, what alternative is worth testing, and are we willing to own the consequence if the measurement tells us to keep it?”

That is judgment. AI does not make it less important. It concentrates more of the value there.

## Consequence determines the workflow

The most obvious objection to this model is also correct: some software should not be sensitive in the same way.

A read-only visualization can be wrong in public and teach us something. A navigation experiment can be ugly for a day and disappear. An experimental search interface, layout, ranking, onboarding flow, or public-data tool can often tolerate aggressive iteration because ordinary failure is reversible information.

A signing primitive is not the same object. Neither is custody, value movement, key handling, protocol compatibility, private data, privileged infrastructure, or durable signed state. Rolling back the application does not necessarily roll back what those systems did.

The right boundary is therefore **consequence, not authorship**. Human-written code can be catastrophic. Agent-written code can be harmless. The assurance burden should scale with what a failure costs.

This also avoids a false choice between the old process and the new one. Protocol work may remain heavily spec-first because the specification is part of the product. Money-critical paths may use AI aggressively for analysis, testing, implementation assistance, and review while still requiring deliberate invariants and independent human judgment before a change can move value. Meanwhile the application surfaces around those primitives can become extremely fluid.

Sensitive software should be most sensitive where being wrong is cheap, and progressively less sensitive as mistakes become expensive or irreversible.

This is not a compromise with the idea. It is the architecture of the idea.

## Personal Vibeware is more profound than shared Vibeware

Chris's health app also reveals another distinction that I think matters more in the long run.

Shared Vibeware uses evidence from a population to move a common product. That can be powerful, but it has all the familiar problems of product analytics: representation, incentives, privacy, Goodharting, minorities, consent, and the provider deciding what the population is being optimized toward.

Personal Vibeware has a much cleaner beneficiary. One person produces the signal, one person's environment changes, and that same person receives the result. The application can learn that I prefer dense lists, that I stopped using a dashboard, that I want fewer notifications, that I always open one path and never another, or that my stated preference no longer matches my revealed behavior. It can propose changes to *my* environment without deciding that everyone else should inherit them.

Taken seriously, that points beyond adaptive applications toward a software environment that can become as sensitive to an individual as a fitness ring is to a wrist. Interfaces, workflows, defaults, automations, information density, and eventually whole combinations of applications can become less like fixed vendor decisions and more like a continuously negotiated expression of what the user wants now.

That possibility makes ownership more important, not less.

The same streams that let software feel a person can become an extraordinarily intimate capture system. If the vendor owns the identity, the history, the telemetry, the permissions, and the model that interprets all of it, “sensitive software” can simply mean software that has become better at sensing its captive user.

The durable actor should therefore not be the model and should not have to be the application vendor. It should be the user: an identity that can outlive a particular model, memory that can move, and explicit grants describing what a device may write, what an application may read, and what an agent may change. Models should become replaceable components of the loop rather than owners of the loop.

This is one reason key-based systems such as Pubky become interesting in a world of increasingly dynamic software. A public key can outlive the current interface. A homeserver can preserve user-controlled memory across applications. Grants can separate identity from whichever agent happens to be useful this month. None of that automatically makes the system sovereign, but it gives us a credible shape for sensitive software whose sensitivity remains in service of the person producing the signal.

Without credible exit, sensitive software is surveillance with better UX.

## The age of sensitive software

The first chapter of AI coding was about authorship: how much code a model could write. The more important chapter is about software economics. What happens when implementations become cheap enough that producing a candidate is easier than exhaustively describing one? What happens when we can afford to put far more running ideas in contact with reality before deciding which ones deserve production engineering? What happens when those running systems can also observe their own consequences and participate in generating the next candidate? At that point, the software-development loop itself starts changing shape.

Developers do not disappear, and neither do specifications, architecture, protocols, or product taste. If anything, their real value becomes easier to see once raw implementation stops consuming so much of the available attention. The scarce work moves toward deciding what should exist, which boundaries must hold, what evidence means, which experiments deserve further life, and what must remain fixed even when a measurement argues otherwise. Vibe coding makes code cheap enough to produce more possibilities; the Running Spec makes those possibilities cheap enough to think with; and Vibeware closes the next loop by allowing software to feel the consequences of those possibilities and participate in producing the next ones.

The defining problem of that world is no longer how to generate enough software. It is how to build systems, organizations, and personal computing environments that can generate abundant variation without surrendering judgment to the generator or authority to the measurement. **Vibeware begins when software can feel use strongly enough to move, without confusing sensitivity for authority.**

---

## References

[^0]: Bitdefender used “vibeware” in 2026 to describe high-volume AI-generated malware. This article uses the word independently and in a different sense: software that is sensitive to real use and capable of participating in its own iterative development. See Radu Tudorica et al., *APT36: A Nightmare of Vibeware*, Bitdefender, 2026.

[^1]: A. H. M. Nazmus Sakib, Dipayan Banik, and Murtuza Jadliwala, *Trust but Verify? Uncovering the Security Debt of Autonomous Coding Agents*, arXiv:2607.12428, 2026. https://arxiv.org/abs/2607.12428

[^2]: Chunqiu Steven Xia and Courtney Miller, *Do These Violent Delights Have Violent Ends? Measuring the Post-Merge Fate of Agentic Code*, arXiv:2607.09902, 2026. https://arxiv.org/abs/2607.09902
