# Proof System - L1 to L4 Evidence Hierarchy

## Purpose

Never promote without evidence. Store verified learning only where proof supports it and constrain transferability to supported scope.

## Proof Levels

### L1: Action/Configuration Evidence
Evidence that an action was taken or configuration was changed.

**Sources:**
- Scheduler execution logs
- Provider invocation records
- Configuration change manifests
- Deployment artifacts
- API request/response pairs

**Validation:**
- Action exists in execution manifest
- Timestamps are consistent
- Required parameters were present
- Provider acknowledged receipt

**Storage:**
```json
{
  "proof_id": "prf-l1-[uuid]",
  "level": "L1",
  "action_ref": "manifest-[id]",
  "configuration_ref": "config-[id]",
  "timestamp": "ISO8601",
  "evidence": ["execution_log", "provider_receipt"],
  "confidence": float(0.8-1.0)
}
```

### L2: Execution Evidence
Evidence that an action was executed successfully.

**Sources:**
- Provider execution receipts
- Task completion records
- Response status codes
- Content extraction results
- API response data

**Validation:**
- Execution completed without error
- Response matches expected schema
- Data volume is non-zero (when applicable)
- Timestamps are within bounds

**Storage:**
```json
{
  "proof_id": "prf-l2-[uuid]",
  "level": "L2",
  "execution_ref": "manifest-[id]",
  "status": "success|partial",
  "response_data": {...},
  "data_size": int,
  "timestamp": "ISO8601",
  "provider_receipt": "receipt_hash",
  "evidence": ["provider_manifest", "response_data"],
  "confidence": float(0.7-1.0)
}
```

### L3: External Effect Evidence
Evidence that an action produced an observable external effect.

**Sources:**
- SERP position changes
- Content availability changes
- Indexing status changes
- Crawl behavior changes
- Third-party platform updates

**Validation:**
- Effect observed by independent source
- Effect correlates with L2 execution
- Effect is not from other sources
- Effect persists over observation window

**Storage:**
```json
{
  "proof_id": "prf-l3-[uuid]",
  "level": "L3",
  "execution_ref": "manifest-[id]",
  "effect_type": "indexing|position|content|availability",
  "target_url": "observed_url",
  "before_state": {...},
  "after_state": {...},
  "observation_method": "search_api|direct_crawl|platform_api",
  "observation_timestamp": "ISO8601",
  "correlation_confidence": float(0-1),
  "evidence": ["observation_record", "comparison_data"],
  "confidence": float(0.5-0.9)
}
```

### L4: Target/Outcome Change Evidence
Evidence that an action produced the desired target/outcome change.

**Sources:**
- Metrics before/after comparison
- KPI changes
- Commercial results
- User behavior changes
- Business outcome data

**Validation:**
- Metric change is statistically significant
- Change direction matches intent
- Change duration exceeds normal variance
- Alternative explanations ruled out

**Storage:**
```json
{
  "proof_id": "prf-l4-[uuid]",
  "level": "L4",
  "effect_ref": "prf-l3-[id]",
  "target_metric": "metric_name",
  "before_value": value,
  "after_value": value,
  "delta": value,
  "delta_percent": float,
  "significance": float(p-value),
  "observation_window_days": int,
  "attribution": "direct|assisted|organic|paid",
  "alternative_explanations": ["list"],
  "confidence": float(0.3-0.9)
}
```

## Evidence Chain

Proof must form a chain from L1 to L4:

```
Action (L1) → Execution (L2) → Effect (L3) → Outcome (L4)
```

Each level references the previous level:
- L2 references L1 action
- L3 references L2 execution
- L4 references L3 effect

Evidence chains enable:
1. Tracing outcome back to specific actions
2. Identifying where breakdowns occur
3. Validating action effectiveness
4. Building verified learning

## Evidence Storage

### Local Storage
- JSON files in `avos-core/proof-system/records/`
- Organized by level, date, target
- Versioned for audit trail

### Index
- Searchable index by target, level, date, status
- Allows querying: "all L4 proofs for target X in date range"
- Supports: "all actions that produced L4 outcomes"

### Retention
- L1: 90 days minimum
- L2: 180 days minimum
- L3: 365 days minimum
- L4: Permanent (verified outcomes)
- Archived after retention period

## Confidence Calculation

Confidence is weighted by:
- **Source reliability**: Platform API > Direct observation > Third-party report
- **Observation method**: Independent > Correlated > Self-reported
- **Consistency**: Multiple observations increase confidence
- **Duration**: Persistent effects increase confidence
- **Magnitude**: Significant changes increase confidence

## Verified Learning

Learning is stored only when:
1. Evidence chain is complete (L1 → L4)
2. Confidence exceeds threshold (configurable, default: 0.7)
3. Outcome is replicable (observed multiple times)
4. Alternative explanations are ruled out

### Learning Format
```json
{
  "learning_id": "learn-[uuid]",
  "source_proof": ["prf-l4-uuid"],
  "action_pattern": "pattern_description",
  "condition": "when X",
  "outcome": "Y occurs",
  "confidence": float,
  "scope": "domains|territories|keywords|all",
  "applicable_targets": ["list"],
  "created": "ISO8601",
  "validated": "ISO8601",
  "validation_count": int,
  "transferable": bool,
  "status": "verified|provisional|rejected"
}
```

## Evidence Verification Process

1. **Collect** evidence at each level
2. **Validate** each piece independently
3. **Chain** evidence across levels
4. **Assess** confidence scores
5. **Store** verified evidence with metadata
6. **Review** evidence periodically for consistency
7. **Update** confidence as new evidence arrives

## Integration Points

- Called by: scheduler/execution, provider/registry
- Used by: C44 diagnosis, AE² prioritization, continuous-operation
- Outputs: Verified learning, exception reports, proof statistics

## Implementation Notes

- Always store raw evidence alongside derived proofs
- Never modify original evidence (immutable audit trail)
- Log all confidence calculations with reasoning
- Support evidence import from external sources
- Test evidence chain integrity regularly
