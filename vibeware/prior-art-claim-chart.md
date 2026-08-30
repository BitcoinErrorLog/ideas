# Vibeware: Prior-Art Claim Chart

**Cutoff date:** 30 August 2026  
**Purpose:** Technical novelty screening for Vibeware (sensitive software with a live update funnel). This is not a legal novelty or freedom-to-operate opinion.

## The claim under test

A product whose **shared source** is updated by a staged pipeline:

> raw population signal → qualified friction → generated candidate → validated candidate → promoted update

The provider intervenes only at **boundaries**: consent, money, keys, private data, and protocol compatibility.

That composition is the object. Individual stages already exist.

## Property legend

- **Q1 — Real population sensor:** the demand signal is a live user population of the product, not synthetic users, lab personas, tests, or a single operator.
- **Q2 — Qualification stage:** raw signal is filtered for repetition, representativeness, and blast radius before it becomes work.
- **Q3 — Automatic candidate generation:** an agent (or search) writes a source-level change from qualified friction. Humans do not author the treatment.
- **Q4 — Population validation:** the candidate is tested against real users or a causal experiment, not only unit tests or synthetic journeys.
- **Q5 — Promotion of source:** the accepted change becomes the shared product update, not a per-user runtime translation and not a config tweak.
- **Q6 — Boundary-only provider:** humans retain authority over consent, money, keys, private data, and irreversible contracts. They are not required for ordinary candidate authorship.

Classification:

- **composed:** Q1 through Q6
- **strong partial:** closes generate-and-ship or real-user validation, but not both with a population sensor
- **adjacent:** same loop shape, different managed object or sensor
- **name collision:** shared phrase, different object
- **not comparable**

## Verdict

No verified academic or production system is **composed**. Adjacent systems close monitor/analyze, generate source, or ship updates. None combine a real-population demand qualifier with automatic source generation and promotion of a shared product.

The phrase **Living Software** is occupied for a different object: runtime translation of an individual's goals. Bitdefender used **vibeware** in 2026 for vibe-coded malware. That is name collision, not this composition. The category name used here is **Vibeware**: software that can feel use and move, with a threshold. The shared mechanism is the **update funnel**. Personal Vibeware (one key, one instrument) is a related object, not the composed claim under test.

Search negatives as of this cutoff:

- `"population-steered"` software: no academic hit (earlier working name for the shared funnel)
- `"user-steered evolution"` software: no academic hit
- `"demand-driven autonomous development"`: no exact hit
- `"sales pipeline" for software change`: no systems paper. Hits are sales tooling or essays arguing *against* pipeline framing of the SDLC
- `"vibeware"` as product-evolution / sensitive-software: no academic hit. Security-press hits are Bitdefender's malware coinage.

---

## 1. Autonomic computing and MAPE-K

### Horn manifesto (2001)

**Source:** Paul Horn, *Autonomic Computing: IBM’s Perspective on the State of Information Technology*, IBM, 15 October 2001.  
**Witness:** Alan G. Ganek and Thomas A. Corbi, “The dawning of the autonomic computing era,” *IBM Systems Journal* 42(1), 2003. [PDF](https://www.mirrorservice.org/sites/www.bitsavers.org/pdf/ibm/IBM_Systems_Journal/421/ganek.pdf)

- Classification: **adjacent**
- Matches: biological self-management metaphor; complexity as the reason for a loop
- Missing: Q1–Q6 as product change. The managed object is IT operations.

### Kephart and Chess (2003)

**Source:** Jeffrey O. Kephart and David M. Chess, “The Vision of Autonomic Computing,” *IEEE Computer* 36(1): 41–50, January 2003. [DOI 10.1109/MC.2003.1160055](https://doi.org/10.1109/MC.2003.1160055)

- Classification: **adjacent** (loop ancestor)
- Matches: Monitor → Analyze → Plan → Execute over shared Knowledge
- Missing: Q1, Q3 as product source, Q5. Execute acts on a managed resource through effectors. Users appear as load, not as demand.

MAPE-K is the architectural ancestor of any later telemetry-to-action loop. Shared Vibeware reuses the loop shape and changes two things: the managed object from running system to product source, and the sensor from ops metrics to qualified population friction.

### IBM blueprint (2003)

**Source:** IBM, *An Architectural Blueprint for Autonomic Computing*, April 2003.

- Classification: **adjacent**
- Difference: engineering spec for collaborating ops loops, not a product-evolution system.

---

## 2. Self-adaptive systems / SEAMS

### Cheng et al. roadmap (2009)

**Source:** Betty H. C. Cheng et al., “Software Engineering for Self-Adaptive Systems: A Research Roadmap,” LNCS 5525, 2009. [DOI 10.1007/978-3-642-02161-9_1](https://doi.org/10.1007/978-3-642-02161-9_1)

- Classification: **adjacent**
- Matches: feedback loops as first-class engineering objects
- Missing: Q1 as product demand, Q3/Q5 as shared source promotion

### de Lemos et al. second roadmap (2013)

**Source:** Rogério de Lemos et al., LNCS 7475, 2013. [DOI 10.1007/978-3-642-35813-5_1](https://doi.org/10.1007/978-3-642-35813-5_1)

- Classification: **adjacent**
- Matches: runtime V&V and decentralized control (useful ancestry for Q4)
- Missing: Q1, Q3, Q5

### Salehie and Tahvildari survey (2009)

**Source:** Mazeiar Salehie and Ladan Tahvildari, “Self-adaptive software: Landscape and research challenges,” *ACM TAAS* 4(2), 2009. [DOI 10.1145/1516533.1516538](https://doi.org/10.1145/1516533.1516538)

- Classification: **adjacent**
- Matches: what / how / when / where taxonomy of adaptation
- Missing: Vibeware as a named *what* (product source) and *how* (qualified-friction pipeline)

### Weyns industry survey (2023)

**Source:** Danny Weyns et al., “Self-Adaptation in Industry: A Survey,” *ACM TOSEM*, 2023. [DOI 10.1145/3589227](https://doi.org/10.1145/3589227)

- Classification: **adjacent**
- Finding: industry self-adaptation is real and is **ops** (elastic cloud, server management), not product-source evolution.

SEAMS evaluation almost never uses a real product population as the adaptation oracle (Gerostathopoulos, Vogel, Weyns et al., SEAMS 2021).

---

## 3. LLM-in-MAPE-K (2024–2026)

### Donakanti et al. — MSE-K (2024)

**Source:** Raghav Donakanti, Prakhar Jain, Shubham Kulkarni, Karthik Vaidhyanathan, “Reimagining Self-Adaptation in the Age of Large Language Models,” arXiv:2404.09866. [DOI 10.1109/ICSA-C63560.2024.00036](https://doi.org/10.1109/icsa-c63560.2024.00036)

- Classification: **adjacent**
- Matches: LLM generates adaptation strategies from runtime logs/metrics/traces
- Missing: Q1, Q5. Execute remains adaptation of the running system.

### Zhang et al. — Microsoft ACV + LLMs (2024)

**Source:** Zhiyang Zhang et al., “The Vision of Autonomic Computing: Can LLMs Make It a Reality?,” arXiv:2407.14402. [Microsoft page](https://www.microsoft.com/en-us/research/publication/the-vision-of-autonomic-computing-can-llms-make-it-a-reality/)

- Classification: **adjacent**
- Matches: hierarchical multi-agent MAPE for microservice *maintenance*; claimed Level 3 of an ops-autonomy taxonomy
- Missing: Q1, Q3 as product features, Q5. Signal is chaos-injected faults on a demo shop.

### Esposito et al. — Agentic AI + MAPE-K (2025)

**Source:** Matteo Esposito et al., “Autonomic Microservice Management via Agentic AI and MAPE-K Integration,” arXiv:2506.22185. [DOI 10.1007/978-3-032-04403-7_11](https://doi.org/10.1007/978-3-032-04403-7_11)

- Classification: **strong partial on Q6**
- Matches: human-in-the-loop when an “autonomic threshold” α is crossed; execute via Ansible/CLI
- Missing: Q1–Q5. Risk classes are ops, not product-change qualification. The α-threshold is a usable ancestor of boundary-only intervention.

### Kara et al. — ACSOS 2026

**Source:** Kerem Kara et al., “Large Language Models for Self-Adaptive Systems: Feedback-Loop Integration and Agentic Adaptation,” ACSOS 2026. [KITopen](https://publikationen.bibliothek.kit.edu/1000194592)

- Classification: **adjacent**
- Finding: MAPE-embedded LLM is more controllable; end-to-end agentic loop is more flexible and less predictable. Architecture paper, not a product pipeline.

### Alves et al. — MAPER (SEAMS 2026)

**Source:** Lucas Vieira Alves et al., “MAPER: Extending MAPE-K with LLM-Based Reasoning…,” SEAMS 2026.

- Classification: **adjacent**
- Missing: Q1, Q5. Evaluated on the SWIM web-app exemplar.

### Sanwouo, Quinton, Temple — AWARE (FSE 2025)

**Source:** “Breaking the Loop: AWARE is the New MAPE-K,” FSE Companion 2025. [DOI 10.1145/3696630.3728512](https://doi.org/10.1145/3696630.3728512)

- Classification: **adjacent**
- Difference: rewrite of MAPE sequencing. No population demand.

---

## 4. Online controlled experimentation

### Kohavi, Henne, Sommerfield (KDD 2007)

**Source:** “Practical Guide to Controlled Experiments on the Web: Listen to Your Customers not to the HiPPO.” [DOI 10.1145/1281192.1281295](https://doi.org/10.1145/1281192.1281295)

- Classification: **strong partial on Q1 and Q4**
- Matches: real users as a causal oracle; keep/kill of a shipped treatment
- Missing: Q3. Humans author the variants.

### Kohavi, Longbotham, Sommerfield, Henne (2009)

**Source:** “Controlled experiments on the web: survey and practical guide,” *Data Mining and Knowledge Discovery* 18: 140–181. [DOI 10.1007/s10618-008-0114-1](https://doi.org/10.1007/s10618-008-0114-1)

- Classification: **strong partial on Q1 and Q4**
- Note: they already say organizations with a clear overall evaluation criterion can “evolve their systems with automated optimizations.” Treatments are still human-authored.

### Kohavi, Tang, Xu (2020)

**Source:** *Trustworthy Online Controlled Experiments*, Cambridge University Press. [CUP](https://www.cambridge.org/core/books/trustworthy-online-controlled-experiments/D97B26382EB0EB2DC2019A7A7B518F59)

- Classification: **strong partial on Q4**
- Difference: industrializes validate → promote. Does not generate the candidate from qualified friction.

OCE is the industrial gold standard for funnel stages 4–5. It is not the generator.

---

## 5. Feature flags, canaries, progressive delivery

### Humble and Farley (2010)

**Source:** *Continuous Delivery*, Addison-Wesley.

- Classification: **adjacent**
- Matches: promote/rollback machinery (Q5 mechanism)
- Missing: Q1 as demand, Q3

### Hodgson / Fowler feature toggles (2017)

**Source:** Pete Hodgson, “Feature Toggles (aka Feature Flags),” [martinfowler.com](https://martinfowler.com/articles/feature-toggles.html)

- Classification: **adjacent**
- Difference: runtime *selection* among already-written paths. Experiment toggles are how a validated candidate is exposed. They are not how it is born.

### Governor — Progressive Delivery (2018)

**Source:** James Governor, “Towards Progressive Delivery,” [RedMonk](https://redmonk.com/jgovernor/towards-progressive-delivery/)

- Classification: **adjacent**
- Difference: industry name for safe ship. Not a demand-to-code pipeline.

---

## 6. Genetic improvement and search-based repair

### GenProg (2009 / 2012)

**Source:** Weimer, Nguyen, Le Goues, Forrest, ICSE 2009; Le Goues et al., *IEEE TSE* 38(1), 2012. [DOI 10.1109/TSE.2011.104](https://doi.org/10.1109/TSE.2011.104)

- Classification: **strong partial on Q3 and Q5-as-patch**
- Matches: automatic source mutation; validation against tests
- Missing: Q1, Q2, Q4 as population. Oracle is a test suite.

### Petke et al. GI survey (2018)

**Source:** Justyna Petke et al., “Genetic Improvement of Software: a Comprehensive Survey,” *IEEE TEVC* 22(3), 2018. [DOI 10.1109/TEVC.2017.2693219](https://doi.org/10.1109/TEVC.2017.2693219)

- Classification: **strong partial on Q3**
- Finding: fitness is almost never “did the real population stop experiencing this friction.”

### Petke, Harman, Langdon, Weimer — specialisation (2018)

**Source:** *IEEE TSE* 44(6): 574–594. [DOI 10.1109/TSE.2017.2702606](https://doi.org/10.1109/TSE.2017.2702606)

- Classification: **strong partial on Q3**
- Difference: closest GI paper to “steer by usage.” Usage is a static scenario set chosen by researchers.

GI proves source can be evolved when an oracle exists. The missing piece is a population-friction oracle, not the ability to mutate code.

---

## 7. 2025–2026 autonomous source-evolution systems

### Kitchen Loop (March 2026)

**Source:** Yannick Roy, “The Kitchen Loop: User-Spec-Driven Development for a Self-Evolving Codebase,” [arXiv:2603.25697](https://arxiv.org/abs/2603.25697). Code: [0xagentkitchen/kitchenloop](https://github.com/0xagentkitchen/kitchenloop)

- Classification: **strong partial on Q3, Q5, and pause-gate discipline**
- Matches: use → ticket → patch → gate → merge; claimed 1,094+ merged PRs; unbeatable tests; drain/starvation/pause gates
- Missing: **Q1**. Sensor is a synthetic “As a User × 1000” agent against a specification surface. Humans share a backlog/spec. They are not the demand sensor.

Best published system for long-running autonomous *source* evolution with ship discipline. Spec-steered and synthetic-user-steered, not population-qualified.

### MOSS (May 2026)

**Source:** Qianshu Cai et al., “MOSS: Self-Evolution through Source-Level Rewriting in Autonomous Agent Systems,” [arXiv:2605.22794](https://arxiv.org/abs/2605.22794)

- Classification: **strong partial on Q3 and Q5** (nearest academic generate-and-ship-from-evidence system)
- Matches: session evidence → locate/plan/implement → trial-worker replay → consent-gated container swap
- Missing: Q1 as a product *population*. Qualification is “this batch of failing chunks.” Reported lift is on a four-task benchmark batch, not organic multi-user traffic. Scope is the agent harness, not a shared product.

### EvoX Genesis (August 2026)

**Source:** Beichen Huang, Zhenyu Liang, Bowen Zheng, Ran Cheng, “Persistent Recursive Worlds Enable Autonomous Software Evolution,” [arXiv:2608.10450](https://arxiv.org/abs/2608.10450)

- Classification: **strong partial on Q3**
- Matches: finite-lived agents, persistent project history, validation-gated acceptance
- Missing: Q1, Q2, Q4. Demand is a human objective plus tests.

### Apeiron (ACL Findings 2026)

**Source:** Junyun Cheng et al., “Apeiron: A Scalable LLM-agentic Framework for Autonomous Full-lifecycle Demand-optimized Application Synthesis,” [ACL anthology](https://aclanthology.org/2026.findings-acl.188.pdf)

- Classification: **strong partial on Q3**
- Matches: named “demand-optimized” synthesis; demand-shift CI/CD
- Missing: **Q1**. Demand lists and CUA-as-Judge are simulated. User study N=18 correlates CUA with humans. Demand is not live telemetry.

Nearest named concept (“demand-optimized”) and the demand is synthetic.

### AlphaEvolve (DeepMind, 2025)

**Source:** Alexander Novikov et al., [arXiv:2506.13131](https://arxiv.org/abs/2506.13131)

- Classification: **strong partial on Q3**
- Matches: LLM-mutate source → evaluator → evolve; used on Google infra
- Missing: Q1. Oracle is a numeric evaluator.

### Darwin Gödel Machine (2025 / ICLR 2026)

**Source:** Jenny Zhang et al., [arXiv:2505.22954](https://arxiv.org/abs/2505.22954)

- Classification: **not comparable as product evolution**
- Difference: agent rewrites its own code; fitness is SWE-bench / Polyglot.

### SICA (2025)

**Source:** Maxime Robeyns, Martin Szummer, Laurence Aitchison, [arXiv:2504.15228](https://arxiv.org/abs/2504.15228)

- Classification: **not comparable**
- Difference: self-edit against benchmarks.

### Datadog BitsEvolve / Bits Code (industry, 2025–2026)

**Source:** Datadog engineering blogs: [self-optimizing system](https://www.datadoghq.com/blog/engineering/self-optimizing-system/), [fully autonomous optimization](https://www.datadoghq.com/blog/ai/fully-autonomous-optimization/), [Bits Code](https://www.datadoghq.com/blog/bits-code/)

- Classification: **strong partial on Q3 and Q5** (nearest *production* auto-ship from live evidence)
- Matches: BitsEvolve observes production traffic, evolves kernels, formally verifies, shadows, hot-swaps WASM. Bits Code turns telemetry into PRs for human review.
- Missing: Q1 as *product friction*. Evidence is data-plane fitness, not qualified UX demand. Bits Code stops at the candidate.

Treat as industry system, not a paper.

### Tero Multiverse (vendor blog)

**Source:** [Tero Multiverse architecture](https://app.tero.run/blog/multiverse-architecture)

- Classification: **unverified description**
- Claim: analytics friction → generated variants → synthetic screen → real-user A/B → merge PR
- Status: closest *commercial sketch* of the composed pipeline. No paper, no independent evaluation. Do not cite as established prior art.

---

## 8. Terminology: “Living Software”

### Jules White (3 August 2024)

**Source:** “Building Living Software Systems with Generative & Agentic AI,” [arXiv:2408.01768](https://arxiv.org/abs/2408.01768)

- Classification: **name collision**
- Genre: self-labeled opinion paper. No methods, no evaluation, five references.
- Definition: software is “dead” because it is static. “Living” systems use generative AI as a translator from human goals to computer actions. Pathway 1: GenAI writes traditional source faster. Pathway 2: agentic AI maps natural-language goals onto APIs at runtime.
- Shared Vibeware: **no**. Users are prompt-givers, not a selecting population. There is no telemetry, qualification, candidate, validation, or promotion pipeline.

### Taskade Genesis (October 2025 onward)

**Source:** [What Is Living Software?](https://www.taskade.com/wiki/genesis/living-software)

- Classification: **name collision** (productized category)
- Definition: persistent memory + embedded agents + continuous execution. “Describe the change in chat; the running app updates.”
- Shared Vibeware: **no**. Change is owner-in-chat or agent re-planning from that workspace’s memory.

### Jack Cheng, *Every* (17 April 2026)

**Source:** [every.to/p/living-software](https://every.to/p/living-software)

- Classification: **name collision**
- Definition: tool-like vs living as a user-expectation taxonomy. Living ≈ non-deterministic / agentic products.
- Difference: Cheng wants a freeze button on feature-churning apps. Opposite pressure from a promotion pipeline.

### Aaron White manifesto

**Source:** [living.software](https://living.software)

- Classification: **name collision**
- Difference: explicitly anti “updates pushed from afar.” Adaptation is per-user.

### Nicolae Rusan (16 September 2024)

**Source:** [nicolaerusan.com/writing/living-sofware](https://www.nicolaerusan.com/writing/living-sofware)

- Classification: **strongest essay neighbor**
- Matches: observe → learn → modify the codebase; analytics merged with source control
- Missing: Q2 as a named gate, Q4 as causal validation, Q6 as the control rule. Framed as AI self-improvement, not population selection.

### Steve Yegge (17 January 2007)

**Source:** “The Pinocchio Problem,” [steve-yegge.blogspot.com](https://steve-yegge.blogspot.com/2007/01/pinocchio-problem.html)

- Classification: **name collision** (older, load-bearing)
- Definition: living software has a command shell, extension language, advice system, plug-in architecture, and should not reboot to grow.
- Difference: runtime extensibility. Not telemetry-selected shared releases.

### Company and vertical collisions

Living Software, Inc. (DairyLive herd software). Living Software Ltd (UK, 2014). “Senior living software” / assisted-living EHR. None are the agentic category.

### Trademark and patent (search, not clearance)

- LIVINGSOFTWARE (USPTO 78046111, 2001): abandoned 12 June 2002, no statement of use.
- LIVINGSOFT (reg. 1,732,658, 1992): cancelled §8, 24 May 1999.
- No live USPTO registration for “LIVING SOFTWARE” found in 2024–2026 searches. No Taskade filing found.
- No patent using “living software” as a defined claim term found. Related metaphors exist (self-generating software). The field is not empty of related *ideas*.

### Bitdefender — “Vibeware” malware (2026)

**Source:** Bitdefender Labs, “APT36: A Nightmare of Vibeware,” [businessinsights.bitdefender.com](https://businessinsights.bitdefender.com/apt36-nightmare-vibeware). Trade repeats: Computer Weekly, 9 March 2026.

- Classification: **name collision**
- Definition: vibe-coded malware; high-volume, mediocre, disposable implants (APT36 / Transparent Tribe).
- Shared Vibeware: **no**. Different object. The word is used here for sensitive software with a live update funnel, not for AI-generated junk malware.

**Naming decision:** the 2024–2026 “Living Software” cluster means runtime agentic adaptivity. Bitdefender’s 2026 “vibeware” means vibe-coded malware. Shared Vibeware here is a selection-and-promotion architecture for shared updates, plus a related personal loop (one key, one instrument). Those are different control boundaries. The article uses **Vibeware** and treats both “living software” and the malware coinage as foils. Do not over-explain either.

---

## 9. Continuous discovery, PLG, and RLHF

### Torres — Continuous Discovery Habits (2021)

**Source:** Teresa Torres, Product Talk LLC.

- Classification: **strong partial on Q1 and Q2** (human-operated)
- Matches: weekly research → opportunities → assumption tests → backlog
- Missing: Q3, Q5 as automation. This is the human-operated version of stages 1–3.

### Bush — Product-Led Growth (2019)

- Classification: **adjacent**
- Difference: usage as a *growth* loop, not an engineering loop.

### Christiano et al. — RLHF (2017) and Ouyang et al. — InstructGPT (2022)

**Source:** [arXiv:1706.03741](https://arxiv.org/abs/1706.03741); [arXiv:2203.02155](https://arxiv.org/abs/2203.02155)

- Classification: **distant analog**
- Matches: a population of evaluators shapes a deployed artifact
- Missing: Q5 as source. Artifact is weights. Signal is ranked completions, not product friction.

---

## 10. Evidence used for the constraint model

These are not prior art for the *idea*. They constrain any implementation.

### Agentic security and maintenance (2026)

- Sakib, Banik, Jadliwala, “Trust but Verify?,” [arXiv:2607.12428](https://arxiv.org/html/2607.12428): 38.9% of 4,022 agent PRs on high-risk paths contained at least one security smell; 82.3% of smells were supply-chain integrity; 99.6% of critical smells were credentials; 81.1% of genuine leaked secrets reached integration with no review comment; humans introduced 67.6% of those genuine secrets.
- Xia and Miller, “Do These Violent Delights Have Violent Ends?,” [arXiv:2607.09902](https://arxiv.org/abs/2607.09902): agentic code showed about 46–51% higher corrective-maintenance hazard; each 10 percentage-point rise in a project’s no-review rate associated with roughly 6% higher agentic maintenance burden.
- Liu et al., “Debt Behind the AI Boom,” [arXiv:2603.28592](https://arxiv.org/abs/2603.28592): 22.7% of tracked AI-introduced issues still present at HEAD; AI introduced about 1.5× as many security issues as it fixed.
- Sawada et al., EASE 2026, [arXiv:2605.06464](https://arxiv.org/abs/2605.06464): humans performed 83.21% of later commits on agent-generated files.

### Metric failure

- Goodhart (1975): observed regularities collapse when used for control.
- Strathern (1997): when a measure becomes a target, it ceases to be a good measure.
- Kohavi et al. (2009, 2012) and Kohavi, Tang, Xu (2020): novelty, peeking, Simpson’s paradox, surrogate OECs, power (`n ≈ 16σ²/δ²` per variant for ~80% power).
- Chaney, Stewart, Engelhardt (RecSys 2018) and Mansoury et al. (CIKM 2020): recommendation feedback loops homogenize users and amplify popularity bias.
- Milli et al. (2023), Twitter Heavy Ranker weights: engagement ranker ≠ stated preference.

### Untrusted telemetry

- GDPR Arts. 5(1)(b)–(c): purpose limitation and data minimisation.
- Greshake et al., “Not What You’ve Signed Up For,” AISec 2023, [arXiv:2302.12173](https://arxiv.org/abs/2302.12173): models do not distinguish data from instructions.
- Chang et al., USENIX Security 2026 / [arXiv:2601.07072](https://arxiv.org/abs/2601.07072): retrieval-aware poisoning; a single poisoned email coerced tool use in up to 80% of trials.

### Runaway automation

- SEC, *In the Matter of Knight Capital Americas LLC*, Rel. 34-70694 (16 October 2013): leftover flag + incomplete deploy + no halt, loss >$460 million in ~45 minutes.
- Hoyos et al., “On the Removal of Feature Toggles,” *Empirical Software Engineering* 2021: leftover toggles as explicit debt.
- Lazer et al., “The Parable of Google Flu,” *Science* 2014: proxy drift when the instrument changes.
- Kitchen Loop pause gates: oracle-fail pause, PR-backlog drain, execute-starvation stop, sealed UAT by a weaker context-free evaluator.

---

## Master comparison

| Item | Q1 pop. sensor | Q2 qualify | Q3 generate source | Q4 pop. validate | Q5 promote source | Q6 boundary-only |
|---|---|---|---|---|---|---|
| MAPE-K / SEAMS / LLM-MAPE | no | ops | no | no | no | partial (α in Esposito) |
| Kohavi OCE | yes | human | no | yes | human | n/a |
| Flags / canary / CD | expose | human | no | partial | yes (human code) | n/a |
| GI / GenProg / AlphaEvolve / DGM | no | no | yes | tests/bench | lab/infra | no |
| Kitchen Loop | no (synthetic) | spec gaps | yes | tests + UAT | yes | pause gates |
| Apeiron | no (simulated) | demand list | yes | CUA | lab | no |
| MOSS | partial (sessions) | batch | yes | replay | consent swap | user consent |
| Genesis | no | human goal | yes | tests | accept | no |
| White / Taskade “living” | no | no | runtime / chat | no | no | “not for flying a plane” |
| Bitdefender “vibeware” | no | no | malware | no | no | n/a |
| Rusan essay | implied | no | implied | no | implied | no |
| BitsEvolve | traffic, not UX | fitness | yes | shadow | WASM swap | verify |
| Bits Code | telemetry | human | yes | no | PR only | human merge |
| Tero (unverified) | claimed | claimed | claimed | claimed A/B | claimed | unknown |
| **Vibeware (shared funnel)** | **yes** | **yes** | **yes** | **yes** | **yes** | **yes** |

---

## What can be claimed

1. The **composed** funnel (Q1–Q6) is unclaimed as a named, implemented, evaluated system.
2. **Living Software** already names runtime intent-translation (White 2024) and a productized category (Taskade 2025–26). Cite and distinguish. Do not reclaim the phrase. Bitdefender’s **vibeware** is malware coinage. Cite as name collision. Do not spend a section on it.
3. MAPE-K is the loop ancestor. The managed object and the sensor both change.
4. Online controlled experiments and feature flags are the validation/ship ancestor. Humans still author treatments.
5. GI, AlphaEvolve, Genesis, DGM, and SICA prove source can evolve without users. Fitness is tests, benchmarks, or a human objective.
6. Kitchen Loop and Apeiron prove an autonomous use → code → gate loop with **fake** users.
7. MOSS is the nearest academic generate-and-ship-from-evidence system and still misses population qualification.
8. BitsEvolve is the nearest production auto-ship-from-live-evidence system and the evidence is kernel performance.
9. The sales-pipeline metaphor for software change, and the phrases “population-steered” / “user-steered evolution,” have no verified prior. Apeiron’s “demand-optimized” is the nearest named concept and uses simulated demand. “Vibeware” as sensitive software / shared update funnel has no verified product-evolution prior. Security-press hits are the malware coinage.
10. Tero is a vendor sketch, not academic prior art.

**Hardest overclaim to avoid:** “nobody uses real users to change software.” They do, through A/B tests, flags, discovery, and product-led growth. The gap is automatic generation and promotion of **source** from **qualified population friction**, not the existence of user signal.
