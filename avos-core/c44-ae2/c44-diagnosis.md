# C44 Diagnosis Engine

## Purpose

C44 observes/interprets/diagnoses/forms hypotheses/beliefs/confidence/contradictions/recommendations/expected results/required capability/proof.

## Architecture

```
c44/
├── c44-diagnosis.md     # This file - Diagnosis engine
├── beliefs.md           # Belief formation and management
├── confidence.md        # Confidence scoring system
├── contradictions.md    # Contradiction detection
├── recommendations.md   # Recommendation generation
├── observations/        # Raw observation storage
├── hypotheses/          # Hypothesis formation
├── expected_results/    # Expected outcome tracking
└── proof_requirements/  # Required proof specification
```

## C44 Process

### Step 1: OBSERVE
Collect raw data from:
- SERP position changes
- Content changes
- Crawl behavior changes
- Indexing status
- Competitive movements
- Commercial signals

Observation format:
```json
{
  "observation_id": "obs-[uuid]",
  "timestamp": "ISO8601",
  "source": "provider/method",
  "target_id": "tid-hash",
  "metric": "position|indexing|content|crawl|commercial",
  "value": {...},
  "context": {
    "geo": "string",
    "device": "string",
    "time_range": "string"
  }
}
```

### Step 2: INTERPRET
Normalize observations into actionable intelligence:
- Compare with expected state
- Identify deviations
- Classify significance level
- Determine category of change

Interpretation format:
```json
{
  "interpretation_id": "interp-[uuid]",
  "observation_refs": ["obs-uuid-1"],
  "deviation_type": "improvement|decline|stable|unknown",
  "significance": "critical|high|medium|low|negligible",
  "category": "technical|content|competitor|algorithm|market",
  "summary": "Human-readable summary of interpretation"
}
```

### Step 3: DIAGNOSE
Root cause analysis:
- Correlate with known factors
- Check for algorithm updates
- Analyze competitor actions
- Review technical changes
- Consider external events

Diagnosis format:
```json
{
  "diagnosis_id": "diag-[uuid]",
  "interpretation_ref": "interp-uuid",
  "hypotheses": [
    {
      "hypothesis_id": "hyp-[uuid]",
      "cause": "explanation",
      "confidence": float(0-1),
      "evidence": ["obs-uuid"],
      "falsifiable": bool
    }
  ],
  "root_cause": "explanation_or_unknown",
  "contributing_factors": ["factor_1", "factor_2"],
  "severity": "critical|high|medium|low"
}
```

### Step 4: FORM HYPOTHESES
Generate testable predictions:
- If cause A, then outcome B
- If fix C, then improvement D
- If no action, then decline E

Hypothesis format:
```json
{
  "hypothesis_id": "hyp-[uuid]",
  "diagnosis_ref": "diag-uuid",
  "prediction": "If [action], then [result]",
  "conditions": ["condition_1"],
  "expected_outcome": {
    "metric": "string",
    "direction": "improve|decline|stable",
    "magnitude": float,
    "timeline": "days"
  },
  "test_method": "how_to_validate",
  "risk_level": "low|medium|high",
  "cost_estimate": float
}
```

## Belief System

### Belief Formation
Beliefs are formed from:
1. Repeated observations supporting a cause
2. Successful predictions from hypotheses
3. Expert analysis and patterns
4. Verified learning from past operations

### Belief Structure
```json
{
  "belief_id": "belief-[uuid]",
  "belief_statement": "What we believe to be true",
  "supporting_evidence": ["prf-l1-uuid", "prf-l3-uuid"],
  "confidence": float(0-1),
  "formed_date": "ISO8601",
  "last_verified": "ISO8601",
  "verification_count": int,
  "scope": ["applicable_to_1"],
  "falsified": bool,
  "falsification_date": "ISO8601|null",
  "status": "active|provisional|rejected"
}
```

## Confidence Scoring

### Confidence Factors
- **Sample size**: More observations = higher confidence
- **Consistency**: Repeated similar results = higher confidence
- **Method reliability**: Platform APIs > Direct observation
- **Temporal stability**: Persistent patterns = higher confidence
- **Cross-validation**: Multiple independent methods = higher confidence

### Confidence Calculation
```
confidence = weighted_sum(factors)
where factors:
  - sample_size_weight: 0.2
  - consistency_weight: 0.25
  - method_reliability_weight: 0.2
  - temporal_stability_weight: 0.15
  - cross_validation_weight: 0.2
```

## Contradiction Detection

### Detection Rules
- Two active beliefs with opposite predictions
- Recent observation contradicts established belief
- L4 outcome doesn't match L3 effect prediction
- Provider reports conflict with direct observation

### Contradiction Resolution
1. Log contradiction with evidence
2. Assess confidence of both sides
3. Seek additional evidence
4. Update beliefs or add nuance
5. Record resolution in learning system

## Recommendation Generation

### Decision Criteria
Recommendations based on:
- **Target condition**: Current GGE state and ALIVE score
- **Intent value**: Commercial and strategic importance
- **Evidence strength**: Available proof for action
- **Upside potential**: Expected improvement magnitude
- **Marginal value**: Value over current state
- **Cost**: Resources required
- **Risk**: Probability of negative outcome
- **Proof deficit**: What evidence is missing
- **Meta Pain**: Does this solve a fundamental problem?

### Recommendation Format
```json
{
  "recommendation_id": "rec-[uuid]",
  "diagnosis_ref": "diag-uuid",
  "action_type": "attack|expand|hold|defend|watch|redirect|deprioritize",
  "description": "Recommended action",
  "rationale": "Why this action",
  "priority": "P0|P1|P2|P3",
  "estimated_impact": float,
  "estimated_cost": float,
  "risk_level": "low|medium|high",
  "proof_required": ["proof_type_1"],
  "expected_timeline": "days",
  "success_metrics": ["metric_1"],
  "fallback_action": "what_if_this_fails",
  "approval_required": bool
}
```

## Expected Results

### Tracking
Each recommendation generates expected results:
- **Before state**: Baseline metrics
- **Expected after**: Predicted improvement
- **Timeframe**: When to observe
- **Success criteria**: What constitutes success
- **Failure criteria**: What constitutes failure

### Verification
1. Wait for observation window
2. Collect new observations
3. Compare with expected results
4. Calculate actual impact
5. Update confidence scores
6. Store verified learning

## Required Capability

### Capability Assessment
Before recommending action:
- **Capability exists**: Can we execute?
- **Capability ready**: Are providers available?
- **Budget available**: Are resources sufficient?
- **Permissions granted**: Are we authorized?
- **Knowledge sufficient**: Do we understand the cause?

### Capability Gap Handling
If capability gap exists:
1. Document gap
2. Assess if action can proceed anyway
3. If not, recommend capability building
4. Track gap until resolved

## Proof Requirements

### Proof Levels for Actions
- **L1 required**: Every action
- **L2 recommended**: Most actions
- **L3 required**: Significant actions
- **L4 preferred**: Learning-generating actions

### Proof Collection Strategy
- What evidence to collect before action
- What evidence to collect during action
- What evidence to collect after action
- How to store and validate evidence
- When to promote learning from evidence

## Integration Points

- Called by: scheduler triggers, continuous-operation
- Used by: AE² allocation, provider execution
- Outputs: Diagnoses, recommendations, hypotheses
- Inputs: Observations, GGE states, proof records

## Implementation Notes

- Always record raw observations before interpretation
- Track belief lifecycle (form → verify → reject → archive)
- Log all contradictions and their resolution
- Never promote without evidence at each level
- Update confidence scores with every new observation
- Test hypotheses and record actual vs expected results
