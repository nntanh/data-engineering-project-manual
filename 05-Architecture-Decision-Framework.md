# Architecture Decision Framework

**Dùng file này để**: Quyết định technical choices một cách systematic

---

## 🏗️ Template: Khi bạn bất ngờ gặp quyết định

### Decision: "Nên dùng [Tech A] hay [Tech B]?"

**Step 1: Define Requirements**
```
Data volume: ________ records/day
Latency SLA: ________ (seconds)
Availability: ________ (uptime %)
Budget: $ ________ /month
Team: ________ người (em solo?)
Timeline: ________ weeks
```

**Step 2: Compare Options**
| Criteria | Tech A | Tech B | Winner |
|----------|--------|--------|--------|
| Throughput | X qps | Y qps | ? |
| Learning curve | Easy/Med/Hard | Easy/Med/Hard | ? |
| Cost | $ | $ | ? |
| Scalability | Local/Cloud/Both | Local/Cloud/Both | ? |
| Community | Big/Small | Big/Small | ? |

**Step 3: Decision**
- Choose: **[Tech]**
- Reason: [2-3 sentence justification]
- Alternative if fail: [Backup option]

---

## 📊 Real Examples: E-Commerce Project

### Decision 1: Ingestion Tool

**Question**: Python script directly vs Kafka queue?

```
Requirements:
- 1000 products/hour
- Processing can tolerate 30min delay
- Cost-sensitive (student budget)
- Solo dev

Option A: Direct Python → DB
├─ Pros: Simple, no extra infra, $0 cost
├─ Cons: No buffering, tight coupling, if processing slow, scraper blocks
└─ Cost: $0

Option B: Kafka queue
├─ Pros: Decoupling, can scale scraper/processor independently
├─ Cons: Extra component, learning curve, Docker container overhead
└─ Cost: $0 (local) + disk space (~1GB)

Decision: PYTHON DIRECT (Phase 1)
Reason: MVP speed matters more than decoupling
Next phase: Add Kafka when scraper/processor need different scales

Alternative if slow: Switch to Kafka by moving producer to separate container
```

### Decision 2: Storage - PostgreSQL vs S3 vs Both?

```
Requirements:
- 50K products (small)
- Need to query by category/price often (structured)
- Want historical data (growing)
- Cost-sensitive

Option A: PostgreSQL only
├─ Pros: ACID, SQL queries, easy joins
├─ Cons: Single failure point, scaling cost
└─ Cost: ~$0 local, $10-15 if AWS RDS

Option B: S3 only (data lake)
├─ Pros: Cheap ($0.02 per GB), infinite scale
├─ Cons: No SQL, slow queries without Athena ($5), semi-structured
└─ Cost: $0.50-2/month

Option C: PostgreSQL (hot) + S3 (archive)
├─ Pros: Query speed + scalability + cheap archive
├─ Cons: Complexity (sync data)
└─ Cost: $10-15/month

Decision: POSTGRESQL (Phase 1) + S3 (Phase 2)
Reason: SQL is critical for analytics queries. Add S3 later for backup/archive
Alternative: If data grows >100GB, move to Redshift + S3
```

### Decision 3: Processing - PySpark vs Pandas vs Polars?

```
Requirements:
- 50K → 1M products soon (scaling)
- Aggregations: count, avg, group-by
- Latency: Can tolerate 5min transform time
- RAM: ~8GB locally

Option A: Pandas (simple)
├─ Pros: Easy syntax, Python native, fast for <2GB data
├─ Cons: Single machine, crashes at 8GB+ data
└─ Time to MVP: 1 day

Option B: PySpark (distributed)
├─ Pros: Scales to petabytes, production-grade, industry standard
├─ Cons: More complex, overhead, slower for small data
└─ Time to MVP: 3 days (learning)

Option C: Polars (fast)
├─ Pros: Modern, faster than Pandas/Spark for medium data, Rust-based
├─ Cons: Newer (less community), not for >100GB
└─ Time to MVP: 2 days

Decision: PYSPARK (with Pandas in notebook for exploration)
Reason: Learned framework, scales to future 1M+ data, production story
Learning path: Pandas (1 day) → PySpark (2 days)
Alternative: Use Polars if data stays <10GB
```

### Decision 4: Visualization - Streamlit vs Metabase vs Custom?

```
Requirements:
- Demo dashboard for recruiter
- Interactive filters
- Real-time updates (or daily refresh ok?)
- Learning value (bonus)

Option A: Metabase
├─ Pros: Beautiful, professional-looking, zero code
├─ Cons: Limited customization, overkill for solo project
└─ Time: 2 hours (UI-only)

Option B: Streamlit
├─ Pros: Code-driven, customizable, quick iteration, show Python skills
├─ Cons: Simpler design (but good enough)
└─ Time: 1 day (learn + build)

Option C: Custom (React + D3)
├─ Pros: Wow factor, full control
├─ Cons: Massive time investment, not necessary
└─ Time: 2+ weeks

Decision: STREAMLIT (recommend) or METABASE (easier)
Reason: Streamlit balances simplicity + learning + customization
Story: "I used Streamlit to build interactive dashboard, learned React-like component thinking"
Alternative: Start Metabase (quick), upgrade to Streamlit later
```

---

## 🎯 Decision Tree: What to Pick

### "Do I need this component?"

```
Q1: Is data volume >1GB/day?
├─ YES → Streaming (Kafka)
└─ NO → File-based batching

Q2: Do I need real-time <1min latency?
├─ YES → Streaming + Kafka
└─ NO → Batch jobs (hourly/daily)

Q3: Is data structured (rows/columns)?
├─ YES → Relational DB (PostgreSQL)
└─ NO → Object storage (S3) + processing

Q4: Will I have non-technical users querying data?
├─ YES → Add BI tool (Metabase/Streamlit)
└─ NO → Just notebooks/API

Q5: Is this solo or team?
├─ SOLO → Prioritize learning over perfection
└─ TEAM → Prioritize docs + monitoring
```

---

## 🏆 "Good Enough" Tech Stack for Each Phase

### Phase 1: MVP (Weeks 1-4, $0)
```
Scraping:     Python + BeautifulSoup
Ingestion:    Direct Python writes (no queue yet)
Processing:   Pandas (single machine)
Storage:      PostgreSQL local
Visualization: Jupyter notebook (for exploration)
```

### Phase 2: Production-ish (Weeks 5-8, $0-20)
```
Scraping:     Python (same)
Ingestion:    Kafka (for decoupling)
Processing:   PySpark (for scale)
Storage:      PostgreSQL (same)
Visualization: Streamlit dashboard
Monitoring:   Basic logging to CloudWatch
```

### Phase 3: Cloud-ready (Weeks 9+, $10-15/month)
```
Scraping:     Lambda function (scheduled)
Ingestion:    Kinesis (AWS Kafka alternative) or SQS
Processing:   EMR (AWS Spark) or Glue
Storage:      RDS (managed DB) + S3 (data lake)
Visualization: Streamlit on EC2 or Lambda
Monitoring:   CloudWatch + SNS alerts
```

---

## 🚫 Anti-Patterns: What NOT to Do

### ❌ Anti-Pattern 1: "I'll use the best technology"
```
WRONG: "Let me use Kubernetes, gRPC, Cassandra, GraphQL"
RIGHT: "Let me use Docker, REST API, PostgreSQL, REST API"
Why: Over-engineering delays MVP. Start simple, add complexity when needed.
```

### ❌ Anti-Pattern 2: "I must handle 1B records"
```
WRONG: "Design for Netflix scale"
RIGHT: "Design for realistic scale (50K-1M), document how to scale"
Why: Premature optimization is evil. Solve today's problem.
```

### ❌ Anti-Pattern 3: "This component is perfect"
```
WRONG: Spend 2 weeks tuning Spark configs
RIGHT: Get MVP working in 2 days, optimize later
Why: 80/20 rule - get to MVP first.
```

### ❌ Anti-Pattern 4: "I'll learn [complex tech] first"
```
WRONG: Learn Kubernetes before building anything
RIGHT: Learn just enough (Docker basics) to unblock
Why: Learn by doing. Theory without practice is useless.
```

### ❌ Anti-Pattern 5: "I'll handle every edge case"
```
WRONG: Error handling, data validation for every scenario
RIGHT: Handle happy path first, add error handling when bugs appear
Why: YAGNI (You Aren't Gonna Need It). Focus on value first.
```

---

## 🧠 How to Think About Trade-offs

### Framework: Complexity vs Benefit

For each technology choice:
```
Benefit = Value gained (scale, correctness, learning, interview story)
Complexity = Time to learn + time to implement + time to maintain

If (Benefit / Complexity) < 1.5:
    Skip it (not worth)
Else:
    Consider it
```

### Examples:

**Kafka**
```
Benefit: Decoupling scraper from processor, future stream processing
Complexity: 3 days to learn + setup + debug
Ratio: Medium/high benefit vs High complexity
Decision: Add to Phase 2 (not MVP), after MVP proves concept
```

**Kubernetes**
```
Benefit: Production deployment, auto-scaling
Complexity: 2 weeks to learn properly
Ratio: High benefit vs VERY high complexity
Decision: Skip for solo student. Docker Compose is enough
```

**Great Expectations (data validation)**
```
Benefit: Catch data quality issues early
Complexity: 1 day to learn + implement
Ratio: Medium benefit vs Low-medium complexity
Decision: Add to Phase 2 after core pipeline works
```

**PySpark**
```
Benefit: Scales to 100GB+ data, production-grade, job interview story
Complexity: 3 days to learn
Ratio: High benefit vs Medium complexity
Decision: Worth it. Part of Phase 2
```

---

## 📋 Architecture Decision Template (Copy-Paste)

```markdown
# Decision: [Component/Tech]

## Context
- Data volume: ___
- Latency: ___
- Budget: ___
- Team: ___

## Options Considered
### Option A: [Tech A]
- Pros:
- Cons:
- Cost:
- Learning time:

### Option B: [Tech B]
- Pros:
- Cons:
- Cost:
- Learning time:

### Option C: [Tech C]
- Pros:
- Cons:
- Cost:
- Learning time:

## Decision
**Chosen**: [Tech]
**Reason**: [Why this is better given our constraints]
**Trade-off accepting**: [What we're giving up]
**Backup plan**: If this fails, we'll switch to [Alternative]
**Revisit date**: Phase [X] or when [Condition]

## How this was decided
- Consulted: [GitHub issues / StackOverflow / tutorials / senior engineer]
- Tested: [Did a quick proof of concept? Yes/No]
- Team agreed: Yes / No (solo so N/A)
```

---

## 🎯 Principles

### Principle 1: Choose Boring
> "Use the most boring, proven technology for the job."
> — Netflix engineering

It means: PostgreSQL over NoSQL, REST over gRPC, Docker over Kubernetes.

### Principle 2: Scale When Needed
> "Premature optimization is the root of all evil."
> — Donald Knuth

Start simple. Add complexity only when:
- Bottleneck is proven (with metrics)
- Cost of not optimizing > cost of optimization
- You understand the current system

### Principle 3: Minimize State
Reduce number of components. Each component is a failure point.

**Bad**: Scraper → Kafka → Spark → PostgreSQL → Cache → API → Dashboard (6 components)
**Good**: Scraper → PostgreSQL → Dashboard (3 components, still covers MVP)

### Principle 4: Document Decisions
Why? Future you won't remember "Why did we use Kafka?" 6 months from now.

Put in `ARCHITECTURE.md`:
```
## Why Kafka?
Initially didn't use Kafka (too complex). But in Phase 2, when scraper and processor needed different scales, Kafka decoupled them. Decision date: 2026-08-15
```

### Principle 5: Version Your Architecture
As you grow, architecture changes. Document:
```
## Architecture Versions
- v1 (Phase 1): Direct Python writes, single Pandas job
- v2 (Phase 2): Kafka + PySpark (current)
- v3 (Phase 3): AWS Lambda + EMR (planned)
```

---

## 🏁 Final Checklist: Before Picking a Tech

- [ ] Do I understand what problem this solves?
- [ ] Is there a simpler alternative?
- [ ] Can I learn this in <5 days?
- [ ] Will it help with interview story?
- [ ] What if I pick wrong? Can I switch?
- [ ] Does it fit my budget?
- [ ] Does the community exist (help available)?
- [ ] Does it scale from MVP to 10x current data?

If you answered "No" to any, reconsider.

---

**End of Architecture Framework**

Use this when:
- You're stuck on a choice
- You want to justify a decision
- Someone asks "Why did you pick X?"
- You're planning Phase 2-3
