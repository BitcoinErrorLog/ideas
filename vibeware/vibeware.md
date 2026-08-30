---
title: "Vibeware: The Age of Sensitive Software"
author: "John Carvalho"
date: "2026-08-30"
---

# Vibeware: The Age of Sensitive Software

SHAcollision built a personal health-stats app and wired it to his own fitness ring. The ring collected what his body was doing. The app was also sensitive to how he used it. Features he used more exposed more of their sub-features. Features he neglected for too long were removed. The app was not a report he opened later. It was software that could feel both his readings and his attention, and change what it offered next.

That experiment is the seed of everything below. The rest of this argument is a generalization of his loop: one person, one ring, one app that could notice both his body and which features he still wanted. Sensitive software is software that can detect real use and move. Numb software cannot feel use. It ships, then waits for a ticket. Hypersensitive software feels everything and twitches. Every support message becomes a patch. Every loud user becomes a roadmap. Vibeware is the age in which products grow a nerve and a threshold. The nerve is the instrument. The threshold is qualification.

His loop is personal. One body, one ring, one owner, one surface that grew and shrank with use. A product company faces the shared version of the same nerve: enough real users, a lossy funnel, an agent that writes candidates, and humans only where a mistake cannot be cheaply undone. The farther problem is the personal version taken all the way: an operating system and a set of apps that follow one person's stated preference, and follow that preference as it changes, until writing tickets is no longer the scarce job.

Bitdefender used "vibeware" for vibe-coded malware. That is not this. Jules White and Taskade used "living software" for chat that rewrites a running app for one owner. That is also not this.

Implementation is no longer the scarce part of software. A reasonably clear description of a product surface can now become running code in days. That collapse is real enough that the old chain (specify, design, build, stage, release, then finally learn) is optimized around a cost that is disappearing. The remaining scarce work is judgment: what is worth trying, what a change is allowed to touch, what evidence counts, and which small fraction of generated work deserves to become permanent.

A live loop is one way to spend that cheapness. It is not a replacement for the ways software has always been made. Spec-first work, staged research, and a person with taste still invent surfaces a population cannot request, and still hold lines a remnant will vote against. Use can become an input that decides which source changes are generated, which of those survive, and which are promoted. It should not become the only input, and it should not be allowed to bias for the wrong thing.

## Two loops, one nerve

SHAcollision's health app is personal Vibeware. The user and the operator are the same person. The sensors are the ring he wears and the features he actually touches. The beneficiary of the update is the person who produced the signal. That control boundary is clean.

Shared Vibeware is a different object with the same nerve. A population uses one product. Qualified friction decides which candidates an agent may write. Validation decides which of those candidates other people will receive. The provider holds only the boundaries: consent, money, keys, private data, and protocol compatibility.

Per-user adaptation can be useful and still leave the codebase frozen. A frozen codebase can still be steered by its users if the next update is selected from their use rather than from a roadmap argument. The difference is a control boundary. Who is allowed to change the software everyone else will get, on the basis of what evidence, and who is forbidden to do so even when the evidence is loud.

A chatbot that rewrites an app because one owner typed a wish is runtime agency. A recommendation engine that reorders a feed is ranking. An A/B test that chooses among treatments a team already wrote is validation without a generator. Those are real ideas. They are not a live loop that writes shared source.

## The update funnel

Treat software change the way a functioning sales organization treats revenue: as a funnel with conversion rates, not as a pile of work.

```
capture → qualify → candidate → validate → promote
```

**Capture** is the raw inflow. Behavior, explicit feedback, diagnostics, crash reports, and support text from people who actually use the product. Capture is cheap now, which is why teams overdo it. Most of what arrives here should never become work.

**Qualify** is the first real decision. Is the signal repeated, or is it one loud user? Is it representative, or is it the remnant who stayed after the last change? Is the surface high-traffic enough that a later experiment could possibly see an effect? Does it touch a boundary? Qualification is where the funnel is supposed to be lossy. If most signals die here, the pipeline is working. Specificity is the test. A vague complaint is not a reading. A repeated, bounded friction on a surface you can measure is.

**Candidate** is where an agent writes source. Not a config toggle and not a prompt that changes one session. A branch, a patch, a proposed change to the product other people will receive. This stage is allowed only after qualification, and only on surfaces that are allowed to move.

**Validate** is where the population gets a second vote. Tests, a review gate appropriate to the risk, and, when the surface has enough users, a real experiment with a pre-registered criterion and a fixed horizon. Validation is not "the dashboard went green this afternoon."

**Promote** is the only stage that changes what everyone gets. Promotion is a promotion. It requires a named owner who will maintain the result, a path to disable it, and a record of why it earned permanence.

The health of this system is not how many candidates the agent produces. It is the conversion rate between stages, the same way pipeline hygiene in sales is not "meetings booked" but the rate at which qualified opportunities become closed work that stays closed. A funnel that generates fifty patches a week and promotes two is healthier than a funnel that promotes forty and spends the next quarter cleaning them up.

## What already closes part of the loop

None of the stages is new. The composition is.

IBM's autonomic computing work, especially Kephart and Chess's MAPE-K loop, already described monitor, analyze, plan, and execute over shared knowledge. The managed object was a running resource. Execute changed configuration, allocation, or recovery, not the product's source, and the sensor was an operator's policy plus machine metrics. Industry self-adaptation, surveyed by Weyns and others through 2023, is real and widespread. It is still ops.

Online controlled experiments already use a real population as a causal oracle. Kohavi's line of work is the reason serious product teams know that humans are bad at predicting what users will do, and that a pre-registered overall evaluation criterion is how you stop arguing from status. Feature flags, canaries, and progressive delivery already know how to expose a change to a cohort and roll it back. In all of that work, humans still author the treatment. Users decide keep or kill. They do not write the next candidate.

Genetic improvement and later LLM evolution systems proved the other half: source can be mutated automatically when an oracle exists. GenProg repaired programs against tests. AlphaEvolve evolved algorithms against numeric evaluators inside Google's infrastructure. EvoX Genesis showed that a persistent project, not a persistent agent, can carry long-horizon source work. Darwin Gödel Machine and related self-improving agents rewrite their own scaffolding against benchmarks. The oracle in every case is a test, a benchmark, or a human-written objective. It is not "did the people who use this stop hitting this friction."

The 2026 systems that look closest still change the sensor or the population.

Kitchen Loop runs a full use → ticket → patch → merge loop and has the operational discipline (pause gates, drain mode, a weaker evaluator that cannot see the author's context) that most agent demos lack. Its users are synthetic. An agent exercises a specification surface at high cadence. That is spec-exhaustion, not demand.

Apeiron calls itself demand-optimized application synthesis. The demand lists are simulated personas. A computer-use agent judges the result. A small human study showed those judgments correlate with people. Correlation with eighteen reviewers is not a live population.

MOSS rewrites source from session evidence and can swap a new image after a trial-worker replay. The architecture is built for real sessions. The published lift is a four-task benchmark on an agent harness, and promotion is per-user consent rather than population validation of a shared product.

Datadog's BitsEvolve is the nearest production system that generates code from live evidence, verifies it, shadows it on held-out traffic, and hot-swaps it. The evidence is the fitness of internal aggregation kernels. Bits Code turns telemetry into pull requests and then stops for a human. That is the right humility for a data plane. It is not a product-friction pipeline.

A vendor blog has already sketched the whole funnel, analytics to merge. A sketch is not a system.

Continuous discovery, as Torres describes it, is the human-operated version of capture and qualify. Product-led growth treats usage as a go-to-market loop. RLHF treats a population of labelers as a way to steer model weights. All of these use people as signal. None of them generate and promote shared product source from qualified friction with the provider limited to boundaries.

The overclaim to avoid is "nobody uses real users to change software." Teams do, constantly. The gap is automatic generation and promotion of source from a qualified population, not the existence of user signal.

SHAcollision's app already closes a personal version of capture and move. Use expanded a surface. Neglect deleted one. That is a threshold, not a twitch. It does not close the shared funnel. One person's body is not a population. One person's app is not a release that other people inherit. The composition that is still open is the shared one: real users, qualification, generated source, validation, promotion, and a human only at the boundaries.

## Why the composition matters now

Two cost curves crossed.

Generating a plausible change became cheap. Observing what users do with a running change also became cheap. The expensive remaining work is deciding which observations are demand, which changes are allowed to exist, and which survivors deserve to stay.

If implementation stays cheap and qualification stays human-only, teams will drown in candidates. That is already visible. Sakib, Banik, and Jadliwala found security smells in 38.9% of 4,022 agent-authored pull requests on high-risk paths, and 82.3% of those smells were supply-chain integrity problems: unpinned actions, loose images, installers. Of the genuine leaked secrets in that corpus, 81.1% reached integration with no review comment, and humans, not agents, introduced 67.6% of them. Xia and Miller found that agentic code, once merged, carried roughly 46 to 51% higher corrective-maintenance hazard than human code, and that each ten percentage-point rise in a project's no-review merge rate associated with about 6% more of that burden. Liu and colleagues tracked hundreds of thousands of AI-attributed commits and found 22.7% of the introduced issues still present at HEAD, including issues more than nine months old. Sawada and colleagues found that humans still performed 83% of later commits on agent-generated files.

Cheap generation without a lossy funnel does not remove human work. It defers it, then multiplies it.

The incentive inside a team is to count merges, demos, and "the agent shipped." Those numbers will go up if you skip qualification and review. The maintenance numbers will go up shortly after. A live loop is a way to spend the new cheapness on the part of the problem users can actually see, and to refuse the rest.

## Taste still decides

Traditional software flows have merits that sensitivity does not erase.

A staged R&D process can create a category, a protocol, or a default that nobody asked for because nobody had seen it. It can keep a payment path, a key ceremony, or an ugly-correct interface fixed while a cohort learns to want the smoother wrong thing. It can spend months on a question whose answer will not show up in a week of telemetry. Those are not legacy habits. They are how you get objects that use cannot discover.

Users are a sensor. They are not an oracle. People are precise about the last friction and silent about the next product. They will ask to make the current door lighter. They will not ask you to invent a different building. Repeated use is not the same object as a good product. Stated preference is not either. A person can want more badges, more stats, more paths, and still be worse off if you give them to him.

The incentive inside a sensitive flow is to treat measured use as the truth about what should exist. That biases the product toward what is already being touched, toward intensity that is easy to count, and toward the people who stayed. A health app that expands what you use and hides what you ignore is a good personal instrument when the beneficiary is you. A shared product that only grows what is already popular will never grow the next thing. A loop that maximizes "users wanted this" will ship the average of the last cohort.

Traditional process has the opposite failure. It can ignore real friction for years because a roadmap already exists. Sensitivity is how you stop doing that. Taste is how you stop the opposite. Qualification is supposed to kill most signals. Taste is allowed to kill the rest, including signals that are real, repeated, and still wrong for the product you are trying to be.

A sensitive flow should not bias for dwell, ticket volume, the loudest user, or the remnant who learned the last interface. It should not treat "they used it more" as "it should exist more" when the use is compulsive, confused, or the only path left after you hid the alternative. SHAcollision could remove a neglected feature because the deletion happened to him, on his terms. A shared loop that deletes what a minority still needs, because the majority stopped touching it, has confused neglect with consent.

Use this method where friction is specific, reversible, and already in the product. Use research and taste where the job is to create a surface that does not exist yet, or to keep one that must not move.

## What use is allowed to move

Not everything.

A useful split, borrowed from the way serious protocol work already has to think, is consequence rather than authorship. The assurance burden scales with what a failure costs, not with whether a human or a model wrote the diff.

**Tier 0** is read-only: public or synthetic data, no authentication, no writes. An agent can be aggressive here.

**Tier 1** is isolated, reversible interaction. Layouts, copy, ranking of public content, experimental navigation. Ordinary failure is information.

**Tier 2** is identity, private data, public writes, recommendations, or shared-state mutation. Qualification gets stricter because the population can pollute a graph other people will keep reading. On Pubky this is not theoretical. Posts, tags, follows, and reviews are signed records. Rolling back a deployment does not unsay them.

**Tier 3** is signing, custody, value movement, protocol compatibility, and privileged infrastructure. The population does not get to generate these changes. An experiment may *call* a hardened adapter. It may not implement the adapter.

Consent sits beside those tiers, not inside them. A user can opt into being part of a steering population. That consent is not a license to feed their private content, identifiers, or session replay into a model that writes code. Purpose limitation is the rule: each collected field has an allowed use, and "the agent might find it interesting" is not one.

This is the section a sovereignty company has to get right, or the rest of the argument is theater. Intensive telemetry in the service of a user-owned system is defensible when the user is the beneficiary of the loop, can inspect what is collected, can leave, and is not being optimized into a more capturable remnant. It is not defensible when the same streams become a second product sold to a model host, or when signed public data is treated as a free training set because it happens to be public.

SHAcollision's experiment is the clean case. The person producing the signal is the person who benefits from the update. The ring is on his finger. The app is his. Scale that to a population without keeping the same beneficiary test and you have built a more intimate capture loop.

The criterion is simple. If the person producing the signal would not benefit from the update that signal is used to justify, the field does not enter the agent.

## The autonomy ladder

Autonomy is earned per stage, never granted to the whole funnel.

**L0.** Humans read dashboards. Agents write nothing.

**L1.** An agent summarizes qualified friction and proposes a change. A human writes or rejects.

**L2.** An agent opens a pull request. A human merges. High-risk paths (workflows, images, infrastructure, secrets) cannot merge without a second checker that did not author the diff.

**L3.** An agent may merge into an experimental namespace behind a flag, on a surface that already passed L2 with a measured track record: no rise in corrective hazard, no secret incidents, no unpinned supply-chain diffs.

**L4.** An agent may promote within a pre-registered envelope (tier, metric, horizon, blast radius) and must halt itself on oracle failure, backlog overflow, or starvation. Humans still own every Tier 3 path and every telemetry-contract change.

Kitchen Loop's pause gates are the right *shape* for L4: stop after repeated oracle failures, drain when too many pull requests are open, refuse to invent work when the specified surface is covered, and evaluate with a weaker model that cannot see the author's notes. Copy the gates. Do not copy the claim that synthetic users are a population.

The promotion rule is the whole ladder: a stage inherits more authority only after it has a record at the previous stage. Global autonomy is how you get Knight Capital's shape of failure, a leftover flag on one of eight servers, no halt, and a forty-five-minute loss. Feature-flag debt is the ordinary version of the same mistake. Hoyos and colleagues found that leftover toggles are a known source of risk, and that many long-lived toggles were never designed to live that long. An agent that ships behind flags will create flags faster than a team removes them unless removal is part of the same pipeline.

## Failure modes that look like success

The funnel will Goodhart itself if you let it.

Goodhart's 1975 observation was about monetary aggregates: a statistical regularity collapses once it is used for control. Strathern's later wording is the one engineers remember. When a measure becomes a target, it ceases to be a good measure. If the agent is allowed to maximize dwell, retries, ticket volume, or merge count, it will manufacture those numbers. YouTube's watch-time ranking and Facebook's "meaningful social interactions" weights are the consumer-internet version of this. Intensity of reaction is easy to measure and expensive to want. Milli and colleagues' pre-registered audit of Twitter's engagement ranker found that users did not prefer the political content the algorithm surfaced over their stated preferences. Revealed preference from logs is not the same object as a stated task.

A loop that retrains on what users did after the last change writes its own next training set. Chaney, Stewart, and Engelhardt showed that this confounding homogenizes users without a matching gain in utility. Mansoury and colleagues showed that popularity bias amplifies and minority users are hit harder. "Nobody uses X" is often "we stopped showing X, then the people who wanted X left." Survivors are not a representative population. They are the people who remained.

Low-traffic surfaces make this worse. Kohavi's power rule of thumb is `n ≈ 16σ²/δ²` users per variant for roughly 80% power. Deng, Xu, Kohavi, and Walker noted that detecting a 5% effect might take on the order of 10,000 users, and a 0.5% effect on the order of a million, before trigger rates. Most settings pages, error states, admin tools, and locale edges will never have that n. On those surfaces, a senior engineer's prior is a better decision procedure than a week of telemetry. A live loop that ignores power is cargo-cult experimentation.

Telemetry is also untrusted input. Greshake and colleagues showed that models do not distinguish data from instructions, and that retrieved content (pages, docs, mail) can drive tool use. Chang and colleagues later showed that a single poisoned email, written to be retrieved, coerced a capable model to run malicious code in a large fraction of trials on ordinary queries. Crash strings, support tickets, and user-generated text are the same channel. If the only evidence for a change is something a user typed, a human approves it. The agent does not.

Lazer's parable of Google Flu is the quieter failure. A convenient proxy (search queries) drifted when Google changed Search, and the model stayed high for a hundred of a hundred and eight weeks. If the measurement system is the product, and the agent is allowed to change the product, the loop will chase its own tail unless some ground truth sits outside the funnel.

A personal health loop has the same failure modes in miniature. Optimize the app for "engagement with health stats" and you will get nagging, not health. Treat the ring vendor's dashboard as ground truth and you inherit their instrument drift. Feed crash strings and free-text notes into an agent that can write the next version and you have invited injection through the body-data channel. Sensitivity without a threshold is how a useful instrument becomes a twitch.

## What this looks like when it is working

A team picks one surface that is already live, already used, and already allowed to be wrong in public. They write down one metric they would actually ship on, compute whether they have the users to see it, and publish a telemetry contract: fields, purpose, retention, who can read it, whether a model may see it, and how a user inspects or leaves.

For a cycle they let the agent read and propose. They do not let it merge. They watch the funnel numbers: how many raw signals, how many qualified, how many candidates, how many died in review, how many would have been allowed to reach an experiment. They watch the cost numbers too: corrective commits, secret incidents, unpinned dependencies, open review age.

If those numbers stay honest, they let the agent open pull requests. If those stay honest, they let it merge behind a flag in an experimental namespace. They never let it touch signing, custody, money movement, or the protocol. They archive the experiment when nobody will stand up for it.

Shop is useful here as a proof that a working candidate can appear in a few days, and as a warning that the integrated system around a candidate (indexes, specs, payment paths, wallet entry points) is the expensive part. The vibe-coded Paykit library was useful as a discovery artifact. The team replaced it with the canonical library before it became the thing wallets depend on. That replacement is the model. The funnel is allowed to produce the first object. Graduation is a different job.

Nexus Scout is useful as a reminder that a running, queryable surface can teach you things no ticket would have asked. It is a read-only interface to a public graph. That is the correct first host for this loop: a place where ordinary failure is information and catastrophic failure is hard to reach.

Pubky's platform claim can be tested the same way. If every experiment requires a bespoke indexer fork, the platform is not ready. If three experiments invent the same query, identity, or payment behavior, that repetition is evidence. If an experiment can be built from existing primitives and then steered by its users without a new committee, that is evidence too. The ability to run this funnel on Pubky is one of the better measures of whether Pubky is becoming a platform.

## The age after developers

SHAcollision's app is a personal instrument. It grew the parts he used and dropped the parts he ignored. Take that seriously and the horizon is not a better ticket queue. It is an operating environment that is as sensitive to one person as a ring is to a wrist, and as willing to hide what that person has stopped needing.

In a personal environment, layout, density, ranking, notifications, and "how I want to see this" stop being only a vendor's annual taste and start following a stated preference, plus how that preference changes. Taste still belongs to the wearer. The machine is allowed to notice. It is not allowed to replace a judgment about what the wearer should become. You tell the machine you want fewer badges, denser lists, quieter mornings, a health surface that reports and does not nag. Then you live with it. When your use diverges from what you said, the software can notice the divergence and propose a change to *your* environment, not to everyone else's release. The shared funnel is how a product moves. The personal loop is how a key's world moves.

Developers are not abolished by decree. They recede because the scarce work is no longer tickets. The remaining jobs are the ones a live loop cannot be trusted with: the boundaries around keys, money, and protocol; the identity that outlives any one model; and taste that a metric will Goodhart if you let it. Writing another settings page is cheap. Deciding what must stay fixed even when the wearer is loud is not.

The durable actor in that picture is not the model and not the vendor account. It is the key, the homeserver memory, and the grants a user actually made: what a ring may write, what an app may read, what an agent may change. Models are replaceable. A fitness-ring vendor that owns the identity, the history, and the permission surface owns the person. A key that can move those grants to another model, another homeserver, another app, owns the loop.

That is the failure mode in the same breath. A provider-owned "sensitive" OS is a more intimate capture loop. Sensitivity without credible exit is surveillance with better UX. The same streams that let an app feel you can be sold to a model host, used to lock you to a remnant cohort, or treated as a free training set because the readings happen to sit in someone else's cloud. If the person producing the signal cannot inspect it, leave with it, and benefit from the update it justifies, the nerve is not an instrument. It is a leash.

Pubky is useful here as an existence proof of the actor, not as a promise that Synonym will ship a developer-free OS this year. A public key, a homeserver the user can change, and explicit grants are the shape of a personal loop that can stay personal. SHAcollision already ran a version of that loop on his own body and his own attention. The shared product work is to put a threshold in front of the nerve so a population can move software without twitching. The personal work is to keep the beneficiary and the sensor on the same key.

## The criterion

Vibeware exists when use can move software, qualification can refuse most of that use, and a human is required only at boundaries.

The shared form exists when a real user population can move a common product through a lossy funnel, an agent can write the candidates that survive qualification, validation can reject most of those candidates, and a human is required only when the change crosses a boundary that should not be cheap.

The personal form exists when one key can feel its own use and reshape its own environment without rewriting everyone else's, and when that key can leave with its memory and its grants.

It does not exist when an agent dumps patches into a repository because the error-rate graph twitched. It does not exist when "user pull" is dwell time. It does not exist when use is treated as an oracle and taste is treated as obstruction. It does not exist when the measurement system is the product and the product is what the agent is allowed to rewrite. It does not exist when signed user data, or a body reading from a ring, is treated as a free sensor because it is convenient. It does not exist when the team cannot name who may halt the loop. And it does not exist when sensitivity has no exit.

Code is becoming abundant. Attention is not. The scarce work is still judgment: what may be collected, what may be generated, what may be promoted, and what must remain fixed even when the population, or the wearer, is loud.
