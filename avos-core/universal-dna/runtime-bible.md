# Universal DNA Runtime Bible

## 1. Runtime Architecture Overview

The Universal DNA serves as the reusable implementation intelligence layer for AVOS Core. It provides:

- **Identity Contracts**: Universal Registry specifications for TARGET_ID, aliases, redirects
- **Runtime Adaptation**: Behavioral intelligence for self-modifying operation
- **Canonical State**: Persistent state management independent of Base44

### Core Components

```
universal-dna/
├── runtime-bible.md          # This file - Runtime architecture
├── registry-contracts/       # Identity contracts
│   ├── identity.md           # TARGET_ID contract
│   ├── target-id.md          # ID resolution contract
│   └── aliases-redirects.md  # Alias management contract
├── runtime-adaptation/       # Runtime intelligence
│   ├── behavioral.md         # Behavioral adaptation patterns
│   └── self-modifying.md     # Self-modification protocols
└── integration/              # Integration specifications
    ├── signal-paths.md       # Signal path contracts
    └── proof-system.md       # Proof hierarchy contracts
```

## 2. Universal Registry Contracts

### 2.1 TARGET_ID Contract

Every governed URL must resolve to a persistent TARGET_ID with:

- **Uniqueness**: Globally unique within the AVOS estate
- **Persistence**: Survives repository operations and migrations
- **Resolvability**: Can be resolved from URL, canonical form, or any alias
- **History**: Complete resolution history preserved

#### TARGET_ID Structure

```json
{
  "target_id": "tid-[hash]",
  "canonical_url": "https://example.com/target",
  "resolved_at": "ISO8601-timestamp",
  "resolved_by": "provider/source",
  "aliases": ["https://...", "http://..."],
  "redirects": ["301: https://...", "302: https://..."],
  "status": "active|inactive|pending|merged|deleted",
  "metadata": {
    "first_seen": "ISO8601",
    "last_verified": "ISO8601",
    "verification_count": int,
    "confidence": float(0-1)
  }
}
```

### 2.2 Identity Resolution Rules

1. **Normalize URL**: Strip protocols, normalize case, resolve unicode
2. **Check Existing**: Query registry for matching TARGET_ID
3. **If Found**: Attach alias, update metadata, verify current
4. **If Not Found**: Generate new TARGET_ID, create canonical entry
5. **On Conflict**: Log collision, preserve historical priority, merge if appropriate

### 2.3 Alias Management

- All URL variants (HTTP/HTTPS, www/non-www, trailing slash) are aliases
- Redirect chains are tracked with source and destination
- Canonical URL is the preferred, authoritative form
- Historical aliases are preserved for back-reference

## 3. Runtime Adaptation

### 3.1 Behavioral Intelligence

The runtime must adapt to:

- **URL Changes**: New content detected on monitored URLs
- **Search Changes**: Position/visibility changes in SERPs
- **Competitor Movements**: Changes in competitor territory
- **Technical Events**: New indexing, deindexing, crawl errors
- **Commercial Signals**: Lead/conversion activity changes

### 3.2 Self-Modification Protocols

The runtime can modify its own:

- **Configuration**: Adjust rates, priorities, parameters
- **Registry**: Add new patterns, update rules
- **Knowledge**: Add verified learning to semantic territory
- **Evidence**: Store new proof from external observations

Self-modification is constrained by:

- **Proof Requirement**: Changes require L1+ evidence
- **Parity Check**: Modified systems must maintain parity with original
- **Rollback**: All changes are versioned and reversible
- **Audit Trail**: Every modification is logged with reason and evidence

## 4. Canonical State Management

### 4.1 State Persistence

All AVOS state must be:

- **Persistent**: Survives restarts and operations
- **Versioned**: Every change is versioned
- **Queryable**: State can be queried and filtered
- **Restorable**: State can be restored from any version

### 4.2 State Categories

1. **Identity**: TARGET_IDs, aliases, redirects, canonical URLs
2. **Knowledge**: Semantic territory, KH, keyword bindings
3. **GGE**: Territory states, attack/expand/hold states
4. **Evidence**: All proof from L1 to L4
5. **Learning**: Verified learning from past operations
6. **Configuration**: Runtime configuration and parameters

### 4.3 State Operations

- **Create**: Generate new state entries
- **Read**: Query existing state
- **Update**: Modify existing state (with versioning)
- **Delete**: Soft-delete with preservation (for audits)
- **Merge**: Combine duplicate or related state
- **Restore**: Rollback to previous version

## 5. Integration Contracts

### 5.1 Signal Path Contract

Every signal path must include:

```json
{
  "signal_id": "sig-[uuid]",
  "source": "provider/type",
  "destination": "target/TARGET_ID",
  "event": "event_type",
  "configuration": {...},
  "verification": {...},
  "transmission": {
    "timestamp": "ISO8601",
    "status": "sent|received|failed|pending"
  },
  "receipt": {
    "timestamp": "ISO8601",
    "verified_by": "provider",
    "confirmed": bool
  },
  "attribution": {
    "type": "direct|assisted|organic|paid",
    "value": float,
    "currency": "string"
  },
  "evidence": ["proof_refs"]
}
```

### 5.2 Proof System Contract

```json
{
  "proof_id": "prf-[uuid]",
  "level": "L1|L2|L3|L4",
  "action": "action_ref",
  "execution": "execution_ref",
  "external_effect": {
    "url": "observed_url",
    "change": "description_of_change",
    "timestamp": "ISO8601",
    "verified": bool
  },
  "outcome": {
    "metric": "metric_name",
    "before": value,
    "after": value,
    "delta": value,
    "significant": bool
  },
  "confidence": float(0-1),
  "evidence": ["evidence_refs"],
  "timestamp": "ISO8601"
}
```

## 6. Behavioral Patterns

### 6.1 URL Monitoring Pattern

```
1. Normalize URL
2. Check if governed
3. If governed:
   a. Resolve TARGET_ID
   b. Load living profile
   c. Inspect current state
   d. Compare with expected state
   e. If change detected:
      - Record change
      - Update evidence
      - Trigger C44 diagnosis
      - If warranted, dispatch action
4. If new:
   a. Add to governed estate
   b. Create initial TARGET_ID
   c. Create initial living profile
   d. Initialize semantic territory
5. Schedule next inspection
```

### 6.2 Semantic Territory Pattern

```
1. For each URL:
   a. Analyze content for keyword opportunities
   b. Compare with existing bindings
   c. Add new bindings with validation
   d. Remove invalid bindings
   e. Validate intended/observed/verified territory
   f. Check for cannibalization
   g. Update KH maturation
2. For each binding:
   a. Monitor performance
   b. Update confidence based on results
   c. Archive if long-term negative
   d. Promote if long-term positive
```

### 6.3 GGE State Transition Pattern

```
State Machine:
  ATTACK -> EXPAND (if metrics improving)
  ATTACK -> HOLD (if metrics stable)
  ATTACK -> WATCH (if metrics declining)
  EXPAND -> HOLD (if growth plateau)
  EXPAND -> ATTACK (if acceleration)
  HOLD -> ATTACK (if opportunity detected)
  HOLD -> DEFEND (if threat detected)
  HOLD -> WATCH (if market declining)
  DEFEND -> WATCH (if threat neutralized)
  WATCH -> ATTACK (if opportunity detected)
  WATCH -> DEPRIORITIZE (if declining without recovery)
  REDIRECT -> HOLD (if transition complete)
  DEPRIORITIZE -> WATCH (if re-evaluation triggered)
```

## 7. Implementation Directives

1. **Do not redesign**: Use this bible as-is
2. **Reuse existing**: Implement only gaps and corrections
3. **Preserve history**: Never delete canonical records
4. **Verify before modify**: All modifications require evidence
5. **Audit everything**: Log all state changes
6. **Rollback ready**: All operations must be reversible
7. **Independent operation**: Do not depend on Base44 for core functions

## 8. Runtime Bible Appendix

### 8.1 Terminology

- **TARGET_ID**: Unique persistent identifier for a governed URL
- **Living Profile**: Complete target information (not just ID)
- **Semantic Territory**: Intended/observed/verified keyword landscape
- **KH**: Keyword Health maturation tracking
- **GGE**: Geographic Geographic Intent intelligence
- **ALIVE**: Technical, discoverability, semantic, relational, geographic, human, economic life
- **C44**: Observe/interpret/diagnose/hypothesize system
- **AE²**: Autonomous Estate Allocation engine
- **Workforce25**: Provider registry and execution system
- **Proof**: L1-L4 evidence hierarchy

### 8.2 File Naming Convention

- Markdown for documentation
- JSON for data structures
- YAML for configuration
- All files use lowercase with hyphens

### 8.3 Commit Convention

- Commit messages follow [MODULE] action description format
- Example: [REGISTRY] add TARGET_ID resolution for example.com
- All implementation ledger items reference REQUIREMENT_ID
