---
title: "The Vibeware Playbook"
subtitle: "An implementation spec for attaching use → qualify → generate → expose → select → use to an existing application."
author: "John Carvalho"
date: "2026-08-30"
---

# The Vibeware Playbook

*An implementation spec for attaching `use → qualify → generate → expose → select → use` to an existing application.*

This file is the object you implement against. It names the records, the schemas, the predicates, the allowed transitions, and the halt conditions. If a field is not in a schema, it is not in the loop. If a predicate fails, the event is dropped. If a halt fires, write access and every experimental flag go dark.

Users are a sensor, not an oracle. The assurance burden follows the consequence of failure, not whether a human or an agent wrote the code. Taste may reject a signal that is real, repeated, and still wrong. The agent cannot mutate the telemetry contract, cannot approve its own work, and cannot expand its own capability.

The loop will not start until every object in the wiring checklist exists.

```
Application → EvidenceEnvelope → Qualifier → Opportunity → CapabilityGatedAgent → Candidate → Exposure → OutcomeEvents → Selector → Application
HaltController can disable Qualifier write-out, Agent write tools, and Exposure flags.
```

---

## 1. ConsequenceClass

A surface without a class does not attach to the loop.

```ts
type ConsequenceClass = "T0" | "T1" | "T2" | "T3";
```

| Class | Surface | Allowed | Forbidden |
|---|---|---|---|
| `T0` | Read-only. Public or synthetic data. No auth. | Read evidence. Observe. Propose. Generate in an experiment repo. | Writes to shared user state. Auth-gated mutation. |
| `T1` | Isolated, reversible interaction. Layout, copy, ranking of public content, experimental navigation. Ordinary failure is information. | Everything in T0, plus flag-gated expose after `generate` has a boring record. | Durable shared writes. Keys, money, protocol. |
| `T2` | Identity, private data, public writes, recommendations, shared-state mutation, durable signed records. Rollback of a deploy does not unsay a write. | Observe and propose only until a named experimental namespace and a deletion or forward-correction plan exist. Generation only inside that namespace or an opted-in cohort. | Promoting a write that other users will keep after archive. Any contract field the wearer cannot inspect or leave. |
| `T3` | Signing, custody, value movement, key handling, protocol compatibility, secrets, privileged infrastructure. | Call a hardened adapter that already exists. Analyze, test, review. | Generate or regenerate the adapter, the protocol, the signer, or the custody path. |

Invariant: a candidate's class is `max(surface.class, filesTouched.class)`. If any touched path is T3, the candidate is T3 and generation of that path is rejected.

```json
{
  "$id": "https://vibeware.spec/consequence-class.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ConsequenceClass",
  "type": "string",
  "enum": ["T0", "T1", "T2", "T3"]
}
```

---

## 2. TelemetryContract

The contract is a human-authored, versioned object. The agent may read it. The agent may not write it. An empty required field means the loop does not start.

```ts
type FieldType = "count" | "error_class" | "path" | "task_result" | "abandonment" | "explicit_preference" | "diagnostic";

interface ContractField {
  name: string;
  type: FieldType;
  purpose: string;
  model_exposure: boolean;
}

interface TelemetryContract {
  version: number;
  purpose: string;
  surface: string;
  tier: ConsequenceClass;
  cohort: "owner" | "opt_in" | "existing_users" | "named_test_keys";
  fields: ContractField[];
  forbidden_fields: string[];
  retention_days: number;
  deletion: string;
  access: string[];
  halt_actor: string[];
  user_inspect: string;
  user_exit: string;
  model_host?: string;
  model_region?: string;
}
```

Required defaults and invariants:

- `purpose` is one sentence naming the update the user should get. "The agent might find it useful" is not a purpose.
- `retention_days` defaults to `14` if omitted. Values above `90` are rejected at contract validation.
- `forbidden_fields` always includes at least: `raw_post_body`, `private_message`, `durable_user_id`, `session_replay`, `payment_detail`, `seed_material`, `key_material`.
- Every `fields[i].name` used by an `EvidenceEnvelope` must appear here. Unknown field names are a contract breach and halt the loop.
- `model_exposure: true` requires `model_host` and `model_region`. Identifiers are stripped before any model call.
- Changing the contract increments `version` and is a human write. The agent has no tool that can PATCH this object.

```json
{
  "$id": "https://vibeware.spec/telemetry-contract.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TelemetryContract",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "version", "purpose", "surface", "tier", "cohort", "fields",
    "forbidden_fields", "retention_days", "deletion", "access",
    "halt_actor", "user_inspect", "user_exit"
  ],
  "properties": {
    "version": { "type": "integer", "minimum": 1 },
    "purpose": { "type": "string", "minLength": 8 },
    "surface": { "type": "string", "minLength": 1 },
    "tier": { "$ref": "https://vibeware.spec/consequence-class.json" },
    "cohort": { "type": "string", "enum": ["owner", "opt_in", "existing_users", "named_test_keys"] },
    "fields": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": ["name", "type", "purpose", "model_exposure"],
        "properties": {
          "name": { "type": "string", "pattern": "^[a-z][a-z0-9_]*$" },
          "type": {
            "type": "string",
            "enum": ["count", "error_class", "path", "task_result", "abandonment", "explicit_preference", "diagnostic"]
          },
          "purpose": { "type": "string", "minLength": 8 },
          "model_exposure": { "type": "boolean" }
        }
      }
    },
    "forbidden_fields": {
      "type": "array",
      "minItems": 6,
      "items": { "type": "string" }
    },
    "retention_days": { "type": "integer", "minimum": 1, "maximum": 90 },
    "deletion": { "type": "string", "minLength": 8 },
    "access": { "type": "array", "minItems": 1, "items": { "type": "string" } },
    "halt_actor": { "type": "array", "minItems": 1, "items": { "type": "string" } },
    "user_inspect": { "type": "string", "minLength": 1 },
    "user_exit": { "type": "string", "minLength": 1 },
    "model_host": { "type": "string" },
    "model_region": { "type": "string" }
  }
}
```

Validate the contract before the first `EvidenceEnvelope` is accepted. Reject the process, do not start with a partial contract.

---

## 3. EvidenceEnvelope

Every captured event is this object. There is no other ingress.

```ts
type EvidenceKind =
  | "count"
  | "error_class"
  | "path"
  | "task_result"
  | "abandonment"
  | "explicit_preference"
  | "diagnostic";

interface EvidenceEnvelope {
  id: string;
  ts: string;
  surface: string;
  contract_version: number;
  contract_field: string;
  kind: EvidenceKind;
  value: number | string | boolean | Record<string, unknown>;
  cohort_id: string;
  deploy_id: string;
  text?: { untrusted: true; bytes: string };
}
```

Invariants:

- `id` is a UUID. `ts` is RFC 3339 UTC.
- `contract_field` must exist on `TelemetryContract.fields` at `contract_version`. Else halt (`contract_breach`).
- `kind` must equal the contract field's `type`.
- `cohort_id` is a rotating cohort token, not a durable user id, email, pubkey, or device id. Lifetime ≤ `retention_days`.
- `deploy_id` names the running binary or release the event was produced under. Qualification uses it to drop deploy-explained spikes.
- `text` is optional. If present it is `{ untrusted: true, bytes }`. Never concatenate `bytes` into a system prompt, tool description, or commit message template. Pass it to the model only as a named data blob, and only if `model_exposure` is true for that field.
- If a later `Opportunity` is supported only by envelopes that contain `text` and no structured `value`, `generate` is forbidden. A human must approve.

```json
{
  "$id": "https://vibeware.spec/evidence-envelope.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "EvidenceEnvelope",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "id", "ts", "surface", "contract_version", "contract_field",
    "kind", "value", "cohort_id", "deploy_id"
  ],
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "ts": { "type": "string", "format": "date-time" },
    "surface": { "type": "string" },
    "contract_version": { "type": "integer", "minimum": 1 },
    "contract_field": { "type": "string" },
    "kind": {
      "type": "string",
      "enum": ["count", "error_class", "path", "task_result", "abandonment", "explicit_preference", "diagnostic"]
    },
    "value": {},
    "cohort_id": { "type": "string", "minLength": 8, "maxLength": 64 },
    "deploy_id": { "type": "string", "minLength": 1 },
    "text": {
      "type": "object",
      "additionalProperties": false,
      "required": ["untrusted", "bytes"],
      "properties": {
        "untrusted": { "const": true },
        "bytes": { "type": "string", "maxLength": 4096 }
      }
    }
  }
}
```

Retention job: delete or irreversibly aggregate envelopes older than `retention_days`. The qualifier must not read expired envelopes.

---

## 4. Objective

The objective is declared before any envelope is qualified. It is not inferred from traffic.

```ts
type IllegalObjective =
  | "dwell"
  | "session_length"
  | "raw_event_count"
  | "ticket_volume"
  | "merge_count"
  | "engagement";

interface Objective {
  id: string;
  statement: string;
  litmus: string;
  illegal: false;
  judgment_only: boolean;
  min_n_per_variant?: number;
}
```

Litmus: a dumb harmful change must not be able to improve this objective. If it can, the objective is illegal and the loop does not start.

Power (optional, required when `judgment_only` is false): `n ≈ 16σ²/δ²` users per variant for roughly 80% power. If you cannot measure σ, or `min_n_per_variant` is larger than observed unique `cohort_id`s on the surface, set `judgment_only: true`. Do not pretend a dashboard decided.

```json
{
  "$id": "https://vibeware.spec/objective.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Objective",
  "type": "object",
  "additionalProperties": false,
  "required": ["id", "statement", "litmus", "illegal", "judgment_only"],
  "properties": {
    "id": { "type": "string" },
    "statement": { "type": "string", "minLength": 8 },
    "litmus": { "type": "string", "minLength": 8 },
    "illegal": { "const": false },
    "judgment_only": { "type": "boolean" },
    "min_n_per_variant": { "type": "integer", "minimum": 1 }
  }
}
```

---

## 5. Qualifier

Qualification is a pure function. It is not a prompt.

```ts
type RejectReason =
  | "once_only"
  | "single_cohort"
  | "explained_by_deploy"
  | "explained_by_incident"
  | "power_user"
  | "n_too_small"
  | "crosses_tier"
  | "requires_contract_change"
  | "wip_full"
  | "unknown_field"
  | "illegal_objective"
  | "unbound_to_objective"
  | "text_only"
  | "halted";

interface Reject {
  ok: false;
  reason: RejectReason;
  evidence_ids: string[];
}

interface QualifyInput {
  envelopes: EvidenceEnvelope[];
  contract: TelemetryContract;
  objective: Objective;
  surface_tier: ConsequenceClass;
  open_opportunities: number;
  wip_cap: number;
  known_incident_ids: string[];
  halted: boolean;
  power_user_cohort_ids: string[];
}

type QualifyResult = Opportunity | Reject;
```

Defaults: `wip_cap = 3`. Repetition bar: at least 2 envelopes, at least 2 distinct `cohort_id`s, unless the owner pre-declared a synthetic check and listed its `cohort_id`.

Predicates, all must hold for `ok`:

1. `halted === false`.
2. `objective.illegal === false`.
3. Every envelope's `contract_field` is on `contract.fields` and not in `forbidden_fields`.
4. `|envelopes| ≥ 2` and `|unique cohort_id| ≥ 2` (or the synthetic exception).
5. The envelopes are not all explained by one `deploy_id` that shipped in the last 2 hours, and none carry a `known_incident_id`.
6. No more than 50% of envelopes share a `cohort_id` that is in `power_user_cohort_ids`.
7. `objective.judgment_only === true` or unique `cohort_id` count ≥ `objective.min_n_per_variant`.
8. Proposed action's implied class ≤ `surface_tier`. T3 never qualifies.
9. Acting does not require a new contract field.
10. `open_opportunities < wip_cap`.
11. At least one envelope has a structured `value` bound to `objective.id`. A bag of `text` is `text_only` and does not qualify for generation.

Most batches return `Reject`. That is the design.

Log every reject with `reason`. Funnel metric `qualified` increments only on `Opportunity`.

---

## 6. Opportunity

```ts
interface Opportunity {
  ok: true;
  id: string;
  created_at: string;
  expires_at: string;
  evidence_ids: string[];
  problem: string;
  objective_id: string;
  allowed_paths: string[];
  forbidden_paths: string[];
  tier: ConsequenceClass;
  judgment_only: boolean;
}
```

Invariants:

- `problem` states a user problem, not a prescribed patch. Reject creation if the string matches `/\b(implement|refactor|rewrite|merge)\b/i` as the first verb.
- `allowed_paths` is a glob list. The agent may only write inside it.
- `forbidden_paths` is unioned with the global denylist in §7. The intersection with `allowed_paths` must be empty.
- `expires_at` is `created_at + 14d` unless the owner sets a shorter TTL. Expired opportunities cannot enter `generate`.
- `tier` is the surface class. It cannot be upgraded by the agent.

```json
{
  "$id": "https://vibeware.spec/opportunity.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Opportunity",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "ok", "id", "created_at", "expires_at", "evidence_ids", "problem",
    "objective_id", "allowed_paths", "forbidden_paths", "tier", "judgment_only"
  ],
  "properties": {
    "ok": { "const": true },
    "id": { "type": "string", "format": "uuid" },
    "created_at": { "type": "string", "format": "date-time" },
    "expires_at": { "type": "string", "format": "date-time" },
    "evidence_ids": { "type": "array", "minItems": 2, "items": { "type": "string", "format": "uuid" } },
    "problem": { "type": "string", "minLength": 8 },
    "objective_id": { "type": "string" },
    "allowed_paths": { "type": "array", "minItems": 1, "items": { "type": "string" } },
    "forbidden_paths": { "type": "array", "items": { "type": "string" } },
    "tier": { "$ref": "https://vibeware.spec/consequence-class.json" },
    "judgment_only": { "type": "boolean" }
  }
}
```

---

## 7. AgentCapabilityEnvelope

Authority is a stage, not a global role.

```ts
type AgentStage = "observe" | "propose" | "generate" | "expose" | "select_in_envelope";

interface AgentCapabilityEnvelope {
  stage: AgentStage;
  tools: string[];
  repo_allow: string[];
  path_allow: string[];
  path_deny: string[];
  can_merge: boolean;
  can_approve_self: false;
  secret_scan_before_tree: true;
}
```

Stage grants:

| Stage | Tools | Source | Deploy |
|---|---|---|---|
| `observe` | `read_contract`, `read_envelopes`, `read_rejects`, `summarize` | none | none |
| `propose` | observe + `write_proposal` | none | none |
| `generate` | propose + `open_branch`, `open_pr` | write inside `path_allow` | none |
| `expose` | generate + `create_flag`, `assign_cohort` | merge to experimental namespace only | flag or experimental URL |
| `select_in_envelope` | expose + `set_selection` inside a pre-registered envelope | no T2/T3 paths | no contract mutation |

Global `path_deny` (always on, cannot be removed by the agent):

```
**/.github/workflows/**
**/Dockerfile*
**/docker-compose*.{yml,yaml}
**/*.{tf,tfvars}
**/secrets/**
**/.env*
**/*lock*.{json,toml}
**/pnpm-lock.yaml
**/package-lock.json
**/Cargo.lock
**/*payment*
**/*custody*
**/*signer*
**/*keystore*
**/*protocol*spec*
```

A path that matches `path_deny` is T3-adjacent. `generate` on that path is rejected. A human who did not author the diff must review any PR that touches a path within one directory of a deny match.

Rules:

- `can_approve_self` is always `false`. The merge actor ≠ the authoring agent identity.
- `secret_scan_before_tree` is always `true`. If the scan fails, the agent does not receive the tree.
- Unit tests written by the authoring agent are not sufficient. Existing CI must pass, and a human must reproduce the friction or a weaker sealed checker with no access to the author's notes must pass.
- Promoting `stage` is a human write after the gates in §11. Demoting `stage` is automatic when any halt in §10 fires.

```json
{
  "$id": "https://vibeware.spec/agent-capability-envelope.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "AgentCapabilityEnvelope",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "stage", "tools", "repo_allow", "path_allow", "path_deny",
    "can_merge", "can_approve_self", "secret_scan_before_tree"
  ],
  "properties": {
    "stage": {
      "type": "string",
      "enum": ["observe", "propose", "generate", "expose", "select_in_envelope"]
    },
    "tools": { "type": "array", "items": { "type": "string" } },
    "repo_allow": { "type": "array", "items": { "type": "string" } },
    "path_allow": { "type": "array", "items": { "type": "string" } },
    "path_deny": { "type": "array", "minItems": 1, "items": { "type": "string" } },
    "can_merge": { "type": "boolean" },
    "can_approve_self": { "const": false },
    "secret_scan_before_tree": { "const": true }
  }
}
```

---

## 8. Candidate

Generation produces a branch and a pull request. It does not produce a production deploy.

```ts
type CiStatus = "pending" | "passed" | "failed";

interface Candidate {
  id: string;
  opportunity_id: string;
  branch: string;
  pr_url: string;
  files_touched: string[];
  high_risk_paths_touched: string[];
  ci: CiStatus;
  human_reviewer?: string;
  kill_switch_id: string;
  flag_id?: string;
  expires_at: string;
  author_agent_id: string;
}
```

Invariants:

- `files_touched` ⊆ `Opportunity.allowed_paths` and ∩ `path_deny` = ∅. Else the PR is closed and the event is a halt if the write landed.
- `high_risk_paths_touched` is the subset of `files_touched` within one directory of a deny glob. If non-empty, `human_reviewer` is required and ≠ `author_agent_id`.
- `ci` must be `passed` before `expose`.
- `kill_switch_id` must reference a live `HaltController` switch that disables this candidate's flag and write access.
- `expires_at` ≤ 14 days from open. Expired candidates cannot expose.

```json
{
  "$id": "https://vibeware.spec/candidate.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Candidate",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "id", "opportunity_id", "branch", "pr_url", "files_touched",
    "high_risk_paths_touched", "ci", "kill_switch_id", "expires_at",
    "author_agent_id"
  ],
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "opportunity_id": { "type": "string", "format": "uuid" },
    "branch": { "type": "string" },
    "pr_url": { "type": "string", "format": "uri" },
    "files_touched": { "type": "array", "items": { "type": "string" } },
    "high_risk_paths_touched": { "type": "array", "items": { "type": "string" } },
    "ci": { "type": "string", "enum": ["pending", "passed", "failed"] },
    "human_reviewer": { "type": "string" },
    "kill_switch_id": { "type": "string" },
    "flag_id": { "type": "string" },
    "expires_at": { "type": "string", "format": "date-time" },
    "author_agent_id": { "type": "string" }
  }
}
```

---

## 9. Exposure

A candidate is not in the loop until a person can use it.

```ts
type ExposurePopulation = "owner" | "opt_in_cohort" | "public_experimental";

interface Peek {
  at: string;
  actor: string;
}

interface Exposure {
  id: string;
  candidate_id: string;
  population: ExposurePopulation;
  ttl: string;
  pre_registered_outcome: string;
  peek_log: Peek[];
  kill_switch_id: string;
  judgment_only: boolean;
  flag_key: string;
  enabled: boolean;
}
```

Invariants:

- `pre_registered_outcome` is written before the first user is assigned. Peeking after assignment appends to `peek_log`. There is no "ship when p < 0.05." Fixed horizon or always-valid sequential tests only.
- If `Opportunity.judgment_only` or `Objective.judgment_only` is true, `judgment_only` is true and no statistical claim may be stored on the selection.
- `ttl` ≤ 14 days. At expiry, `enabled` becomes false without a meeting.
- `kill_switch_id` must match the candidate's switch. Flipping it sets `enabled = false` and removes the cohort assignment.
- T2 exposures require `population !== "public_experimental"` unless the opportunity names a namespace and a deletion or forward-correction plan.
- T3 never exposes generated adapters.

```json
{
  "$id": "https://vibeware.spec/exposure.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Exposure",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "id", "candidate_id", "population", "ttl", "pre_registered_outcome",
    "peek_log", "kill_switch_id", "judgment_only", "flag_key", "enabled"
  ],
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "candidate_id": { "type": "string", "format": "uuid" },
    "population": { "type": "string", "enum": ["owner", "opt_in_cohort", "public_experimental"] },
    "ttl": { "type": "string", "format": "date-time" },
    "pre_registered_outcome": { "type": "string", "minLength": 8 },
    "peek_log": {
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": ["at", "actor"],
        "properties": {
          "at": { "type": "string", "format": "date-time" },
          "actor": { "type": "string" }
        }
      }
    },
    "kill_switch_id": { "type": "string" },
    "judgment_only": { "type": "boolean" },
    "flag_key": { "type": "string" },
    "enabled": { "type": "boolean" }
  }
}
```

Outcome events after exposure are ordinary `EvidenceEnvelope`s with `deploy_id` set to the candidate's `flag_key`. They re-enter the qualifier. They do not auto-promote.

---

## 10. Selection

Selection is a state machine. Merge is not a state.

```ts
type SelectionState =
  | "keep_vibing"
  | "redirect"
  | "archive"
  | "graduate"
  | "remain_standalone"
  | "extract_primitive";

interface GraduationRecord {
  maintainer: string;
  shortcuts: string[];
  decision: "merge" | "rewrite" | "extract" | "discard_code_keep_insight";
}

interface Selection {
  candidate_id: string;
  state: SelectionState;
  actor: string;
  at: string;
  reason: string;
  graduation?: GraduationRecord;
}
```

Transitions:

```
(unselected) → keep_vibing | redirect | archive
keep_vibing  → redirect | archive | graduate | remain_standalone | extract_primitive
redirect     → keep_vibing | archive
archive      → (terminal)
graduate     → (terminal; requires GraduationRecord)
remain_standalone → archive | graduate
extract_primitive → graduate | archive
```

Rules:

- Default at review if `actor` is empty: `archive`.
- `graduate` requires `GraduationRecord.maintainer` (a named person, not a team) and a non-empty `shortcuts` list. An empty shortcuts list is a lie; write `"none"` only if the owner will sign that sentence.
- `extract_primitive` means several candidates proved the same missing capability. The prototype code is not the primitive.
- Taste may force `archive` on a candidate that improved the objective.

```json
{
  "$id": "https://vibeware.spec/selection.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Selection",
  "type": "object",
  "additionalProperties": false,
  "required": ["candidate_id", "state", "actor", "at", "reason"],
  "properties": {
    "candidate_id": { "type": "string", "format": "uuid" },
    "state": {
      "type": "string",
      "enum": [
        "keep_vibing", "redirect", "archive",
        "graduate", "remain_standalone", "extract_primitive"
      ]
    },
    "actor": { "type": "string", "minLength": 1 },
    "at": { "type": "string", "format": "date-time" },
    "reason": { "type": "string", "minLength": 8 },
    "graduation": {
      "type": "object",
      "additionalProperties": false,
      "required": ["maintainer", "shortcuts", "decision"],
      "properties": {
        "maintainer": { "type": "string", "minLength": 1 },
        "shortcuts": { "type": "array", "minItems": 1, "items": { "type": "string" } },
        "decision": {
          "type": "string",
          "enum": ["merge", "rewrite", "extract", "discard_code_keep_insight"]
        }
      }
    }
  },
  "allOf": [
    {
      "if": { "properties": { "state": { "const": "graduate" } } },
      "then": { "required": ["graduation"] }
    }
  ]
}
```

---

## 11. HaltController

Halt is a single object. Any actor in `TelemetryContract.halt_actor` can flip it. No meeting.

```ts
type HaltReason =
  | "oracle_fail"
  | "review_overflow"
  | "corrective_spike"
  | "secret_or_supply_chain"
  | "starvation"
  | "contract_breach"
  | "canary_retrieval"
  | "no_advocate"
  | "manual";

interface HaltController {
  halted: boolean;
  reason?: HaltReason;
  at?: string;
  actor?: string;
  open_agent_prs: number;
  consecutive_oracle_failures: number;
  cycles_without_qualified: number;
  kill_switches: Record<string, boolean>;
}
```

Predicates (evaluate on a timer, not on trust):

| Reason | Fire when |
|---|---|
| `oracle_fail` | `consecutive_oracle_failures >= 3` |
| `review_overflow` | `open_agent_prs > 10` (drain: review and close only until `< 5`) |
| `corrective_spike` | week-over-week rise in commits whose first parent is an agent candidate |
| `secret_or_supply_chain` | any leaked secret, unpinned action, or unpinned image merged from an agent PR |
| `starvation` | `cycles_without_qualified >= 10` |
| `contract_breach` | envelope field not on contract, or agent tool attempted a contract write |
| `canary_retrieval` | a planted canary string from user text appeared in a tool call |
| `no_advocate` | review ended with no `Selection.actor` |
| `manual` | any `halt_actor` |

On halt:

1. `AgentCapabilityEnvelope.stage` becomes `observe`.
2. `can_merge` becomes `false`.
3. Every `Exposure.enabled` becomes `false`.
4. Every `kill_switches[id]` becomes `true`.
5. Qualifier returns `Reject { reason: "halted" }` for new batches.

Restart is a human write by a `halt_actor`. `secret_or_supply_chain` and `contract_breach` require a second `halt_actor`.

First cycle: fire `manual` once in week 1 and confirm flags and write tools actually died. If they did not, the loop does not proceed.

```json
{
  "$id": "https://vibeware.spec/halt-controller.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "HaltController",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "halted", "open_agent_prs", "consecutive_oracle_failures",
    "cycles_without_qualified", "kill_switches"
  ],
  "properties": {
    "halted": { "type": "boolean" },
    "reason": {
      "type": "string",
      "enum": [
        "oracle_fail", "review_overflow", "corrective_spike",
        "secret_or_supply_chain", "starvation", "contract_breach",
        "canary_retrieval", "no_advocate", "manual"
      ]
    },
    "at": { "type": "string", "format": "date-time" },
    "actor": { "type": "string" },
    "open_agent_prs": { "type": "integer", "minimum": 0 },
    "consecutive_oracle_failures": { "type": "integer", "minimum": 0 },
    "cycles_without_qualified": { "type": "integer", "minimum": 0 },
    "kill_switches": {
      "type": "object",
      "additionalProperties": { "type": "boolean" }
    }
  }
}
```

---

## 12. Required logs

The application must emit these counters. If a counter is missing, capability cannot leave `observe`.

| Name | Increments on |
|---|---|
| `funnel.raw` | accepted `EvidenceEnvelope` |
| `funnel.rejected.{reason}` | each `Reject` |
| `funnel.qualified` | each `Opportunity` |
| `funnel.candidates_opened` | each `Candidate` |
| `funnel.candidates_rejected` | PR closed without expose |
| `funnel.exposures` | each `Exposure` with `enabled=true` |
| `funnel.selections.{state}` | each `Selection` |
| `agent.files_touched` | union of `Candidate.files_touched` |
| `agent.high_risk_paths` | union of `high_risk_paths_touched` |
| `review.time_to_first_review_seconds` | first human comment on an agent PR |
| `review.secret_comment_rate` | comments that name a secret, not merges |
| `post.corrective_commits` | human commits on files last touched by an agent candidate |
| `post.unpinned_deps` | unpinned action or image diffs in agent PRs |
| `halt.events` | each HaltController transition to `halted=true` |

Forbidden as logged generation inputs: full session replay, sentiment scores, engagement weights the agent can retune, cross-product identity joins not on the contract.

---

## 13. Wiring checklist

The loop does not start, and the agent does not leave `observe`, until all of the following exist and validate:

1. `ConsequenceClass` on the surface.
2. `TelemetryContract` (schema-valid, no empty required fields).
3. `Objective` with a passing litmus and `illegal: false`.
4. Evidence ingress that only accepts `EvidenceEnvelope`.
5. Qualifier as a pure function with the predicates in §5.
6. `AgentCapabilityEnvelope` at `observe`, `can_approve_self: false`, `secret_scan_before_tree: true`, global `path_deny` installed.
7. `HaltController` with at least one `kill_switches` entry and at least one `halt_actor`.
8. The kill switch exercised once (`manual`), flags and write tools confirmed dark, then restarted by a human.
9. The log names in §12 emitting.

Capability promotions (human write, after the previous stage is boring):

| From | To | Gate |
|---|---|---|
| `observe` | `propose` | two review cycles of funnel counts, zero contract breaches |
| `propose` | `generate` | same, plus rejects dominating qualifies |
| `generate` | `expose` | no secret incidents, no unpinned supply-chain diffs merged, review happening, `post.corrective_commits` not rising, tested kill switch |
| `expose` | `select_in_envelope` | not in the first cycle; requires a pre-registered envelope and T0/T1 only |

Move backward automatically on any halt.

---

## 14. When not to attach this loop

Do not instantiate these objects when:

- The job is to invent a surface users cannot yet ask for. Use R&D.
- The surface is T3.
- `n` is too small and you will not set `judgment_only: true`.
- No person will be `GraduationRecord.maintainer` if a candidate wins.
- You would have to build a new telemetry platform to produce the first envelope. Use diagnostics you already have or do not start.
- The only available objective fails the litmus test.
- The change would write durable signed or shared state you cannot later correct.
- Taste, not the loop, should decide. A real repeated signal can still be wrong.

Vibeware is attached when use can produce envelopes, qualification can refuse most of them, generation is path-bounded, exposure is reversible, selection is explicit, and a halt actor can turn the whole thing off. It is not attached when any of those objects is a slide.
