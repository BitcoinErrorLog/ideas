# Exit-Ordered Role Allocation for Recovery and Continuity

Recovery systems usually assign each recovery role to a specific person or device during setup. A company names a backup administrator, a family labels an inheritance key, or a private group designates an emergency owner. This is easy to understand and administer, but it also creates a durable mapping between a recovery object and the authority it will eventually exercise:

`device 8142 → highest recovery authority`

That mapping can leak through inventory systems, setup logs, shipping records, photographs, device attestations, or the people who handled the ceremony. If an attacker can identify the future controller, targeting that person or device may be easier than breaking the cryptography.

A controller-neutral continuity kit separates two decisions that recovery systems normally combine. It defines the number, order, and powers of the recovery roles in advance, but delays assigning those roles to individual controllers until an authorized ceremony. The operating principle is simple:

> **Commit the recovery roles before committing those roles to individual controllers.**

## Start with the recovery policy

Consider a continuity policy with five ordered roles:

`R₅, R₄, R₃, R₂, R₁`

The roles might cover immediate device revocation, emergency account access, service or homeserver migration, deeper identity recovery, and final succession. Their powers and order are fixed in advance, so relying systems can evaluate the recovery policy before an emergency occurs.

Five eligible recovery devices begin as a role-neutral cohort:

`C₅`

Role-neutrality does not mean that the devices are anonymous or identical. They may have serial numbers, independent entropy, and distinct internal shares. It means only that no device has an exclusive assignment to one of the future recovery roles.

When an authorized ceremony selects a device, that device receives R₅, while the other four devices remain unassigned with respect to the lower roles:

`C₅ → R₅ + C₄`

The next accepted allocation assigns R₄:

`C₄ → R₄ + C₃`

The allocation event therefore creates the controller-role mapping instead of revealing a label established during setup.

## Late-generated and precommitted keys

There are two practical versions of the design. In the simpler version, the selected device generates a fresh terminal key when it receives a role. The continuity policy then issues a certificate binding that public key to the role, cohort, and accepted allocation history. This works when relying systems can learn the recovery public key at allocation time.

The precommitted-key version is needed when relying systems must accept the recovery policy before the original authority becomes unavailable. In that version, the recovery public keys exist in advance, while their private material remains under collective custody across the cohort. No individual device initially controls a complete role key. An authorized allocation attempts to establish exclusive terminal control of one role key, after which the survivors refresh their shares for the roles that remain.

The keys already exist under collective control; the individual terminal controllers do not. This distinction is important because the cohort remains a security target. The design removes a specific device-to-role mapping, not the need to protect the underlying recovery system.

## The security improvement

The strict benefit is resistance to premature selective targeting. An attacker may know the identities of all five trustees and the serial numbers of all five devices, but cannot steal the highest recovery role from one designated trustee if no trustee has received that role.

This is useful only under several assumptions. Allocation must not be predictable or manipulable, manufacturing and logistics metadata must not recreate the future mapping, and compromising the whole cohort must remain harder than targeting one preselected controller. When those conditions hold, delaying assignment removes information that an attacker could otherwise exploit.

The design does not provide anonymity by itself. Once a role is allocated, the controller may become fully visible. The benefit lies in avoiding an individual assignment before that assignment is operationally necessary.

## Pubky identity continuity

A Pubky identity may need to survive the loss of a phone, app, homeserver, or administrator. A future continuity policy could allow the Pubky root to commit a recovery hierarchy and a root of the recovery public keys, after which the root could return to cold storage.

Ring devices would hold the recovery material collectively. When the policy authorizes continuity, one selected device would receive the next recovery role and the remaining devices would refresh their state. Ring could then issue an AppCert for a newly generated delegated-key bundle, while a separate recovery-policy layer records and enforces the cohort, rank, and permitted recovery action.

PKARR could publish the original commitment and latest accepted state, while a homeserver retains complete transition certificates. This would make the policy discoverable without making PKARR the authority that chooses a canonical transition. A signed mutable DHT record can publish state, but it cannot prove that no conflicting branch exists elsewhere.

The continuity layer therefore needs an explicit serializer: a physical device, threshold quorum, consensus process, or other mechanism capable of accepting one successor from one predecessor.

## Private-group succession

The same mechanism could preserve a private group when its original administrators become unavailable. Conventional designs either name backup administrators in advance, trust a server to appoint replacements, or allow the group to die with its owners. A succession policy can instead precommit emergency-administration roles while leaving their individual controllers unassigned.

When continuity is triggered, an authorized cohort transition assigns one recovery role to a selected Ring device. Under a future group-policy extension, that controller might revoke lost administrative devices, rotate compromised administration, restore access to protected group state, or appoint a new ordinary administrator.

This mechanism would not replace private-message encryption or MLS. It would sit above them as a governance and continuity layer. Its additional complexity is justified only when the group must precommit recovery authority, the original administrators may be unavailable, and naming backup administrators in advance creates a meaningful risk.

## Continuity of command

Organizations operating through disasters or infrastructure failures have a similar problem. A continuity cohort could commit a bounded hierarchy of emergency roles for incident command, safety, communications, treasury, and infrastructure recovery without publishing a permanent list of named successors.

An accepted activation would assign one role at a time. This preserves a known authority structure while delaying the identity of each individual controller. The tradeoff is that the activation mechanism must remain safe under the same conditions that made continuity necessary.

Two disconnected caches cannot both continue allocating roles while guaranteeing one global history unless they share a noncopyable or externally ordered resource. During a partition, the system must stop, rely on trusted monotonic hardware, or accept that conflicting allocations may only be detected after reconnection.

## Authorization and serialization

A device disappearing from the cohort is not enough to create authority. Battery failure, radio interference, and network partition are indistinguishable from removal in many distributed environments. A valid allocation therefore requires positive authorization and a serialization rule:

`one accepted predecessor ⇒ one accepted successor`

Possible serializers include a secure tray that releases one device at a time, a threshold of surviving trustees, protected monotonic hardware, or a consensus process. A transparency system can expose conflicting transitions after they become visible, but fork detection is weaker than fork prevention.

The serializer determines both the safety and availability of the continuity kit. A physical tray may work well for a local custody ceremony but poorly for geographically distributed trustees. A quorum can distribute trust but may become unavailable during a disaster. Consensus can provide a canonical order but introduces network and membership assumptions.

## Transfer and state advancement

The precommitted-key version must coordinate delivery of the recovery role, advancement of cohort state, refresh of survivor shares, and publication of the transition certificate. These steps cannot be treated independently.

If the selected controller receives the role key but the cohort fails to advance, returning to the old state could allocate the same role twice. If the cohort advances but the selected controller loses the key, the role may be permanently burned. The protocol therefore needs durable pending-transition state and a deterministic recovery rule.

Before usable key delivery, an interrupted transition may safely abort. After usable key delivery, recovery must complete the same assignment because the selected controller cannot be forced to forget the secret.

## When the complexity is justified

A controller-neutral continuity kit is unnecessary when named trustees are acceptable, the original root remains available to issue fresh delegated keys, or one trusted database can serialize recovery. Anonymous credentials are better when unlinkable membership is the main objective, and conventional group administration is better when successor targeting is not a credible threat.

The additional machinery is justified only when all of the following are true:

- recovery authority must be committed in advance;
- future controllers must remain individually unassigned;
- the original authority may be unavailable at allocation time;
- allocation order has policy meaning;
- and duplicate transitions can be prevented or exposed.

A normal recovery kit says, “this object is the successor.” A controller-neutral kit says, “this cohort can produce a bounded sequence of successors, but no individual member controls the next role yet.”

The distinction matters when delaying the controller-role mapping removes information that an attacker could otherwise use.
