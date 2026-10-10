# AE² - Autonomous Estate Allocation Engine

## Purpose

AE² prioritizes estate-wide allocation based on:
- Target condition
- Intent value
- KH/GGE/search/human/commercial evidence
- Historical learning
- Upside potential
- Marginal value
- Cost
- Risk
- Proof deficit
- Meta Pain contribution

## Architecture

```
ae2/
├── ae2-allocation.md       # This file - Allocation engine
├── priority-rules.md       # Priority calculation rules
├── allocation.md           # Resource allocation logic
├── risk-evaluation.md      # Risk assessment framework
└── meta-pain.md            # Meta Pain evaluation
```

## Priority Calculation

### Formula

```
Priority Score = Σ(Weight × Factor)

Where Factors:
  - Target Condition (0.15): ALIVE score, readiness level
  - Intent Value (0.15): Commercial value, strategic importance
  - Evidence Strength (0.10): Available proof quality and quantity
  - Historical Learning (0.10): Verified past outcomes
  - Upside Potential (0.15): Expected improvement magnitude
  - Marginal Value (0.10): Value added over current state
  - Cost (0.05): Resources required (inverted - lower cost = higher priority)
  - Risk (0.05): Probability of negative outcome (inverted)
  - Proof Deficit (0.05): How much evidence is needed
  - Meta Pain (0.10): Does solving this address fundamental problems?
```

### Priority Levels

| Level | Score | Allocation |
|-------|-------|------------|
| P0 (Critical) | 0.90+ | Immediate, unlimited resources |
| P1 (High) | 0.70-0.89 | Same day, full resources |
| P2 (Medium) | 0.40-0.69 | Within 48 hours, standard resources |
| P3 (Low) | 0.20-0.39 | Within week, minimal resources |
| P4 (Deferred) | 0.00-0.19 | When resources allow, minimal resources |

## Target Condition Assessment

### ALIVE Score Components

1. **Technical Life** (0-100)
   - HTTP availability
   - Load performance
   - Mobile responsiveness
   - Security posture
   - Technical SEO health

2. **Discoverability** (0-100)
   - Search visibility
   - Indexing status
   - Crawl efficiency
   - Internal link equity
   - External backlinks

3. **Semantic Life** (0-100)
   - Keyword coverage
   - Content relevance
   - Topic authority
   - Entity alignment
   - Answer box presence

4. **Relational Life** (0-100)
   - Competitor positioning
   - Market share
   - Partnership strength
   - Brand recognition
   - Community engagement

5. **Geographic Life** (0-100)
   - Local visibility
   - Regional dominance
   - Multi-market presence
   - Geo-specific optimization

6. **Human Life** (0-100)
   - User engagement
   - Return rate
   - Session duration
   - Bounce rate
   - Social signals

7. **Economic Life** (0-100)
   - Revenue contribution
   - Lead generation
   - Conversion rate
   - Customer lifetime value
   - ROI

### Readiness States

- **FULLY_READY**: All criteria met, can act immediately
- **PARTIALLY_READY**: Some criteria met, limited action
- **NOT_READY**: Major gaps, capability building needed
- **UNKNOWN**: Insufficient data for assessment

## Intent Value Assessment

### Value Dimensions

1. **Commercial Value**
   - Direct revenue impact
   - Lead quality
   - Conversion probability
   - Customer acquisition cost

2. **Strategic Value**
   - Competitive positioning
   - Market expansion potential
   - Brand building
   - Authority establishment

3. **Portfolio Value**
   - Content cluster centrality
   - Internal linking importance
   - Traffic distribution balance
   - Category representation

### Scoring

Each dimension scored 0-100, weighted by estate goals.

## Evidence Assessment

### Evidence Quality Tiers

| Tier | Description | Trust Level |
|------|-------------|-------------|
| Tier 1 | Platform API verified | 0.95-1.0 |
| Tier 2 | Independent observation | 0.80-0.94 |
| Tier 3 | Correlated data | 0.60-0.79 |
| Tier 4 | Self-reported | 0.40-0.59 |
| Tier 5 | Anecdotal | 0.20-0.39 |

### Evidence Requirements by Action

- **P0 actions**: Minimum Tier 1-2 evidence, L3 proof required
- **P1 actions**: Minimum Tier 2 evidence, L2 proof preferred
- **P2 actions**: Minimum Tier 2-3 evidence
- **P3 actions**: Minimum Tier 3 evidence
- **P4 actions**: Minimum Tier 4 evidence

## Historical Learning

### Learning Sources

1. **Past Actions**: What worked and what didn't
2. **Verified Outcomes**: Confirmed L4 changes
3. **Failed Experiments**: Documented failures with causes
4. **Pattern Recognition**: Recurring successful patterns

### Learning Application

- Similar targets get priority boost if past success
- Failed patterns reduce priority until re-evaluated
- Seasonal patterns influence timing recommendations
- Competitive patterns inform attack/expand decisions

## Upside Potential

### Calculation

```
Upside = (Target Value - Current Value) × Probability of Improvement

Where:
  - Target Value: Estimated value after successful action
  - Current Value: Verified current value
  - Probability: Based on evidence strength and historical success
```

### Categories

- **HIGH**: 50%+ potential improvement
- **MEDIUM**: 10-50% potential improvement
- **LOW**: 5-10% potential improvement
- **MINIMAL**: &lt;5% potential improvement

## Marginal Value

### Concept

Marginal value measures the incremental value of this allocation over other allocations.

```
Marginal Value = (Value if allocated to this) - (Value if allocated to next best)
```

### Evaluation

- Positive marginal value = worth allocating here
- Negative marginal value = redirect to better use
- Zero marginal value = indifferent between options

## Cost Assessment

### Cost Components

1. **Direct Cost**
   - API credits
   - Tool subscriptions
   - Infrastructure

2. **Indirect Cost**
   - Engineering time
   - Analysis time
   - Verification time

3. **Opportunity Cost**
   - Value of alternative uses of resources
   - Time pressure on other projects

### Budget Constraints

- Monthly budget limits per provider
- Priority-based spending limits
- Risk-adjusted allocation caps

## Risk Assessment

### Risk Types

1. **Technical Risk**: Implementation failures
2. **Commercial Risk**: Negative revenue impact
3. **Reputational Risk**: Brand/damage potential
4. **Competitive Risk**: Alerting competitors
5. **Compliance Risk**: Policy violations

### Risk Calculation

```
Risk Score = Σ(Probability × Impact)

Risk Levels:
  - LOW: Score &lt; 0.2
  - MEDIUM: Score 0.2-0.5
  - HIGH: Score 0.5-0.8
  - CRITICAL: Score &gt; 0.8
```

### Risk Mitigation by Priority

- **P0**: Risk assessment mandatory, mitigation plan required
- **P1**: Risk assessment required, mitigation optional
- **P2**: Risk assessment recommended
- **P3**: Risk assessment discretionary
- **P4**: Risk assessment not required

## Proof Deficit

### Assessment

How much evidence is missing to confidently act?

| Deficit Level | Description | Action |
|---------------|-------------|--------|
| None | Full proof available | Act immediately |
| Low | Minor gaps | Act with monitoring |
| Medium | Significant gaps | Gather evidence first |
| High | Major gaps | Pause and investigate |
| Critical | No evidence | Cannot act |

### Evidence Gathering Priority

1. Missing L3 evidence (external effects)
2. Missing L4 evidence (outcomes)
3. Missing L2 evidence (execution)
4. Missing L1 evidence (actions)

## Meta Pain Evaluation

### Concept

Meta Pain identifies fundamental, systemic problems that, if solved, would improve the entire estate.

### Meta Pain Categories

1. **Technical Foundation**: Core infrastructure issues
2. **Data Integrity**: Evidence and record quality
3. **Identity Resolution**: TARGET_ID and canonical URL issues
4. **Coverage Gaps**: Important topics/territories missing
5. **Competition Disadvantage**: Systemic competitive weaknesses
6. **Operational Friction**: Process inefficiencies at scale

### Meta Pain Scoring

- **Solved**: Meta Pain addressed, no action needed
- **Partially Solved**: Some progress, continue work
- **Unsolved**: Major gap, high priority to address
- **Unknown**: Need investigation

## Allocation Decision Process

### Step 1: Score All Targets
Calculate priority score for every actionable target.

### Step 2: Filter by Constraints
Apply budget, provider availability, and risk filters.

### Step 3: Rank
Sort by priority score within each P-level.

### Step 4: Allocate Resources
Distribute resources based on allocation rules:

```
Allocation Rules:
  - P0: Allocate as needed, no budget limit
  - P1: Allocate full resources, within budget
  - P2: Allocate standard resources
  - P3: Allocate minimal resources
  - P4: Allocate only surplus resources
```

### Step 5: Dispatch
Send allocation decisions to scheduler and workforce.

### Step 6: Monitor
Track allocation effectiveness and adjust priorities.

## Allocation Formats

### Target Allocation Record

```json
{
  "allocation_id": "alloc-[uuid]",
  "timestamp": "ISO8601",
  "target_id": "tid-hash",
  "priority": "P0|P1|P2|P3|P4",
  "priority_score": float,
  "factors": {
    "target_condition": float,
    "intent_value": float,
    "evidence_strength": float,
    "historical_learning": float,
    "upside_potential": float,
    "marginal_value": float,
    "cost": float,
    "risk": float,
    "proof_deficit": float,
    "meta_pain": float
  },
  "resource_allocation": {
    "provider": "provider_name",
    "budget_limit": float,
    "max_requests": int,
    "timeline_days": int
  },
  "action": {
    "type": "action_type",
    "description": "What to do",
    "expected_outcome": "Expected result",
    "success_criteria": ["criteria_1"]
  },
  "risk_assessment": {
    "level": "LOW|MEDIUM|HIGH|CRITICAL",
    "score": float,
    "mitigation": "Mitigation plan"
  },
  "approval": {
    "required": bool,
    "approved_by": "approver|null",
    "approved_at": "ISO8601|null"
  },
  "status": "pending|allocated|active|completed|cancelled",
  "proof_required": ["proof_levels"]
}
```

## Continuous Adjustment

### Dynamic Re-prioritization

Priorities must be reassessed when:
- New evidence arrives
- Competitor actions detected
- Algorithm changes observed
- Budget changes occur
- Meta Pain status changes

### Adjustment Rules

1. **Always** re-score when L3+ evidence arrives
2. **Always** re-score when GGE state changes
3. **Re-score weekly** for active targets
4. **Re-score monthly** for all monitored targets

## Integration Points

- Called by: scheduler triggers, C44 diagnosis
- Used by: workforce dispatch, provider execution
- Inputs: Target conditions, GGE states, evidence levels
- Outputs: Allocation decisions, priority assignments

## Implementation Notes

- Always document priority calculations with reasoning
- Track priority changes over time
- Compare estimated vs actual value of allocations
- Update weights based on allocation effectiveness
- Never allocate based solely on intuition
- Always consider opportunity cost
- Update Meta Pain assessment regularly
