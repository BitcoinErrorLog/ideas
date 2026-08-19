# The Keys That Do Not Exist Yet

Imagine ten identical pens in the middle of a table.

The first pen removed becomes Pen 10.

The second becomes Pen 9.

The eight still on the table are not secretly Pens 1 through 8. There are no labels underneath them. They remain interchangeable with respect to every identity they may still receive.

Eventually, removal order becomes identity:

`Cₙ → Iₙ + Cₙ₋₁`

That sounds like a parlor trick.

It may also be a useful way to allocate cryptographic authority.

The precise claim is narrow.

The interesting property is not anonymity in general. It is not digital scarcity by magic. And under the precommitted-key profile, the keys may already exist.

The useful idea is:

> **A system can commit to a bounded sequence of roles before it commits those roles to individual terminal controllers.**

An authorized event assigns one role.

Every survivor remains without exclusive assignment to any remaining terminal role.

That is **bounded role supply without prior exclusive allocation**.

## Two different things can mean “the key does not exist”

The idea has two versions. They should not be conflated.

### A key generated at allocation

The simple version really does involve a key that does not exist yet.

A group contains ten eligible devices.

The first authorized allocation selects one device. That device generates a fresh key and receives a certificate saying:

> This public key controls role 10 under cohort X.

At the next authorized allocation, the selected device generates a fresh terminal key and receives role 9.

Before each allocation, there is no terminal key for that role.

This is not exotic cryptography. It is fresh key generation plus a canonical, serialized allocation history. A merely signed history can expose conflicting branches, but signatures alone do not prevent them.

It is still useful when:

- the number of roles must be fixed;
- the role order matters;
- assignment must follow a physical or operational event;
- and unused devices should remain role-neutral.

Its limitation is obvious:

> A public key that does not exist cannot already appear in an external policy.

That rules it out when Bitcoin, a certificate tree, or another system must commit to the terminal public keys before allocation.

### A key that exists without an exclusive terminal controller

The precommitted-key profile creates role keys in advance:

`x₁, …, xₙ`

with public keys:

`Xᵢ = xᵢG`

The public keys can already appear in a Bitcoin policy, recovery policy, or other commitment.

But no single device initially controls any xᵢ.

Instead, each role secret is held collectively, perhaps through threshold shares distributed across the cohort. An authorized allocation attempts to establish exclusive terminal control of xₙ under explicit threshold, transfer, refresh, and erasure assumptions. The survivors then refresh their shares for the remaining roles.

Under this profile, the keys exist.

The cohort collectively controls them.

What does not exist is the exclusive mapping:

`device d ↦ role Iₙ`

The accurate description is:

> **Precommitted role keys under collective custody, transferred to later-selected terminal controllers.**

That is less mysterious than “authority without a holder,” but it is also more useful because it states the actual control boundary.

## The real security benefit is resistance to premature targeting

Suppose five hardware devices will eventually control five unequal recovery roles.

If they are labeled during manufacture, packaging, or setup, a mapping can leak:

`serial 8142 → highest recovery authority`

That fact may appear in:

- manufacturing systems;
- setup logs;
- inventory databases;
- shipping records;
- photographs;
- device attestations;
- or someone’s memory.

An attacker can target the most valuable object before distribution is complete.

Now change the ceremony.

The five roles are committed in advance, but all five devices remain eligible for every remaining role. The first authorized allocation gives one selected device the highest role. The others remain role-neutral. The next allocation assigns the next role.

A warehouse attacker may still identify every chassis.

What they cannot learn from a role mapping is which chassis will become the most valuable signer, because the protocol has not assigned that fact yet.

This does not eliminate supply-chain risk.

Under assumptions that allocation is not predictable or manipulable and metadata does not recreate the mapping, it removes one pre-allocation mapping leak from the supply chain.

## This is not residual anonymity

The unused devices may be completely identifiable.

They can have:

- serial numbers;
- unique entropy;
- attestation keys;
- network addresses;
- hardware fingerprints;
- and different internal threshold shares.

The desired property is not that nobody knows which physical device is which.

It is:

> **No surviving device has been exclusively assigned to any remaining role.**

Call that **residual role non-assignment**.

A candidate stronger property is **role exchangeability**: protocol-view distributions remain indistinguishable under permutations of honest survivors until a later allocation selects one. That requires a precise permutation-invariance definition and proof.

Actual anonymity and unlinkability require additional mechanisms. A public allocation ceremony may reveal exactly who received each role.

## Where the idea appears strongest

The idea only deserves its complexity when all of the following matter together:

- fixed and meaningful role supply;
- event-ordered issuance;
- durable terminal key control;
- no prior exclusive assignment;
- local or offline operation;
- and enforced nonforking or later-verifiable fork evidence.

Most applications fail this test. Four families survive it better than the rest.

## Isolated spare controllers

Imagine a satellite, mine, undersea system, remote station, or industrial plant with several dormant controllers.

The spares need to be installed before anyone knows which one will become the next privileged controller.

Activation order may determine:

- failover epoch;
- command authority;
- software-update authority;
- recovery priority;
- or access to protected state.

This environment has something ordinary software systems often lack: a local physical serializer.

A backplane, secure enclosure, or monotonic secure element may be able to authorize one activation at a time even without network connectivity.

The benefit is real if prelabeling “the final recovery controller” would make one spare more targetable.

The catch is equally real:

> The serialization mechanism must survive the same failure that disabled the primary system.

If controller state can be cloned or rolled back, the system may only detect forks later.

## Continuity-of-command kits

Sealed, interchangeable authority devices could be positioned before a disaster without naming future custodians.

Authorized withdrawal order might assign:

- incident command;
- safety authority;
- communications authority;
- treasury authority;
- or critical-infrastructure access.

This use case genuinely values ranked authority, physical distribution, offline operation, and resistance to pre-event targeting.

It also exposes the hardest tradeoff.

Two disconnected caches cannot both continue allocating and still guarantee one global history unless they share a noncopyable or externally ordered resource.

During a partition, the system must choose:

- preserve safety and stop;
- continue locally and risk a fork;
- or rely on trusted monotonic hardware.

There is no notation that makes this tradeoff disappear.

## Pubky Ring recovery

Pubky makes the idea interesting because keys can identify sovereign identities and authorize delegated applications, recovery actions, or payment-receiver records.

Ring could eventually create a ranked recovery cohort:

`R₅, R₄, R₃, R₂, R₁`

The physical devices remain role-neutral until a deliberate recovery ceremony assigns one. After allocation, Ring could issue an AppCert for a newly generated delegated-key bundle, while a separate future policy layer records and enforces the recovery cohort and rank. UKD v0.2 scopes are capability hints, not an authorization system for this recovery policy.

PKARR could publish:

- the cohort commitment;
- the current accepted tip;
- a policy hash;
- and a pointer to complete transition certificates.

Nexus could index the history.

Atomicity’s equivocation pattern suggests how two valid signatures from the same authority could evidence a conflict when both bind the same domain, cohort, predecessor, and rank but name different successors.

But PKARR is not consensus.

A mutable signed DHT record can publish state. It cannot prove that no conflicting transition exists elsewhere. Cloned Ring or backup state can still allocate the same rank twice unless another mechanism prevents rollback.

The best Pubky use case is therefore not “anonymous identities from nowhere.”

It is:

> **A bounded recovery-policy extension whose physical controller is selected only during the recovery ceremony.**

## Sealed chain-of-custody and unblinding kits

Role-neutral physical inventory could assign ordered collection, validation, unblinding, release, or disclosure authority during field work, clinical trials, evidence handling, or protected-source workflows.

This only qualifies when the order changes durable authority and identifying the future controller would create a target.

If privacy or one-time redemption is the actual requirement, anonymous credentials and conventional audit logs are simpler and stronger.

## Bitcoin custody is a useful near-miss

Bitcoin makes the target-resistance argument easy to understand.

A custody policy can commit several public keys before deciding which physical device or custodian will receive each role.

That can protect the device-role mapping during manufacturing and logistics.

But the threshold cohort already controls the private material.

And Bitcoin does not know whether an off-chain allocation certificate exists.

If a quorum can produce a valid signature under Xᵢ, Bitcoin accepts it unless the spending policy itself enforces equivalent allocation state.

This means the construction is a key ceremony, not a new Bitcoin primitive.

It may still be worth doing. It is simply not stronger than its trust model.

It is also not a current Bitkit feature. Bitkit Core presently builds single-xpub descriptors, its Taproot path is key-path oriented, and its hardware integration does not provide the required general multisignature and script-tree workflow.

## Where simpler mechanisms win

The decremental pattern is not automatically useful whenever there is a finite number of things.

Use a linearizable allocator, trusted database transaction, or rollback-resistant counter when all you need is:

> At most ten accounts may be created.

Use fresh delegated keys when all you need is:

> Create a moderator or application identity when appointed.

Use anonymous credentials or e-cash when all you need is:

> Issue a finite number of unlinkable tokens.

Use a database when all you need is:

> Enroll merchant terminals and prevent duplicate records.

Use threshold custody when all you need is:

> Prevent one party from controlling a recovery key.

Use Bitcoin descriptors and timelocks when all you need is:

> Commit alternative spending paths.

Generic moderator accounts, founder memberships, gift cards, allowances, ordinary Paykit terminals, software licenses, AI-agent quotas, and Lightning participants do not justify this machinery by default.

If removing the “no prior exclusive allocation” requirement leaves the application unchanged, then this idea is probably not the right tool.

## The hard part is serialization

An exit cannot mean:

> I stopped hearing the other devices.

A jammer, dead battery, or network partition could then create authority.

An allocation needs an accepted transition:

`one predecessor state ⇒ one accepted successor state`

That may come from:

- a physical docking bus;
- a secure enclosure;
- a surviving quorum;
- Byzantine consensus;
- an append-only sequencer;
- trusted monotonic hardware;
- or a transparency system that exposes equivocation later.

These choices provide different guarantees.

Fork prevention is stronger than fork detection.

Offline liveness and global serialization conflict during partitions unless a shared noncopyable or externally ordered resource exists.

And “exactly once” should be split into:

- at-most-once safety;
- and liveness or recoverability after crashes.

## The transition is a transaction

The precommitted-key version must coordinate at least three events:

1. transfer terminal control of xₙ;
2. advance the cohort to Cₙ₋₁;
3. refresh or invalidate survivor state.

What happens if the recipient gets the key but the cohort does not commit?

What happens if the cohort advances but the recipient loses the key?

What happens if an extracted device keeps old shares?

What happens when the cohort shrinks below its original threshold?

What authorizes:

`C₁ → I₁ + C₀`

when no survivor remains?

These are not implementation details.

They are the protocol.

## This idea has close relatives

Every component has strong prior art.

Anonymous Random Allocation creates hidden one-to-one assignments.

YOSO and Encryption to the Future assign abstract roles to later or anonymous participants.

Dynamic proactive secret sharing transfers and refreshes secrets across changing committees.

Anonymous counting tokens, e-cash, Signal prekeys, MLS KeyPackages, and XMSS all provide different forms of bounded or indexed one-time consumption.

Transparency logs and consensus systems provide ordering and fork resistance.

Provisioning systems already use generic key pools, late device personalization, physical role tokens, and “next available” credentials.

Absent a formally distinct property and proof, the combination is a security objective and state-machine pattern:

`bounded roles + event-ordered allocation + survivor role non-assignment`

Primitive status would require a formal property that straightforward composition cannot already realize.

## The principle worth keeping

The intuition is:

> Instead of giving an object an identity and recording what happens to it, let what happens to the object determine the identity it receives.

That still works as an intuition.

The technical version is more precise:

> **Commit the role before committing the controller. Let an authorized event create the exclusive mapping, and leave every unused member eligible for every remaining role.**

That is not universal.

It is not simple.

It does not remove the need for trust.

The benefit exists only when delaying the controller-role mapping removes information an attacker could otherwise exploit.
