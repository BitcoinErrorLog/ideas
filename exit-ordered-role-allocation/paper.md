# Exit-Ordered Role Allocation

## Precommitted and Late-Generated Keys with Residual Role Non-Assignment

## Abstract

Most key-assignment systems bind a credential or cryptographic secret to a terminal controller before that controller begins acting. Anonymous systems may conceal the binding, but usually still create a distinct credential, hidden permutation, or owner-key relation in advance.

A narrower allocation functionality begins with a finite cohort, no exclusive mapping between individual members and a committed sequence of terminal roles, and an authorized allocation event that assigns one selected member the next ordinal role:

```
C[n] --allocate(d)→ I[n](d) + C[n-1].
```

The selected member becomes the terminal controller of `I[n]`. Every survivor remains unassigned with respect to `I[1],…,I[n-1]`. This property is **residual role non-assignment**. It must be distinguished from anonymity and unlinkability: members may remain physically and cryptographically identifiable while still lacking an exclusive future-role assignment.

Two profiles separate materially different functionality. In the **late-key profile**, the terminal key is created during allocation. In the **precommitted-key profile**, role public keys are committed in advance while their private material remains under collective custody until transfer to a selected terminal controller. The second profile is naturally constructed from threshold custody, selected-recipient transfer or resharing, proactive refresh, and a serialization mechanism.

The model includes a candidate ideal functionality, threat-model dimensions, safety and liveness properties, transition phases, trust boundaries, and comparisons with late key generation, threshold escrow, anonymous allocation, YOSO role assignment, dynamic proactive secret sharing, anonymous credentials, one-time prekeys, MLS KeyPackages, stateful signatures, transparency logs, and provisioning systems.

Primitive-level novelty remains unproven. Exit-ordered role allocation is a potentially useful systems abstraction and security objective for applications that require finite roles, event-derived ordinal assignment, no prior exclusive allocation, and continued role neutrality of unused members.

---

## 1. Introduction

Identity systems usually establish:

```
object → identity → history.
```

A hardware signer generates a key. A server receives a UUID. A security token has a serial number. A threshold participant receives an identifier. Later events are recorded against that preexisting identity.

The alternative is:

```
role-eligible cohort → authorized event → terminal role.
```

Consider `N` devices initialized as cohort `C[N]`. The system commits to roles:

```
I[N],I[N-1],…,I[1].
```

Before allocation, no device is exclusively assigned to a role. The first accepted allocation selects device `d[N]`, which becomes terminal controller of `I[N]`. The survivors form `C[N-1]` and remain eligible for every remaining role. Repeating the process yields:

```
C[N]
→ I[N](d[N])+C[N-1]
→ I[N](d[N])+I[N-1](d[N-1])+C[N-2].
```

The ordinal is evidence about accepted allocation history:

> `d[i]` is the member selected at rank `i` under cohort commitment `Γ`.

The functionality is useful only under a restricted requirement set:

1. the role supply is finite and meaningful;
2. allocation order matters;
3. terminal control must persist after allocation;
4. individual members must not receive exclusive roles in advance;
5. unused members must remain role-neutral;
6. accepted state must prevent or expose duplicate rank consumption.

If an application can discard one of these requirements, a simpler mechanism will often dominate.

### 1.1 Defined scope

The model separates and defines:

- a precise separation between late-generated and precommitted role keys;
- terminology distinguishing role non-assignment, exchangeability, anonymity, and unlinkability;
- an ideal functionality for serialized, event-ordered allocation;
- safety and liveness goals for terminal assignment and survivor state;
- explicit treatment of rollback, cloning, partition, atomic transfer, threshold evolution, and the final transition;
- a prior-art comparison focused on assignment timing rather than only mechanism similarity; and
- a strict application test that excludes examples solved by counters, fresh keys, credentials, or ordinary enrollment.

### 1.2 Boundaries

The following do not follow from the construction:

- that precommitted keys do not exist;
- that authority can exist without any controller;
- that role non-assignment implies anonymity;
- that secret sharing creates noncopyable digital scarcity;
- that an offline cohort can remain live during arbitrary partitions while preserving one global history;
- that a certificate chain prevents forks without a finality mechanism;
- that the construction sketches are complete under malicious security; or
- that exit-ordered role allocation is already established as a new cryptographic primitive.

---

## 2. Terminology

### 2.1 Role

A **role** is a committed policy position with ordinal `i` and policy `Pol[i]`. A role may contain or reference a public key.

### 2.2 Member

A **member** is a device, process, or other object eligible to receive a role while it remains in the cohort.

Members need not be byte-identical or anonymous. They may contain independent entropy, serial numbers, attestation keys, and distinct shares.

### 2.3 Terminal controller

A **terminal controller** is the member that receives exclusive individual control of an allocated role’s private key or equivalent non-exportable capability.

This term excludes the claim that nobody controls a precommitted role before allocation. A threshold quorum is a collective controller in the operational security sense.

### 2.4 Authorized allocation transition

An **authorized allocation transition** is an accepted state change that selects one eligible member, assigns the current rank, advances cohort state, and produces evidence.

Silence, disappearance, radio loss, battery failure, or network partition is not authorization.

### 2.5 Residual role non-assignment

After allocating `I[n]`, survivors in `C[n-1]` have no exclusive assignment to any role in:

```
{I[1],…,I[n-1]}.
```

This is the central property.

For `n=1`, non-assignment remains syntactically true until the final transition, but the only remaining member is already the logically determined future controller. Role exchangeability and counterfactual uncertainty are meaningful only while at least two eligible survivors remain.

### 2.6 Role exchangeability

Survivors are **role-exchangeable** when permuting honest surviving members yields an indistinguishable protocol view, conditioned on declared side information and a correspondingly permuted future allocation schedule.

### 2.7 Anonymity and unlinkability

**Anonymity** means an adversary cannot identify a participant within an anonymity set.

**Unlinkability** means an adversary cannot connect pre-allocation observations to a later terminal role beyond declared leakage.

Neither follows from residual role non-assignment.

### 2.8 Accepted history

An **accepted history** is the canonical or locally accepted chain of transition certificates under a declared finality rule.

The finality rule may prevent forks or merely make them detectable. These are different guarantees.

---

## 3. Two Profiles

### 3.1 Late-key profile

In profile EORA-L, no terminal public key exists before allocation.

The selected member generates or derives:

```
(sk[n],PK[n])←KeyGen(1^λ).
```

The serializer issues an ordinal certificate binding `PK[n]` to cohort, rank, predecessor, and successor state.

This profile provides no preexisting terminal key inventory. It is the natural baseline for applications that can register public keys at allocation time.

### 3.2 Precommitted-key profile

In profile EORA-P, setup creates:

```
SK[1],…,SK[N]
```

and:

```
PK[i]=Public(SK[i]).
```

The public keys are committed under cohort commitment `Γ`. No single member initially controls a full `SK[i]`. Private material remains under collective custody until the accepted allocation for rank `i`.

This profile supports external policies that require terminal public keys before physical allocation.

It also creates the main concentration risk:

> The collective custody mechanism controls the future namespace before allocation.

The accurate claim is therefore:

> No individual terminal controller exists before allocation.

---

## 4. Strongest Baselines

A meaningful comparison must use the strongest conventional alternative, not only pre-labeled devices.

### 4.1 Baseline A: late key generation plus ordinal certificate

Maintain canonical counter `n`, active-set commitment `M[n]`, spent-nullifier commitment `Z[n]`, and durable pending-transition state. On authorized allocation:

1. verify eligible member `d` and require its one-time membership nullifier to be unused;
2. let `d` generate `(sk[n],PK[n])`, durably seal `sk[n]`, and prove possession;
3. atomically consume the nullifier, remove `d` from the active set, commit the canonical terminal certificate, and advance `H[n]→ H[n-1]`;
4. recover interrupted executions to abort before usable delivery or to the same committed transition afterward.

This realizes the core EORA-L functionality under a trusted or consensus-backed allocator with durable delivery and atomic eligibility removal.

### 4.2 Baseline B: threshold escrow plus selected-recipient transfer

Generate `SK[1],…,SK[N]`, commit their public keys, and hold each secret under threshold custody. On allocation `n`:

1. serialize the request;
2. transfer or reshare `SK[n]` to selected member `d`;
3. advance canonical state;
4. refresh or erase survivor material;
5. issue a certificate.

This is the natural EORA-P construction.

### 4.3 Baseline C: anonymous member plus decrementing allocator

When recipient anonymity matters:

1. prove cohort membership anonymously;
2. present a nullifier preventing repeat claims;
3. atomically fetch and decrement `n`;
4. generate or receive the corresponding terminal key;
5. obtain a rank certificate.

Anonymous credentials, Semaphore-like membership proofs, anonymous counting tokens, e-cash mechanisms, or a smart contract can supply variants.

### 4.4 Baseline D: blind credentials or e-cash

If the objective is only finite unlinkable issuance or redemption, blind credentials and e-cash are better-targeted mechanisms.

Exit-ordered allocation becomes relevant only when the application additionally needs durable terminal key control, ordinal history, physical allocation, or precommitted external roles.

---

## 5. Related Work

### 5.1 Anonymous allocation and hidden permutations

Soleimanian’s *Anonymous Random Allocation and Its Applications* formalizes anonymous one-to-one assignment, uniqueness, fairness, confidentiality, integrity, and later revelation [1]. Secret-permutation protocols similarly assign hidden unique slots [2].

These systems create a complete mapping during allocation or shuffle. The mapping may be hidden, but it exists before later use.

The candidate distinction here is:

> The future event schedule creates the member-role relation rather than revealing a hidden preexisting permutation.

That distinction requires formal treatment to become more than an implementation description.

### 5.2 YOSO and future role assignment

YOSO assigns ephemeral abstract roles to physical machines and limits each role’s exposure [3]. Encryption to the Future allows messages to target anonymous committees selected by future blockchain state [4]. Related work transfers state and authenticates messages from anonymous future role holders [5–7].

This is the closest vocabulary for future-event role assignment.

YOSO roles are normally independent protocol responsibilities. Nonwinners do not remain in one finite, shrinking allocation pool. EORA’s additional invariant is residual role non-assignment of that survivor pool.

### 5.3 Dynamic proactive secret sharing and resharing

CHURP, Shanrang, DyCAPS, and related dynamic proactive-secret-sharing protocols refresh and transfer secret shares across changing committees [8–11]. Verifiable secret redistribution and non-interactive key resharing preserve a secret or public key while changing shareholders [12–13].

These mechanisms are directly relevant to EORA-P. They do not define the ordinal allocation rule.

### 5.4 Anonymous bounded consumption

Anonymous Counting Tokens enforce issuance limits while retaining unlinkability [14]. One-show credentials, compact e-cash, periodic `n`-times authentication, and newer rate-limited tokens provide finite or rate-bounded anonymous use [15–18].

These mechanisms cover one-time or bounded consumption. Their tokens are normally owner-bound before use.

### 5.5 One-time key inventories

Signal X3DH publishes batches of one-time prekeys that a server supplies and deletes [19]. MLS KeyPackages are prepublished introduction objects intended for single use, with an availability exception for last-resort packages [20–21].

These are strong analogies for consumable key inventory. Each key is already bound to its owner or client.

### 5.6 Stateful and indexed keys

XMSS commits a finite indexed set of one-time signing keys and tracks the next unused leaf [22]. Key-regression and forward-secure systems similarly define ordered key evolution while depending on correct state management and erasure [23–24].

XMSS is close to:

```
event i → consume capability i.
```

It does not assign different members to those indices.

### 5.7 Transparency and fork consistency

Certificate Transparency, CONIKS, and newer key-transparency work provide append-only evidence, consistency proofs, gossip, and split-view detection [25–27].

These systems can serialize or expose EORA history. They do not create the role assignment.

### 5.8 Anonymous renaming

Anonymous renaming assigns unique names to initially symmetric participants under explicit communication and failure assumptions [38]. It breaks symmetry across the population rather than preserving an unassigned survivor set after each externally authorized allocation.

### 5.9 Provisioning systems and patents

Provisioning prior art includes:

- generic session-key pools not bound to destinations before selection [28];
- authenticated “next available” encrypted device-key sets [29–30];
- event-triggered assignment to unassigned gift objects [31];
- physical tokens that assign roles and role seniority [32];
- SIM, eSIM, secure-element, and device-onboarding personalization [33–36]; and
- physical threshold custody for wallet secrets [37].

References [28–37] and the request-ordinal allocator in [40], as of 19 August 2026, contain no issued independent claim combining finite committed stock, no prior exclusive role assignment, authorized event-ordinal selection, irreversible depletion, residual role non-assignment, threshold custody, and physical embodiment. The strongest obviousness combination is generic key inventory, request-ordinal allocation, late physical personalization, and threshold custody.

No legal novelty or freedom-to-operate conclusion follows from this technical comparison.

---

## 6. System Model

Let:

- `λ` be the security parameter;
- `N` be initial capacity;
- `D={d[1],…,d[N]}` be initialized members;
- `R={1,…,N}` be terminal ranks;
- `ctx` be application context;
- `Pol[i]` be policy for rank `i`;
- `Ser` be the serialization mechanism;
- `cid` be the cohort identifier.

### 6.1 Public commitment

For EORA-P:

```
Γ=
Com(
cid,
N,
ctx,
(Pol[i]) for i=1…N,
(PK[i]) for i=1…N,
M[N],
Ser
).
```

EORA-L omits `(PK[i])`.

### 6.2 Public state

At epoch `e=N-n`:

```
H[n]=
(cid,n,e,h[n],Γ,M[n],Z[n]),
```

where `h[n]` commits to accepted history ending at `C[n]`, `M[n]` commits to the active member set, and `Z[n]` commits to spent membership nullifiers.

### 6.3 Member state

Member `d` holds:

```
σ[d,n]=
(ρ[d],ω[d,n],ctr[d,n]),
```

where:

- `ρ[d]` is independent entropy;
- `ω[d,n]` is cohort or threshold state;
- `ctr[d,n]` is optional monotonic state.

### 6.4 Terminal record

Successful allocation emits canonical terminal record `τ[n]` defined in Section 14. Its required abstract projection is:

```
proj(τ[n])=
(
cid,
n,
PK[n],
H[n],
H[n-1],
ν[n],
χ[n]
),
```

where `ν[n]` is a unique transition nonce and `χ[n]` authenticates every canonical record field preceding `χ[n]`, including authorization and succession.

The terminal holder receives:

```
θ[n]=(SK[n],τ[n],Pol[n])
```

or an equivalent non-exportable capability.

---

## 7. Ideal Functionality

Let `F[EORA, linearizable]` maintain:

- active member set `U`, initially `D`;
- remaining rank `n=N`;
- canonical history head `h`;
- assignments `A`, initially empty;
- spent ranks `S[spent]`, initially empty;
- spent membership nullifiers `Z`, initially empty;
- pending transition `p∈{⊥,record}`;
- EORA-P role handles `K[i]`, initialized with the committed roles;
- corruption set `C`.

EORA-L creates `K[n]` during allocation rather than maintaining preexisting role handles.

On authorized request `(Allocate,d)`:

1. derive membership nullifier `η[d]` and require `d∈ U`, `η[d]∉ Z`, `n>0`, `n∉ S[spent]`, and `p=⊥`;
2. obtain a unique linearization token from `Ser`;
3. set `p=(h,n,d,ν)`;
4. create `K[n]` for EORA-L or prepare transfer of EORA-P handle `K[n]`;
5. stage assignment `(n,d)`, successor active set `U'=U∖{d}`, and refreshed survivor state;
6. durably deliver or seal `K[n]` for `d` and obtain terminal acknowledgment;
7. compute `h'=H(h,n,d,ν)`;
8. atomically commit:

```
A← A∪{(n,d)},

S[spent]← S[spent]∪{n},

Z← Z∪{η[d]},

U← U',

h← h',

n← n-1;
```

9. output terminal record and successor state;
10. set `p=⊥`.

Before usable terminal delivery in step 6, an interruption may either:

- leave no externally accepted effect; or
- permit deterministic recovery to the same transition.

After the EORA-P secret becomes usable by `d`, rollback to a reusable predecessor is unsafe because `d` cannot be forced to forget `K[n]`. Recovery must complete the same assignment and successor transition.

A weaker functionality `F[EORA, detectable]` may permit equivocation but must produce publicly verifiable fork evidence. It is not equivalent to linearizable EORA.

### 7.1 Leakage modes

- **Public order:** rank, order, and recipient are public.
- **Blind allocation:** rank and transition are public; recipient is hidden.
- **Private cohort:** only minimal terminal evidence becomes public.
- **Auditable issuance:** complete rank history is public.

---

## 8. Protocol Syntax

An implementation defines:

```
Π=
(
Init,
Request,
Authorize,
Prepare,
Transfer,
Refresh,
Commit,
Recover,
VerifyState,
VerifyTerminal,
ProveFork,
Use
).
```

### 8.1 Initialization

```
Init(1^λ,N,ctx,Pol)
→
(Γ,H[N],{σ[d,N]}).
```

EORA-P requires verifiable DKG or auditable MPC if secrecy against the initializer is claimed. A dealer who generates every `SK[i]` can retain the future namespace.

### 8.2 Authorization

```
Authorize(H[n],d,evidence)
→
a[n].
```

The serializer linearizes at most one request against `H[n]`:

```
a[n]=
(
cid,
H(H[n]),
n,
commit(d),
η[d],
H(membershipProof[d]),
ν[n]
).
```

### 8.3 Prepare

The parties validate eligibility, policy, predecessor state, threshold availability, and the terminal delivery channel.

### 8.4 Transfer

For EORA-L, the selected member generates or derives a fresh key.

For EORA-P, the cohort performs verifiable selected-recipient transfer or private reconstruction of the secret underlying `PK[n]`.

### 8.5 Refresh

Survivors erase obsolete state and obtain fresh state for remaining roles.

### 8.6 Commit

Commit produces:

```
(H[n-1],τ[n],θ[n]).
```

### 8.7 Recovery

Recovery must classify partial executions and deterministically resolve to:

- abort with reusable predecessor only before usable terminal capability delivery; or
- the same committed terminal record and successor state.

After usable EORA-P secret delivery, recovery must complete the same assignment because the recipient cannot be forced to forget the secret.

---

## 9. Security Properties

### 9.1 Correctness

Honest authorized allocations produce valid terminal roles in schedule order. Terminal records verify and survivors retain usable state.

### 9.2 Schedule sensitivity

For accepted schedule:

```
S=(d[N],d[N-1],…,d[1]),
```

holder of rank `i` is `d[i]`.

Exchanging two members in the schedule exchanges their ranks.

This is correctness, not fairness. Whoever controls the schedule controls recipient-role mapping.

### 9.3 No latent exclusive assignment

Define the exposed setup view as the public commitment, manufacturer-visible metadata, protocol transcripts, and private states obtained through corruption queries up to the declared threshold. No latent exclusive assignment requires a simulator that generates an indistinguishable exposed view from cohort parameters and allowed leakage without sampling a hidden member-role permutation.

For experiments `Real[A]` and `Ideal[A,Sim]`:

```
| Pr[Real[A]=1] - Pr[Ideal[A,Sim]=1] | ≤ negl(λ).
```

The corruption interface, threshold, and leakage function must be fixed by each realization.

The experiment continues through adaptively selected allocation events. The adversary may request permitted state openings and corruptions, observe terminal outputs, and choose the next eligible member subject to the declared authorization policy. Accepted assignments must follow that challenged schedule rather than any concealed setup-time permutation.

Before rank `i` commits, no eligible individual member or allowed corrupted coalition should exercise `I[i]`.

This claim is limited to protocol state. It cannot prove that a manufacturer kept no external private mapping.

### 9.4 Survivor role exchangeability

For `n≥2`, permutation `π` of honest survivors, and correspondingly permuted future schedule:

```
View[A](Exec(U,S))
≈
View[A](Exec(π(U),π(S))).
```

The definition must condition on serial numbers, physical observations, attestation, and network metadata declared visible.

### 9.5 Future-role secrecy

For every unissued rank `i≤ n`, an adversary within the corruption bound cannot recover `SK[i]`, produce an accepted role signature, or learn useful secret information beyond public `PK[i]`.

### 9.6 Premature-use resistance

The adversary wins if an application accepts an action under rank `i` before `τ[i]` commits.

A precommitted bare public key does not provide this property. If Bitcoin or another relying system accepts signatures under `PK[i]`, it will accept a valid signature regardless of off-chain EORA state.

Premature-use resistance therefore requires:

- policy verification of `τ[i]`;
- script-level coupling to equivalent state;
- cohort refusal to sign before allocation;
- or trusted hardware enforcing the state machine.

### 9.7 Terminal isolation

Compromise of issued terminal roles does not expose unissued roles.

### 9.8 At-most-once monotonic consumption

An adversary must not produce:

- two accepted terminal records for one rank;
- two accepted successors from one predecessor;
- a history that reuses or skips ranks contrary to policy;
- or more than `N` accepted terminal roles.

This property requires a shared acceptance rule.

### 9.9 Recoverable atomicity

The system must not leave an accepted state where:

- the terminal key is usable but `C[n]` remains current;
- `C[n-1]` is accepted but no unique terminal controller can recover `SK[n]`;
- predecessor and successor both continue allocating;
- or two devices receive usable copies of `SK[n]`.

Strong fair exchange is generally unavailable under arbitrary asynchronous crash. Recoverable atomicity is the more realistic objective, with transaction-commit and fair-exchange assumptions made explicit [41–42].

### 9.10 Rollback resistance

After `H[n-1]` is accepted, restoring prior snapshots must not yield another accepted descendant of `H[n]`.

Pure software cannot guarantee this against full cloning. The construction needs nonrollbackable hardware, quorum intersection with honest persistent state, or an external linearizable log [43].

### 9.11 Fork prevention and detectability

**Fork prevention:** no two honest verifiers anchored to `Γ` accept incomparable histories.

**Fork detectability:** once two incomparable histories are co-observed, their authenticators produce transferable evidence:

```
φ=(H[n],τ[n],τ′[n]).
```

Detectability requires eventual comparison and does not undo temporary duplicate authority.

### 9.12 Authorization

Removal, disappearance, jamming, power loss, or partition cannot by itself award a role.

### 9.13 Liveness

Under stated synchrony, a live honest quorum, fair delivery, and an honest or censorship-resistant serializer, every valid request eventually commits. Invalid or unauthorized requests terminate with a definitive abort. A permanently unavailable quorum or serializer falls outside the liveness condition.

Safety and liveness are separate. A safe system may halt during partition.

### 9.14 Optional unlinkability

Given two honest members’ pre-allocation activity and one later terminal role, an adversary cannot determine which earlier transcript belongs to the terminal controller beyond declared leakage.

This requires additional communication and ceremony privacy.

---

## 10. Threat-Model Dimensions

### 10.1 Initializer

A malicious initializer may retain role secrets, bias key generation, watermark shares, install hidden mappings, duplicate members, or retain recovery material.

No-latent-assignment against the initializer requires distributed setup and a precise corruption assumption.

### 10.2 Serializer

A malicious serializer may reorder, censor, duplicate, front-run, disclose, or equivocate allocation requests.

Because assignment is schedule-sensitive, serialization policy is authority policy.

### 10.3 Members

Corrupted members may leak shares, retain obsolete state, clone themselves, withhold messages, send malformed transfer shares, or claim multiple allocations.

### 10.4 Terminal controllers

Issued controllers may leak or misuse their role and may collude with later-corrupted survivors.

### 10.5 Manufacturer and logistics observer

This observer may know serials, packaging, locations, shipment order, extraction order, and telemetry.

EORA does not hide a mapping from an observer who sees the allocation. Its narrower goal is to avoid creating the terminal role mapping earlier than necessary.

### 10.6 Corruption classes

The construction must specify:

- static versus adaptive corruption;
- mobile corruption across epochs;
- Byzantine versus crash-only behavior;
- corruption threshold per epoch;
- secure-erasure assumptions;
- and whether terminal controllers count toward later collusion.

---

## 11. Trust and Impossibility Boundaries

### 11.1 No exit from silence

Asynchronous absence cannot distinguish removal, failure, partition, or delay [39]. Allocation needs positive authorization.

### 11.2 Partition safety versus liveness

Two disconnected cohorts cannot both continue allocating while preserving one global successor unless they share a noncopyable or externally ordered resource.

### 11.3 No software-only clone resistance

A copied snapshot can run twice. Local prevention requires nonrollbackable hardware; otherwise external consensus or later fork evidence is necessary.

### 11.4 Mobile security requires erasure or stronger machinery

Without effective erasure, an adversary can accumulate historical shares across epochs.

### 11.5 Precommitted public keys have a collective controller

If a threshold quorum can reconstruct or sign under `SK[i]`, that quorum operationally controls the role before terminal allocation.

### 11.6 Atomic transfer is not free

Secret delivery and state advancement create a distributed commit problem. A protocol must define crash recovery and duplicate-delivery behavior.

### 11.7 Availability degrades as the cohort shrinks

Thresholds, access structures, and fault tolerance must evolve with `n`. A fixed original threshold may become impossible to satisfy.

### 11.8 The final transition is special

```
C[1] → I[1]+C[0]
```

cannot be authorized by a quorum of survivors because none remain. It requires an external serializer, preauthorized terminal rule, or nonrollbackable state on the final member.

### 11.9 Residual role non-assignment becomes vacuous

At `n=1`, the final eligible member is logically determined even before the final transition. There is no anonymity set and no meaningful permutation of survivors.

---

## 12. Construction A: Late-Key Allocation

Construction A is a baseline protocol under a linearizable serializer, active-set commitment, one-time membership nullifiers, and durable terminal storage.

### 12.1 Setup

1. Initialize members with independent entropy and one-time membership credentials.
2. Commit the active membership set under root `M[N]`.
3. Compute `Γ` using the binding commitment defined in Section 6.1.
4. Initialize canonical state `H[N]` with active-set root `M[N]` and empty spent-nullifier root `Z[N]`.

### 12.2 Allocation

1. Selected member proves active membership, derives one-time membership nullifier `η[d]`, and commits to fresh public-key generation context `κ[d]`.
2. Serializer verifies eligibility, requires `η[d]` to be unused, and authorizes `a[n]` against `H[n]`.
3. Member generates:

```
(sk[n],PK[n])←KeyGen(1^λ).
```

4. Member durably seals `sk[n]`, proves possession of `sk[n]`, and acknowledges `a[n]`, `PK[n]`, `κ[d]`, and `η[d]`.
5. Before promising forced completion, the serializer durably records pending transition `p[n]=(a[n],PK[n],η[d],κ[d],PoP[n],ack[d])`.
6. The serializer removes member `d` from the active-set commitment, adds `η[d]` to the spent-nullifier commitment, computes successor roots `M[n-1]` and `Z[n-1]`, and atomically advances to `H[n-1]`.
7. The serializer emits canonical `τ[n]` from Section 14. Its authorization evidence binds `a[n]`, `commit(d)`, and `η[d]`; its transcript binds `κ[d]`, `PoP[n]`, `ack[d]`, and the `M[n],Z[n]→ M[n-1],Z[n-1]` transition.
8. Member stores `(sk[n],τ[n],Pol[n])`; survivors adopt `H[n-1]`.

A crash before durable pending-state persistence leaves the predecessor reusable and the fresh key unassigned. A crash after persistence recovers deterministically to the same transition; it does not allocate rank `n` to another member.

### 12.3 Optional deterministic derivation

A member may derive:

```
sk[n]=
KDF
(
ρ[d],
H(a[n]),
cid,
n,
"terminal"
).
```

This supports domain separation or backup policy. It does not prove to a verifier that the member followed the KDF unless attestation or a proof is added.

### 12.4 Security scope

Construction A’s key isolation follows from independent key generation. Its monotonicity, rollback resistance, and rank uniqueness come from the serializer. Its one-role-per-member rule comes from atomic active-set removal and nullifier consumption. Its liveness depends on durable terminal storage and the recovery rule.

It does not precommit `PK[n]`.

---

## 13. Construction B: Precommitted-Key Allocation Sketch

Construction B is an architectural sketch, not a complete malicious-secure protocol.

### 13.1 Setup

For each rank `i`:

1. run dealer-based generation or DKG for `SK[i]`;
2. publish `PK[i]`;
3. distribute shares under access structure `A[N]`;
4. commit all public keys, normally through a Merkle root.

A dealer-based setup requires trusted erasure. Avoiding dealer knowledge may require `N` DKG executions or a more efficient derived construction.

### 13.2 Selected-recipient transfer

On authorization of `d` at rank `n`, the cohort executes a verifiable private-transfer functionality:

```
Transfer
(
{share[j] of SK[n]},
d,
a[n]
)
→
(
d:SK[n],
proof
).
```

The transfer should prevent unauthorized reconstruction and detect malformed contributions.

### 13.3 Survivor refresh

Survivors refresh shares for:

```
SK[1],…,SK[n-1]
```

under access structure `A[n-1]`.

Extracted members’ retained old shares must become useless under the stated mobile-adversary bound.

### 13.4 Commit protocol

A practical protocol needs phases such as:

1. `PREPARED`
2. `TRANSFER_READY`
3. `TERMINAL_ACK`
4. `REFRESH_READY`
5. `COMMITTED`

Before usable secret delivery, recovery evidence may force every honest party toward one committed result or a reusable predecessor. After usable `SK[n]` delivery, recovery must force completion of the same recipient and successor; returning to a reusable predecessor is unsafe.

### 13.5 Open construction requirements

A complete construction must specify:

- DKG or dealer trust;
- threshold `t[n]` for every epoch;
- malicious-secure selected-recipient transfer;
- complaint and exclusion handling;
- secure erasure;
- refresh correctness;
- terminal acknowledgment;
- atomic commit;
- crash recovery;
- rollback resistance;
- final-transition authorization;
- and a proof under adaptive or mobile corruption.

Construction B defines the architecture but not a completed protocol.

---

## 14. Transition Certificates and Publication

For a compact concrete encoding, define:

```
roleRoot
=
MerkleRoot(PK[1],…,PK[N])
```

for EORA-P, with a distinguished empty value for EORA-L, and:

```
policyRoot
=
MerkleRoot(Pol[1],…,Pol[N]).
```

These roots instantiate the corresponding lists inside commitment `Γ`. Let:

```
R[N]
:=
Γ
:=
H(
"EORA/v1"
||
cid
||
ctx
||
N
||
roleRoot
||
policyRoot
||
M[N]
||
Ser
).
```

This concrete hash instantiates the abstract binding commitment in Section 6.1. Let `T[n]` be the canonical pre-authentication transcript of authorization, member eligibility, terminal proof of possession, delivery acknowledgment, survivor refresh readiness, and proposed successor state. It explicitly excludes `τ[n]`, `χ[n]`, finality evidence, and every hash that depends on those values. A canonical terminal record is:

```
τ[n]=
(
1,
domain,
R[N],
n,
PK[n],
ν[n],
H[n],
H[n-1],
authorizationEvidence,
H(T[n]),
finalityEvidence,
χ[n]
).
```

Here `χ[n]` is a signature, threshold signature, quorum certificate, or hardware authenticator over every preceding field. The record binds:

- protocol version and domain;
- cohort and policy commitments;
- rank;
- predecessor and successor;
- terminal key;
- authorization evidence;
- transcript hash;
- finality source.

### 14.1 Compact publication

Public-key-addressed systems such as PKARR can publish a compact root, current tip, policy hash, and pointer to complete history [44].

They should not embed the full role set in one mutable record when record size is constrained.

### 14.2 Indexing and equivocation

An indexer can make history queryable without becoming authority.

Two valid certificates from one predecessor create evidence:

```
(H[n],τ[n],τ′[n]).
```

This mirrors signed same-sequence equivocation proofs.

An indexer cannot prove global nonexistence of an unseen branch.

---

## 15. Applications

The application gate requires:

- fixed meaningful role supply;
- event-ordered issuance;
- durable terminal control;
- no prior exclusive allocation;
- local or offline operation;
- nonforking enforcement or credible later evidence.

### 15.1 Isolated spare controllers

Interchangeable controllers in satellites, remote stations, undersea systems, mines, or industrial plants receive ordered failover or update authority only when activated.

This is the strongest fit because a local backplane or secure element may serialize activation offline.

The fatal risk is split-brain or rollback in the same failure domain that disabled the primary controller.

### 15.2 Continuity-of-command kits

Sealed authority devices remain role-neutral until authorized withdrawal assigns incident-command, safety, communications, treasury, or infrastructure roles.

The design reduces pre-event targeting of named backup custodians.

Partitioned caches must stop, share trusted monotonic state, or accept later-detectable forks.

### 15.3 Pubky Ring ranked recovery

Within Pubky, a compatible future design could let a root commit a finite recovery hierarchy while physical Ring devices remain role-neutral. Allocation could issue a root-signed AppCert plus a new ranked-recovery policy layer above AppCert issuance; UKD v0.2 scopes are capability hints rather than a safety mechanism for this policy [45]. PKARR could publish the commitment and tip; homeservers could retain complete records; Nexus could index history. An equivocation format could expose two valid same-authority transitions that bind the same domain, cohort, predecessor, and rank but name different successors [46].

These components do not themselves serialize allocation.

Ordinary social recovery or threshold custody remains simpler unless ranked precommitment and pre-distribution target resistance are explicit requirements.

### 15.4 Sealed chain-of-custody and unblinding kits

Physical inventory may allocate ordered collection, validation, unblinding, release, or disclosure roles during field work, clinical trials, evidence handling, or protected-source workflows.

If privacy or one-time redemption is the primary goal, anonymous credentials are better.

---

## 16. Near-Misses and Dominated Applications

### 16.1 Bitcoin custody and inheritance

Precommitting role keys before selecting physical custodians can reduce mapping leakage.

However:

- threshold custody already supplies the key-control mechanism;
- Bitcoin does not verify an off-chain allocation certificate;
- a quorum may exercise a role before terminal allocation;
- and the Bitkit Core version pinned in [47] constructs single-key descriptors without general multiparty or Taproot script-path policies, while its Trezor signer rejects multisig inputs.

This is a qualified key-ceremony application, not a new Bitcoin primitive.

### 16.2 Federated replacement signers

In deployments that already use federation consensus or a threshold HSM, those mechanisms serialize membership. Any quorum that can transfer a role may also exercise it. The EORA advantage appears only when physical replacement inventory and pre-allocation target resistance are first-order requirements.

Ark operators and DLC oracles do not inherently use federation consensus or threshold custody. EORA applies to them only if a separate physical replacement cohort is intentionally introduced; their ordinary key models do not create this use case.

### 16.3 Ecash issuance

Blind credentials and threshold mint designs already address issuance privacy and distributed custody. EORA adds no clear benefit unless future physical signer roles must be precommitted but remain unassigned.

### 16.4 Paykit terminals

Ordinary root-signed app enrollment and database-backed receiver records dominate normal merchant deployment.

EORA becomes relevant only for fixed offline shipments whose inventory-role mapping is sensitive and whose issuer is unavailable during deployment.

### 16.5 Autonomous agents and pseudonyms

Software is clonable. Bounded agent reproduction requires a database, anonymous rate-limited credential, trusted hardware, or consensus. Once that external state exists, EORA adds little unless physical deployment matters.

### 16.6 Memberships, tickets, gift cards, and allowances

Blind credentials, e-cash, signed counters, and databases provide simpler and stronger constructions.

### 16.7 Moderator accounts and sequential names

Fresh delegated keys plus signed appointment records are sufficient. Ordinal certificates do not require threshold role inventory.

### 16.8 Lightning

Channel participants and keys are fixed during establishment. EORA is not a drop-in Lightning role mechanism.

---

## 17. Open Problems

1. Can no-latent-assignment be defined as a composable simulation property?
2. Does survivor exchangeability require a new lower bound or follow from standard permutation invariance?
3. Can EORA-P avoid `N` independent DKGs or quadratic role-share state?
4. Can selected-recipient transfer remain private from survivors under practical assumptions?
5. Can the transfer, refresh, and commit phases achieve recoverable atomicity efficiently?
6. Can proactive security survive realistic erasure failures?
7. How should thresholds evolve as the cohort shrinks?
8. What is the minimal noncopyable or externally ordered resource needed for offline fork prevention?
9. Can zero-knowledge proofs hide rank, cohort cardinality, or recipient while preserving monotonic consumption?
10. Can a final-member transition remain safe without an external serializer?
11. Is there a compelling application where every requirement is necessary and a standard threshold HSM plus signed counter fails?
12. Does existing provisioning literature name the complete functionality under another term?

---

## 18. Conclusion

The useful inversion is:

```
commit role
→
authorized event
→
commit terminal controller.
```

The strongest practical benefit is resistance to premature role targeting.

The strongest conceptual property is residual role non-assignment.

The strongest novelty risk is straightforward composition:

```
threshold custody
+
selected-recipient transfer
+
monotonic allocator
+
finality mechanism.
```

Exit-ordered role allocation is a candidate ideal functionality and security profile for late-bound ordered authority.

Its candidate distinguishing invariant is:

> Before the authorized event that allocates rank `i`, no exclusive protocol fact selects which survivor controls role `I[i]`. Processing that event creates the fact, consumes one member and role, and leaves every other survivor role-neutral while at least two eligible survivors remain.

Whether this invariant supports a new cryptographic primitive remains open.

The systems pattern is useful even if it does not.

---

## References

1. Azam Soleimanian, “Anonymous Random Allocation and Its Applications; Generalizing Single Secret Leader Election,” Cryptology ePrint Archive, Paper 2022/1219, 2022, revised 2025. https://eprint.iacr.org/2022/1219
2. Chris Studholme and Ian Blake, “Multiparty Computation to Generate Secret Permutations,” Cryptology ePrint Archive, Paper 2007/353, 2007. https://eprint.iacr.org/2007/353
3. Craig Gentry, Shai Halevi, Hugo Krawczyk, Bernardo Magri, Jesper Buus Nielsen, Tal Rabin, and Sophia Yakoubov, “You Only Speak Once: Secure MPC with Stateless Ephemeral Roles,” CRYPTO 2021. https://doi.org/10.1007/978-3-030-84245-1_3
4. Hamidreza Khoshakhlagh, Matteo Campanelli, Bernardo David, Anders Konring, and Jesper Buus Nielsen, “Encryption to the Future: A Paradigm for Sending Secret Messages to Future Anonymous Committees,” ASIACRYPT 2022. https://eprint.iacr.org/2021/1423
5. Fabrice Benhamouda, Craig Gentry, Sergey Gorbunov, Shai Halevi, Hugo Krawczyk, Chengyu Lin, Tal Rabin, and Leonid Reyzin, “Can a Public Blockchain Keep a Secret?” TCC 2020. https://eprint.iacr.org/2020/464
6. Ignacio Cascudo, Bernardo David, Lydia Garms, and Anders Konring, “YOLO YOSO: Fast and Simple Encryption and Secret Sharing in the YOSO Model,” ASIACRYPT 2022. https://eprint.iacr.org/2022/242
7. Ran Canetti, Sebastian Kolby, Divya Ravi, Eduardo Soria-Vazquez, and Sophia Yakoubov, “Taming Adaptivity in YOSO Protocols: The Modular Way,” TCC 2023. https://eprint.iacr.org/2023/1077
8. Sai Krishna Deepak Maram, Fan Zhang, Lun Wang, Andrew Low, Yupeng Zhang, Ari Juels, and Dawn Song, “CHURP: Dynamic-Committee Proactive Secret Sharing,” ACM CCS 2019. https://doi.org/10.1145/3319535.3363203
9. Yunzhou Yan, Yu Xia, and Srinivas Devadas, “Shanrang: Fully Asynchronous Proactive Secret Sharing with Dynamic Committees,” Cryptology ePrint Archive, Paper 2022/164. https://eprint.iacr.org/2022/164
10. Bin Hu, Zongyang Zhang, Han Chen, You Zhou, Huazu Jiang, and Jianwei Liu, “DyCAPS: Asynchronous Dynamic-committee Proactive Secret Sharing,” Cryptology ePrint Archive, Paper 2022/1169. https://eprint.iacr.org/2022/1169
11. Bin Hu, Jianwei Liu, Zhenliang Lu, Qiang Tang, Zhuolun Xiang, and Zongyang Zhang, “Optimistic Asynchronous Dynamic-committee Proactive Secret Sharing,” Cryptology ePrint Archive, Paper 2025/880. https://eprint.iacr.org/2025/880
12. Theodore M. Wong, Chenxi Wang, and Jeannette M. Wing, “Verifiable Secret Redistribution,” CMU-CS-01-155. https://doi.org/10.1184/r1/6619883
13. Jens Groth, “Non-interactive Distributed Key Generation and Key Resharing,” Cryptology ePrint Archive, Paper 2021/339. https://eprint.iacr.org/2021/339
14. Fabrice Benhamouda, Mariana Raykova, and Karn Seth, “Anonymous Counting Tokens,” ASIACRYPT 2023. https://doi.org/10.1007/978-981-99-8724-5_8
15. Jan Camenisch and Anna Lysyanskaya, “An Efficient System for Non-transferable Anonymous Credentials with Optional Anonymity Revocation,” EUROCRYPT 2001. https://eprint.iacr.org/2001/019
16. Jan Camenisch, Susan Hohenberger, and Anna Lysyanskaya, “Compact E-Cash,” EUROCRYPT 2005. https://eprint.iacr.org/2005/060
17. Jan Camenisch, Susan Hohenberger, Markulf Kohlweiss, Anna Lysyanskaya, and Mira Meyerovich, “How to Win the Clone Wars: Efficient Periodic n-Times Anonymous Authentication,” ACM CCS 2006. https://eprint.iacr.org/2006/454
18. Rutchathon Chairattana-Apirom, Nico Döttling, Anna Lysyanskaya, and Stefano Tessaro, “Everlasting Anonymous Rate-Limited Tokens,” ASIACRYPT 2025. https://eprint.iacr.org/2025/1030
19. Moxie Marlinspike and Trevor Perrin, “The X3DH Key Agreement Protocol,” Signal specification, 2016. https://signal.org/docs/specifications/x3dh/
20. Richard Barnes, Benjamin Beurdouche, Raphael Robert, Jon Millican, Emad Omara, and Katriel Cohn-Gordon, “The Messaging Layer Security Protocol,” RFC 9420, 2023. https://doi.org/10.17487/RFC9420
21. Benjamin Beurdouche, Eric Rescorla, Emad Omara, Srinivas Inguva, and Alan Duric, “The Messaging Layer Security Architecture,” RFC 9750, 2025. https://doi.org/10.17487/RFC9750
22. Andreas Hülsing, Denis Butin, Stefan-Lukas Gazdag, Joost Rijneveld, and Aziz Mohaisen, “XMSS: eXtended Merkle Signature Scheme,” RFC 8391, 2018. https://doi.org/10.17487/RFC8391
23. Kevin Fu, Seny Kamara, and Tadayoshi Kohno, “Key Regression: Enabling Efficient Key Distribution for Secure Distributed Storage,” NDSS 2006. https://eprint.iacr.org/2005/303
24. Mihir Bellare and Sara Miner, “A Forward-Secure Digital Signature Scheme,” CRYPTO 1999. https://doi.org/10.1007/3-540-48405-1_28
25. Ben Laurie, Eran Messeri, and Rob Stradling, “Certificate Transparency Version 2.0,” RFC 9162, 2021. https://doi.org/10.17487/RFC9162
26. Marcela S. Melara, Aaron Blankstein, Joseph Bonneau, Edward W. Felten, and Michael J. Freedman, “CONIKS: Bringing Key Transparency to End Users,” USENIX Security 2015. https://www.usenix.org/conference/usenixsecurity15/technical-sessions/presentation/melara
27. Joakim Brorsson, Elena Pagnin, Bernardo David, and Paul Stankovski Wagner, “Consistency-or-Die: Consistency for Key Transparency,” Cryptology ePrint Archive, Paper 2024/879. https://eprint.iacr.org/2024/879
28. IBM, “Pre-generation of Generic Session Keys for Use in Communicating Within Communications Environments,” US7885412B2, priority 2005. https://patents.google.com/patent/US7885412B2/en
29. “Secure Device-Key Provisioning,” US11063753B2, priority 2018. https://patents.google.com/patent/US11063753B2/en
30. “Secure Device-Key Provisioning,” US11677548B2, continuation. https://patents.google.com/patent/US11677548B2/en
31. “Just-in-time Gift-card Activation and Assignment,” US8200544B1, priority 2008. https://patents.google.com/patent/US8200544B1/en
32. “Assigning Roles to Devices Using Physical Tokens,” US7502793B2, priority 2004. https://patents.google.com/patent/US7502793B2/en
33. “Subscriber Identity Personalization from Unoccupied IMSI Inventory,” WO2010015883A1, priority 2008. https://patents.google.com/patent/WO2010015883A1/en
34. “Secure-element Personalization,” US10057759B2, priority 2011. https://patents.google.com/patent/US10057759B2/en
35. “Allocated and Unallocated eSIM Profile Pools,” US11825549B2, priority 2021. https://patents.google.com/patent/US11825549B2/en
36. “FIDO Device Onboard Commissioning,” US11144911B2, priority 2016. https://patents.google.com/patent/US11144911B2/en
37. “Threshold Hardware-wallet Custody,” US11164182B2, priority 2018. https://patents.google.com/patent/US11164182B2/en
38. Hagit Attiya, Amotz Bar-Noy, Danny Dolev, David Peleg, and Rüdiger Reischuk, “Renaming in an Asynchronous Environment,” Journal of the ACM 37(3), 1990. https://doi.org/10.1145/79147.79158
39. Tushar Deepak Chandra and Sam Toueg, “Unreliable Failure Detectors for Reliable Distributed Systems,” Journal of the ACM 43(2), 1996. https://doi.org/10.1145/226643.226647
40. Sudhir R. Ahuja and Dhiraj K. Sharma, “Distributed First-Come First-Served Bus Allocation Apparatus,” US4281381A, priority 14 May 1979. https://patents.google.com/patent/US4281381A/en
41. Jim Gray and Leslie Lamport, “Consensus on Transaction Commit,” ACM Transactions on Database Systems 31(1), 2006. https://doi.org/10.1145/1132863.1132865
42. N. Asokan, Victor Shoup, and Michael Waidner, “Optimistic Fair Exchange of Digital Signatures,” EUROCRYPT 1998. https://doi.org/10.1007/BFb0054136
43. Bryan Parno, Jonathan M. McCune, and Adrian Perrig, “Memoir: Practical State Continuity for Protected Modules,” IEEE Symposium on Security and Privacy, 2011. https://doi.org/10.1109/SP.2011.38
44. PKARR, `SignedPacket` implementation, commit `c63a5098f9fab64084bc373104eafb8282882cbd`. https://github.com/BitcoinErrorLog/pkarr/blob/c63a5098f9fab64084bc373104eafb8282882cbd/pkarr/src/signed_packet.rs
45. Pubky Core, “Unified Key Delegation Specification v0.2,” commit `cd8e3c03b718dff266d00c2e491a2502e4a76d22`, dated 19 January 2026. https://github.com/BitcoinErrorLog/pubky-core/blob/cd8e3c03b718dff266d00c2e491a2502e4a76d22/docs/PUBKY_UNIFIED_KEY_DELEGATION_SPEC_v0.2.md
46. Atomicity Core, “V2 Claim and Equivocation Verification,” commit `4e81bce6fac32721516f662a46f2012b9c1becf2`. https://github.com/atomicity-credit/atomicity-core/blob/4e81bce6fac32721516f662a46f2012b9c1becf2/src/v2/claim.rs
47. Bitkit Core, “On-chain Descriptor Construction and Trezor Signing,” commit `7e9849873a4f2600ef1c7fcfa0953c914d54f0f5`. https://github.com/synonymdev/bitkit-core/blob/7e9849873a4f2600ef1c7fcfa0953c914d54f0f5/src/modules/onchain/implementation.rs; https://github.com/synonymdev/bitkit-core/blob/7e9849873a4f2600ef1c7fcfa0953c914d54f0f5/src/modules/trezor/implementation.rs
