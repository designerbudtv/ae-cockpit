# Canonical State Manager

## Purpose

Persistent canonical state management for AVOS Core, independent of Base44 runtime dependencies.

## Architecture

```
state-manager/
├── state-manager.md    # This file
├── persistence/        # Storage abstraction layer
│   ├── file-system.md  # File-based persistence
│   ├── database.md     # Database-backed persistence
│   └── cache.md        # Caching layer
├── operations/         # State operations
│   ├── create.md       # Create new state entries
│   ├── read.md         # Query and read state
│   ├── update.md       # Update existing state
│   ├── delete.md       # Soft-delete with preservation
│   ├── merge.md        # Merge related state entries
│   └── restore.md      # Rollback to previous version
└── validation/         # State validation
    ├── schema.md       # Schema validation
    ├── integrity.md    # Data integrity checks
    └── consistency.md  # Cross-entry consistency
```

## State Categories

### 1. Identity State
- TARGET_IDs with full lifecycle
- URL canonicalization maps
- Alias and redirect chains
- Migration history

### 2. Knowledge State
- Semantic territory data
- Keyword binding evidence
- KH maturation records
- Verified learning entries

### 3. GGE State
- Territory state transitions
- Attack/expand/hold states
- Competitive intelligence
- Market signals

### 4. Evidence State
- All L1-L4 proof entries
- External effect recordings
- Verification timestamps
- Confidence scores

### 5. Configuration State
- Runtime parameters
- Rate limits and budgets
- Provider configurations
- Scheduler settings

## State Operations

### Create
```
create(category, entry) → state_version
- Generate unique ID
- Apply schema validation
- Set created timestamp
- Increment version counter
- Store in versioned storage
```

### Read
```
read(category, id) → entry | null
- Query by category and ID
- Apply filters if provided
- Return full entry or null
- Log access if auditable
```

### Update
```
update(category, id, changes) → state_version
- Load current version
- Validate changes against schema
- Apply changes with diff
- Create new version
- Log change with reason
```

### Delete (Soft)
```
soft_delete(category, id, reason) → deleted_version
- Mark as deleted (not remove)
- Record deletion reason
- Preserve all historical data
- Update active flag to false
```

### Merge
```
merge(category, id_source, id_target, rules) → merged_version
- Load both entries
- Apply merge rules
- Resolve conflicts
- Create merged entry
- Archive original entries
- Preserve merge audit trail
```

### Restore
```
restore(category, id, version) → restored_entry
- Load specified version
- Validate integrity
- Replace current version
- Log restoration event
- Trigger re-verification
```

## Version Control

Every state change creates a new version:

```json
{
  "version": 3,
  "timestamp": "ISO8601",
  "author": "system/provider",
  "changes": {
    "field": {
      "from": "old_value",
      "to": "new_value"
    }
  },
  "reason": "Change justification",
  "proof_refs": ["proof_id_1", "proof_id_2"],
  "previous_version": 2,
  "checksum": "sha256_hash_of_entry"
}
```

## Persistence Strategy

### File-Based (Default)
- JSON files per category
- Versioned in Git
- Human-readable
- Natural backup via Git

### Database-Backed (Optional)
- SQL for relational data
- NoSQL for flexible schemas
- Indexed for fast queries
- ACID compliance

### Caching
- In-memory cache for hot data
- TTL-based expiration
- Write-through cache
- Cache invalidation on write

## Integrity Checks

### Checksums
- SHA-256 for all entries
- Verified on read/write
- Alert on mismatch

### Cross-Reference
- All references must resolve
- No orphaned entries
- Cascade updates on delete

### Audit Trail
- Every operation logged
- Who/what/when/why
- Immutable audit log

## Parity with Base44

Before retiring Base44 dependencies:

1. Verify all data accessible via state manager
2. Verify all operations produce same results
3. Verify performance meets requirements
4. Verify recovery procedures work
5. Document any differences
6. Obtain sign-off for migration

## API Contract

### Internal
```
StateManager
├── create(category, data) → Version
├── read(category, id) → Entry
├── update(category, id, changes) → Version
├── list(category, filters) → [Entry]
├── delete(category, id) → Version
├── restore(category, id, version) → Entry
├── merge(category, ids, rules) → Version
├── verifyIntegrity() → Report
└── export(category, format) → Data
```

### External (REST)
```
GET    /api/v1/state/{category}          # List
GET    /api/v1/state/{category}/{id}     # Read
POST   /api/v1/state/{category}          # Create
PUT    /api/v1/state/{category}/{id}     # Update
DELETE /api/v1/state/{category}/{id}     # Delete
POST   /api/v1/state/{category}/{id}/restore  # Restore
```

## Implementation Notes

- Use immutable data patterns
- Prefer functional over imperative
- Log all state transitions
- Support concurrent access
- Graceful degradation on failure
- Test with mock data first
