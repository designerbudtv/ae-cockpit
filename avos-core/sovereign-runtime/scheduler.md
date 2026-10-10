# Sovereign Runtime Scheduler

## Purpose

Persistent, event-driven scheduler for AVOS Core continuous autonomous operation, independent of Base44 runtime dependencies.

## Architecture

```
scheduler/
├── scheduler.md         # This file
├── triggers/            # Event trigger definitions
│   ├── time-based.md    # Cron/scheduled triggers
│   ├── event-based.md   # External event triggers
│   ├── health.md        # Health check triggers
│   └── threshold.md     # Metric threshold triggers
├── execution/           # Execution engine
│   ├── queue.md         # Task queue management
│   ├── worker.md        # Worker process management
│   └── concurrency.md   # Concurrent execution control
├── governance/          # Rate limiting and budgeting
│   ├── rate-limits.md   # Per-provider rate limits
│   ├── budgets.md       # Budget enforcement
│   └── priorities.md    # Priority-based scheduling
└── durable-execution/   # Durable execution framework
    ├── state.md         # Execution state tracking
    ├── recovery.md      # Failure recovery
    └── compensation.md  # Compensation actions
```

## Event Triggers

### Time-Based Triggers

```json
{
  "trigger_id": "trig-time-[uuid]",
  "type": "cron|interval|once",
  "schedule": "0 0 * * *",
  "timezone": "UTC",
  "action": "action_name",
  "target_filter": {
    "categories": ["domains", "pages"],
    "states": ["active", "monitoring"]
  },
  "max_instances": 1,
  "timeout_seconds": 300,
  "next_run": "ISO8601",
  "last_run": "ISO8601",
  "run_count": int,
  "enabled": true
}
```

### Event-Based Triggers

```json
{
  "trigger_id": "trig-event-[uuid]",
  "type": "webhook|callback|external",
  "source": "provider_name",
  "event_type": "url_changed|new_url|error|threshold",
  "conditions": {
    "field": "operator": "value"
  },
  "action": "action_name",
  "enabled": true
}
```

### Threshold Triggers

```json
{
  "trigger_id": "trig-threshold-[uuid]",
  "type": "metric_threshold",
  "metric": "error_rate|success_rate|response_time",
  "operator": "greater_than|less_than|equal",
  "threshold": float,
  "window_minutes": int,
  "action": "alert|suspend|escalate",
  "enabled": true
}
```

### Health Check Triggers

```json
{
  "trigger_id": "trig-health-[uuid]",
  "type": "health_check",
  "target": "provider_name|service_name",
  "interval_seconds": int,
  "timeout_seconds": int,
  "actions": {
    "healthy": "continue",
    "degraded": "alert",
    "unhealthy": "failover",
    "offline": "escalate"
  }
}
```

## Task Queue

### Queue Structure

```
Priority Queue:
├── Critical (P0): System failures, proof validation
├── High (P1): GGE state transitions, target monitoring
├── Medium (P2): Routine inspections, data collection
├── Low (P3): Reporting, maintenance, cleanup
└── Deferred (P4): Non-critical async tasks
```

### Task Format

```json
{
  "task_id": "task-[uuid]",
  "priority": "P0|P1|P2|P3|P4",
  "action": "action_name",
  "target_id": "tid-hash",
  "payload": {
    "parameters": {...},
    "config": {...}
  },
  "provider": "provider_name",
  "timeout_seconds": int,
  "max_retries": int,
  "scheduled_at": "ISO8601",
  "due_at": "ISO8601",
  "status": "pending|running|completed|failed|cancelled",
  "created_at": "ISO8601",
  "started_at": "ISO8601",
  "completed_at": "ISO8601",
  "attempts": [
    {
      "attempt": int,
      "status": "success|failed",
      "started_at": "ISO8601",
      "completed_at": "ISO8601",
      "error": "Error description"
    }
  ],
  "result": {
    "data": {...},
    "artifacts": ["artifact_refs"]
  }
}
```

### Queue Operations

1. **Enqueue**: Add task with priority and scheduling
2. **Dequeue**: Get next task respecting priority and due dates
3. **Update**: Modify task status/priority
4. **Retry**: Re-enqueue failed task with backoff
5. **Cancel**: Stop pending task
6. **Purge**: Remove completed/failed tasks after retention period

## Worker Management

### Worker Pool

```
Workers:
├── Crawl Workers: URL monitoring, content extraction
├── Measure Workers: SERP tracking, analytics collection
├── Process Workers: Data processing, analysis
├── Dispatch Workers: Action dispatch, provider calls
└── Maintenance Workers: Cleanup, verification, reconciliation
```

### Worker Configuration

```json
{
  "worker_id": "worker-[type]-[instance]",
  "type": "crawl|measure|process|dispatch|maintenance",
  "max_concurrent": int,
  "queue": "P0|P1|P2|P3|P4",
  "health_check_interval": int,
  "max_execution_time": int,
  "graceful_shutdown": bool,
  "metrics": {
    "tasks_processed": int,
    "tasks_failed": int,
    "avg_execution_time": float,
    "uptime_hours": float
  }
}
```

## Rate Limiting

### Per-Provider Limits

```json
{
  "rate_limit_id": "rl-[provider]",
  "provider": "provider_name",
  "requests_per_minute": int,
  "requests_per_hour": int,
  "concurrent_requests": int,
  "burst_size": int,
  "enforcement": "strict|graceful",
  "backoff_strategy": "exponential|linear|fixed",
  "state": {
    "current_rpm": int,
    "current_rph": int,
    "window_start": "ISO8601"
  }
}
```

### Budget Enforcement

```json
{
  "budget_id": "budget-[month]",
  "period": "monthly|weekly|daily",
  "total_budget": float,
  "spent": float,
  "currency": "USD",
  "alerts": {
    "at_50_pct": float,
    "at_80_pct": float,
    "at_95_pct": float
  },
  "actions": {
    "at_100_pct": "defer_low_priority",
    "over_budget": "suspend_non_critical"
  }
}
```

## Durable Execution

### State Tracking

```json
{
  "execution_id": "exec-[uuid]",
  "steps": [
    {
      "step": int,
      "name": "step_name",
      "input": {...},
      "output": {...},
      "status": "pending|running|completed|failed",
      "started_at": "ISO8601",
      "completed_at": "ISO8601"
    }
  ],
  "compensation_steps": [
    {
      "step": int,
      "name": "compensation_step",
      "description": "What to undo if step fails"
    }
  ],
  "checkpoint": {
    "last_completed_step": int,
    "checkpoint_data": {...}
  }
}
```

### Recovery Protocol

1. Detect failure (timeout, error, crash)
2. Restore state from checkpoint
3. Run compensation for failed steps
4. Resume from last successful step
5. If compensation fails, escalate

## Continuous Operation

### Core Loop

```
While running:
  1. Load next trigger or scheduled task
  2. Validate trigger conditions
  3. Check rate limits and budgets
  4. Create execution task
  5. Enqueue with priority
  6. Workers dequeue and execute
  7. Process results
  8. Store evidence
  9. Trigger downstream events
  10. Update scheduler state
  11. Sleep until next trigger
```

### 48-Hour Reconciliation

Every 48 hours:

```
1. Load all governed URLs
2. Verify identity completeness
3. Verify profile completeness
4. Check ALIVE readiness
5. Verify semantic territory
6. Check GGE states
7. Verify proof levels
8. Check provider health
9. Reconcile state consistency
10. Generate reconciliation report
11. Queue recovery tasks for gaps
```

## Provider Integration

### Provider Invocation

```
InvokeProvider(provider, action, params):
  1. Check provider health
  2. Check rate limits
  3. Check budget
  4. Create execution manifest
  5. Send request to provider
  6. Wait for response (with timeout)
  7. Parse response
  8. Validate response
  9. Store evidence
  10. Log execution
  11. Handle errors (retry/fallback)
```

## API Contract

### Internal
```
Scheduler
├── schedule(cron, action, target) → Trigger
├── unschedule(trigger_id) → void
├── enqueue(priority, action, payload) → Task
├── dequeue() → Task | null
├── complete(task_id, result) → void
├── fail(task_id, error) → void
├── retry(task_id, max_attempts) → void
├── getTrigger(trigger_id) → Trigger
├── getTasks(filters) → [Task]
├── getMetrics() → Metrics
├── getHealth() → HealthStatus
└── reconcile() → Report
```

### REST
```
GET    /api/v1/scheduler/triggers        # List triggers
GET    /api/v1/scheduler/triggers/{id}   # Get trigger
POST   /api/v1/scheduler/triggers        # Create trigger
DELETE /api/v1/scheduler/triggers/{id}   # Delete trigger
GET    /api/v1/scheduler/tasks           # List tasks
GET    /api/v1/scheduler/tasks/{id}      # Get task
POST   /api/v1/scheduler/tasks           # Enqueue task
GET    /api/v1/scheduler/metrics         # Get metrics
GET    /api/v1/scheduler/health          # Health check
POST   /api/v1/scheduler/reconcile       # Run reconciliation
```

## Implementation Notes

- Scheduler must be deterministic (same input = same order)
- All operations must be logged with timestamps
- State must be persisted (not just in-memory)
- Handle shutdown gracefully (drain queue, save state)
- Monitor worker health and restart if needed
- Support hot-reconfiguration of triggers
- Never block main loop on I/O operations
