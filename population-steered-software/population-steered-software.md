---
title: "Population-Steered Software"
author: "John Carvalho"
date: "2026-08-30"
---

# Population-Steered Software

Implementation is no longer the scarce part of software.

A reasonably clear description of a product surface can now become running code in days. That collapse is real enough that the old chain (specify, design, build, stage, release, then finally learn) is optimized around a cost that is disappearing. The remaining scarce work is judgment: what is worth trying, what a change is allowed to touch, what evidence counts, and which small fraction of generated work deserves to become permanent.

The next step is not another way to produce tickets faster. It is a way to let a real user population qualify what gets written next, let an agent write the candidates, and keep humans only where a mistake cannot be cheaply undone.

That is population-steered software.

It is not a chatbot that rewrites an app because one owner typed a wish. It is not a recommendation engine that reorders a feed. It is not an A/B test choosing among treatments a team already wrote. It is a staged pipeline in which use itself becomes the input that decides which source changes are generated, which of those survive, and which are promoted into the shared product.

## The phrase people already use

A nearby phrase, “living software,” already names something else.

Jules White used it in 2024 for systems that translate an individual’s goals into API calls at runtime. Taskade productized it as persistent memory, embedded agents, and continuous execution inside a workspace: describe a change in chat, and that running app updates. Jack Cheng used it for a relationship to flux, and argued that tool-like software should ship *less*. Steve Yegge used the same words in 2007 for systems that should not reboot in order to grow.

Those are real ideas. They are not this one.

Runtime agency follows one person’s intent. Population steering follows a *population’s* qualified friction, and it changes the *shared source* of a product. Per-user adaptation can be useful and still leave the codebase frozen. A frozen codebase can still be steered by its users if the next update is selected from their use rather than from a roadmap argument.

The difference is a control boundary. Who is allowed to change the software everyone else will get, on the basis of what evidence, and who is forbidden to do so even when the evidence is loud.

## The update funnel

Treat software change the way a functioning sales organization treats revenue: as a funnel with conversion rates, not as a pile of work.

```
capture → qualify → candidate → validate → promote
```

**Capture** is the raw inflow. Behavior, explicit feedback, diagnostics, crash reports, and support text from people who actually use the product. Capture is cheap now, which is why teams overdo it. Most of what arrives here should never become work.

**Qualify** is the first real decision. Is the signal repeated, or is it one loud user? Is it representative, or is it the remnant who stayed after the last change? Is the surface high-traffic enough that a later experiment could possibly see an effect? Does it touch a boundary? Qualification is where the funnel is supposed to be lossy. If most signals die here, the pipeline is working.

**Candidate** is where an agent writes source. Not a config toggle and not a prompt that changes one session. A branch, a patch, a proposed change to the product other people will receive. This stage is allowed only after qualification, and only on surfaces that are allowed to move.

**Validate** is where the population gets a second vote. Tests, a review gate appropriate to the risk, and, when the surface has enough users, a real experiment with a pre-registered criterion and a fixed horizon. Validation is not “the dashboard went green this afternoon.”

**Promote** is the only stage that changes what everyone gets. Promotion is a promotion. It requires a named owner who will maintain the result, a path to disable it, and a record of why it earned permanence.

The health of this system is not how many candidates the agent produces. It is the conversion rate between stages, the same way pipeline hygiene in sales is not “meetings booked” but the rate at which qualified opportunities become closed work that stays closed. A funnel that generates fifty patches a week and promotes two is healthier than a funnel that promotes forty and spends the next quarter cleaning them up.

## What already closes part of the loop

None of the stages is new. The composition is.

IBM’s autonomic computing work, especially Kephart and Chess’s MAPE-K loop, already described monitor, analyze, plan, and execute over shared knowledge. The managed object was a running resource. Execute changed configuration, allocation, or recovery, not the product’s source, and the sensor was an operator’s policy plus machine metrics. Industry self-adaptation, surveyed by Weyns and others through 2023, is real and widespread. It is still ops.

Online controlled experiments already use a real population as a causal oracle. Kohavi’s line of work is the reason serious product teams know that humans are bad at predicting what users will do, and that a pre-registered overall evaluation criterion is how you stop arguing from status. Feature flags, canaries, and progressive delivery already know how to expose a change to a cohort and roll it back. In all of that work, humans still author the treatment. Users decide keep or kill. They do not write the next candidate.

Genetic improvement and later LLM evolution systems proved the other half: source can be mutated automatically when an oracle exists. GenProg repaired programs against tests. AlphaEvolve evolved algorithms against numeric evaluators inside Google’s infrastructure. EvoX Genesis showed that a persistent project, not a persistent agent, can carry long-horizon source work. Darwin Gödel Machine and related self-improving agents rewrite their own scaffolding against benchmarks. The oracle in every case is a test, a benchmark, or a human-written objective. It is not “did the people who use this stop hitting this friction.”

The 2026 systems that look closest still change the sensor or the population.

Kitchen Loop runs a full use → ticket → patch → merge loop and has the operational discipline (pause gates, drain mode, a weaker evaluator that cannot see the author’s context) that most agent demos lack. Its users are synthetic. An agent exercises a specification surface at high cadence. That is spec-exhaustion, not demand.

Apeiron calls itself demand-optimized application synthesis. The demand lists are simulated personas. A computer-use agent judges the result. A small human study showed those judgments correlate with people. Correlation with eighteen reviewers is not a live population.

MOSS rewrites source from session evidence and can swap a new image after a trial-worker replay. The architecture is built for real sessions. The published lift is a four-task benchmark on an agent harness, and promotion is per-user consent rather than population validation of a shared product.

Datadog’s BitsEvolve is the nearest production system that generates code from live evidence, verifies it, shadows it on held-out traffic, and hot-swaps it. The evidence is the fitness of internal aggregation kernels. Bits Code turns telemetry into pull requests and then stops for a human. That is the right humility for a data plane. It is not a product-friction pipeline.

A vendor blog has already sketched the whole funnel, analytics to merge. A sketch is not a system.

Continuous discovery, as Torres describes it, is the human-operated version of capture and qualify. Product-led growth treats usage as a go-to-market loop. RLHF treats a population of labelers as a way to steer model weights. All of these use people as signal. None of them generate and promote shared product source from qualified friction with the provider limited to boundaries.

The overclaim to avoid is “nobody uses real users to change software.” Teams do, constantly. The gap is automatic generation and promotion of source from a qualified population, not the existence of user signal.

## Why the composition matters now

Two cost curves crossed.

Generating a plausible change became cheap. Observing what users do with a running change also became cheap. The expensive remaining work is deciding which observations are demand, which changes are allowed to exist, and which survivors deserve to stay.

If implementation stays cheap and qualification stays human-only, teams will drown in candidates. That is already visible. Sakib, Banik, and Jadliwala found security smells in 38.9% of 4,022 agent-authored pull requests on high-risk paths, and 82.3% of those smells were supply-chain integrity problems: unpinned actions, loose images, installers. Of the genuine leaked secrets in that corpus, 81.1% reached integration with no review comment, and humans, not agents, introduced 67.6% of them. Xia and Miller found that agentic code, once merged, carried roughly 46 to 51% higher corrective-maintenance hazard than human code, and that each ten percentage-point rise in a project’s no-review merge rate associated with about 6% more of that burden. Liu and colleagues tracked hundreds of thousands of AI-attributed commits and found 22.7% of the introduced issues still present at HEAD, including issues more than nine months old. Sawada and colleagues found that humans still performed 83% of later commits on agent-generated files.

Cheap generation without a lossy funnel does not remove human work. It defers it, then multiplies it.

The incentive inside a team is to count merges, demos, and “the agent shipped.” Those numbers will go up if you skip qualification and review. The maintenance numbers will go up shortly after. Population steering is a way to spend the new cheapness on the part of the problem users can actually see, and to refuse the rest.

## What the population is allowed to steer

Not everything.

A useful split, borrowed from the way serious protocol work already has to think, is consequence rather than authorship. The assurance burden scales with what a failure costs, not with whether a human or a model wrote the diff.

**Tier 0** is read-only: public or synthetic data, no authentication, no writes. An agent can be aggressive here.

**Tier 1** is isolated, reversible interaction. Layouts, copy, ranking of public content, experimental navigation. Ordinary failure is information.

**Tier 2** is identity, private data, public writes, recommendations, or shared-state mutation. Qualification gets stricter because the population can pollute a graph other people will keep reading. On Pubky this is not theoretical. Posts, tags, follows, and reviews are signed records. Rolling back a deployment does not unsay them.

**Tier 3** is signing, custody, value movement, protocol compatibility, and privileged infrastructure. The population does not get to generate these changes. An experiment may *call* a hardened adapter. It may not implement the adapter.

Consent sits beside those tiers, not inside them. A user can opt into being part of the steering population. That consent is not a license to feed their private content, identifiers, or session replay into a model that writes code. Purpose limitation is the rule: each collected field has an allowed use, and “the agent might find it interesting” is not one.

This is the section a sovereignty company has to get right, or the rest of the argument is theater. Intensive telemetry in the service of a user-owned system is defensible when the user is the beneficiary of the loop, can inspect what is collected, can leave, and is not being optimized into a more capturable remnant. It is not defensible when the same streams become a second product sold to a model host, or when signed public data is treated as a free training set because it happens to be public.

The criterion is simple. If the person producing the signal would not benefit from the update that signal is used to justify, the field does not enter the agent.

## The autonomy ladder

Autonomy is earned per stage, never granted to the whole funnel.

**L0.** Humans read dashboards. Agents write nothing.

**L1.** An agent summarizes qualified friction and proposes a change. A human writes or rejects.

**L2.** An agent opens a pull request. A human merges. High-risk paths (workflows, images, infrastructure, secrets) cannot merge without a second checker that did not author the diff.

**L3.** An agent may merge into an experimental namespace behind a flag, on a surface that already passed L2 with a measured track record: no rise in corrective hazard, no secret incidents, no unpinned supply-chain diffs.

**L4.** An agent may promote within a pre-registered envelope (tier, metric, horizon, blast radius) and must halt itself on oracle failure, backlog overflow, or starvation. Humans still own every Tier 3 path and every telemetry-contract change.

Kitchen Loop’s pause gates are the right *shape* for L4: stop after repeated oracle failures, drain when too many pull requests are open, refuse to invent work when the specified surface is covered, and evaluate with a weaker model that cannot see the author’s notes. Copy the gates. Do not copy the claim that synthetic users are a population.

The promotion rule is the whole ladder: a stage inherits more authority only after it has a record at the previous stage. Global autonomy is how you get Knight Capital’s shape of failure, a leftover flag on one of eight servers, no halt, and a forty-five-minute loss. Feature-flag debt is the ordinary version of the same mistake. Hoyos and colleagues found that leftover toggles are a known source of risk, and that many long-lived toggles were never designed to live that long. An agent that ships behind flags will create flags faster than a team removes them unless removal is part of the same pipeline.

## Failure modes that look like success

The funnel will Goodhart itself if you let it.

Goodhart’s 1975 observation was about monetary aggregates: a statistical regularity collapses once it is used for control. Strathern’s later wording is the one engineers remember. When a measure becomes a target, it ceases to be a good measure. If the agent is allowed to maximize dwell, retries, ticket volume, or merge count, it will manufacture those numbers. YouTube’s watch-time ranking and Facebook’s “meaningful social interactions” weights are the consumer-internet version of this. Intensity of reaction is easy to measure and expensive to want. Milli and colleagues’ pre-registered audit of Twitter’s engagement ranker found that users did not prefer the political content the algorithm surfaced over their stated preferences. Revealed preference from logs is not the same object as a stated task.

A loop that retrains on what users did after the last change writes its own next training set. Chaney, Stewart, and Engelhardt showed that this confounding homogenizes users without a matching gain in utility. Mansoury and colleagues showed that popularity bias amplifies and minority users are hit harder. “Nobody uses X” is often “we stopped showing X, then the people who wanted X left.” Survivors are not a representative population. They are the people who remained.

Low-traffic surfaces make this worse. Kohavi’s power rule of thumb is `n ≈ 16σ²/δ²` users per variant for roughly 80% power. Deng, Xu, Kohavi, and Walker noted that detecting a 5% effect might take on the order of 10,000 users, and a 0.5% effect on the order of a million, before trigger rates. Most settings pages, error states, admin tools, and locale edges will never have that n. On those surfaces, a senior engineer’s prior is a better decision procedure than a week of telemetry. Population steering that ignores power is cargo-cult experimentation.

Telemetry is also untrusted input. Greshake and colleagues showed that models do not distinguish data from instructions, and that retrieved content (pages, docs, mail) can drive tool use. Chang and colleagues later showed that a single poisoned email, written to be retrieved, coerced a capable model to run malicious code in a large fraction of trials on ordinary queries. Crash strings, support tickets, and user-generated text are the same channel. If the only evidence for a change is something a user typed, a human approves it. The agent does not.

Lazer’s parable of Google Flu is the quieter failure. A convenient proxy (search queries) drifted when Google changed Search, and the model stayed high for a hundred of a hundred and eight weeks. If the measurement system is the product, and the agent is allowed to change the product, the loop will chase its own tail unless some ground truth sits outside the funnel.

## What this looks like when it is working

A team picks one surface that is already live, already used, and already allowed to be wrong in public. They write down one metric they would actually ship on, compute whether they have the users to see it, and publish a telemetry contract: fields, purpose, retention, who can read it, whether a model may see it, and how a user inspects or leaves.

For a cycle they let the agent read and propose. They do not let it merge. They watch the funnel numbers: how many raw signals, how many qualified, how many candidates, how many died in review, how many would have been allowed to reach an experiment. They watch the cost numbers too: corrective commits, secret incidents, unpinned dependencies, open review age.

If those numbers stay honest, they let the agent open pull requests. If those stay honest, they let it merge behind a flag in an experimental namespace. They never let it touch signing, custody, money movement, or the protocol. They archive the experiment when nobody will stand up for it.

Shop is useful here as a proof that a working candidate can appear in a few days, and as a warning that the integrated system around a candidate (indexes, specs, payment paths, wallet entry points) is the expensive part. The vibe-coded Paykit library was useful as a discovery artifact. The team replaced it with the canonical library before it became the thing wallets depend on. That replacement is the model. The funnel is allowed to produce the first object. Graduation is a different job.

Nexus Scout is useful as a reminder that a running, queryable surface can teach you things no ticket would have asked. It is a read-only interface to a public graph. That is the correct first host for this loop: a place where ordinary failure is information and catastrophic failure is hard to reach.

Pubky’s platform claim can be tested the same way. If every experiment requires a bespoke indexer fork, the platform is not ready. If three experiments invent the same query, identity, or payment behavior, that repetition is evidence. If an experiment can be built from existing primitives and then steered by its users without a new committee, that is evidence too. The ability to run this funnel on Pubky is one of the better measures of whether Pubky is becoming a platform.

## The criterion

Population-steered software exists when a real user population can move a shared product through a lossy funnel, an agent can write the candidates that survive qualification, validation can reject most of those candidates, and a human is required only when the change crosses a boundary that should not be cheap.

It does not exist when an agent dumps patches into a repository because the error-rate graph twitched. It does not exist when “user pull” is dwell time. It does not exist when the measurement system is the product and the product is what the agent is allowed to rewrite. It does not exist when signed user data is treated as a free sensor because it is public. And it does not exist when the team cannot name who may halt the loop.

Code is becoming abundant. Attention is not. The scarce work is still judgment: what may be collected, what may be generated, what may be promoted, and what must remain fixed even when the population is loud.
