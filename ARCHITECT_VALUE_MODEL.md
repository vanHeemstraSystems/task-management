# Architect Value Model – Task Management Layer

> *“Don’t start with the task — start with the problem it solves.”*
> This document extends the existing Strategic Task Processing Framework with the personal Business Model layer.

See parent: [`sprint-management / ARCHITECT_VALUE_MODEL.md`](https://github.com/vanHeemstraSystems/sprint-management/blob/main/ARCHITECT_VALUE_MODEL.md)
See also: [`strategic-planning-management / PERSONAL_STRATEGY.md`](https://github.com/vanHeemstraSystems/strategic-planning-management/blob/main/PERSONAL_STRATEGY.md)

-----

## Extension to the Strategic Task Processing Framework

The existing Five Pillars framework evaluates task value **for the client or project**. This document adds the **practitioner lens**: does this task serve your own Business Model as a software architect?

Both lenses are required. A task can score highly on the Five Pillars (great for the project) but still be `misaligned` for your practice (execution-only, no judgment involved). Catching this pattern early is what protects your practice over time.

-----

## Extended Task Intake: The Sixth Question

The existing four Strategic Task Intake questions become five:

```
1. Do I know which Core Pain Point this task solves today?
2. Do I know which Core Pain Point this task will solve tomorrow?
3. If I asked my team which Underlying Needs are priority, would they all give the same answer?
4. Does this task create new customers/value, or just maintain existing systems?

➕ 5. Am I being engaged as an architect (judgment) or a resource (execution)?
      If execution: is there a clear path back to architectural influence on this engagement?
      If no path: this task is a signal — the engagement may need reframing.
```

-----

## Extended Database Schema

Add one column to the existing `strategic_tasks` table:

```sql
-- Add to strategic_tasks (extends existing schema in README.md)
ALTER TABLE strategic_tasks
ADD COLUMN architect_value_type VARCHAR(50) 
    CHECK (architect_value_type IN (
        'judgment',        -- deciding/advising/reviewing trade-offs — core architect work
        'execution',       -- implementing a defined solution
        'knowledge-build', -- building own expertise capital
        'relationship',    -- strengthening a key stakeholder connection
        'admin',           -- necessary overhead
        'misaligned'       -- should be declined — flag for retrospective
    ));

-- Useful query: weekly practice health check
CREATE VIEW weekly_practice_health AS
SELECT 
    architect_value_type,
    COUNT(*) as task_count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 1) as percentage,
    AVG(strategic_total) as avg_strategic_score
FROM strategic_tasks
WHERE created_at >= CURRENT_DATE - INTERVAL '7 days'
GROUP BY architect_value_type
ORDER BY task_count DESC;

-- Index for practice health queries
CREATE INDEX idx_architect_value_type ON strategic_tasks(architect_value_type, created_at DESC);
```

-----

## Updated Strategic Task Evaluation Matrix

The existing matrix (0–25 strategic score) gains a second dimension:

```
Task: _________________________________

[ ] Core Pain Point    (0–5): ___
[ ] Underlying Need    (0–5): ___
[ ] Stakeholder Impact (0–5): ___
[ ] Context Clarity    (0–5): ___
[ ] Outcome Value      (0–5): ___

STRATEGIC SCORE: ___ / 25

architect_value_type:
  ( ) judgment        → core architect work — prioritise
  ( ) execution       → watch ratio; is this >25% of your week?
  ( ) knowledge-build → schedule deliberately
  ( ) relationship    → invest as needed
  ( ) admin           → minimise
  ( ) misaligned      → decline or flag

COMBINED DECISION:
  Score ≥ 20 + judgment      → Execute immediately
  Score ≥ 15 + judgment      → Schedule this sprint
  Score ≥ 15 + execution     → Accept with awareness — track ratio
  Score ≥ 15 + misaligned    → Decline regardless of score
  Score < 15 + any           → Defer, delegate, or delete
```

-----

## Weekly Practitioner Review

Add these questions to your existing daily/weekly/monthly review cycle:

### Daily (add to existing)

- What `architect_value_type` were most of today’s tasks?
- Did I create or update any Architecture Decision Record today?

### Weekly (add to existing)

- What was my `judgment` vs `execution` ratio this week?
  - Run: `SELECT * FROM weekly_practice_health;`
- Was I consulted before or after key decisions?
- Did I document reasoning (not just conclusions) in any ADR or note?
- Did I add anything to `note-management` from this week’s work?
- Did I accept any `misaligned` tasks? How did they get through the intake gate?

### Monthly (add to existing)

- Is the trend in my `judgment` percentage going up, flat, or down?
- Am I solving architectural problems or just maintaining systems?
- Would my stakeholders describe my value the way I would?
- Is my `PERSONAL_STRATEGY.md` still accurate, or does it need updating?

-----

## Weekly Practice Health — Target Ratios

|`architect_value_type`|Target        |
|----------------------|--------------|
|`judgment`            |> 50% of tasks|
|`knowledge-build`     |10–20%        |
|`execution`           |< 25%         |
|`admin`               |< 10%         |
|`misaligned`          |0%            |

**If `execution` exceeds 40% for two consecutive weeks:** this is a signal to reframe the engagement or renegotiate scope at the sprint or PIPE level.

-----

## Connection to note-management

Every task tagged `judgment` or `knowledge-build` that produces a meaningful insight should generate an entry in `note-management`. This creates a compounding knowledge asset — the record of *why* you made the decisions you made, which is the evidence of your architectural value over time.

**Trigger rule:** If you made a significant architectural trade-off decision, created an ADR, or learned something domain-relevant — add a note. Takes 5 minutes. Compounds indefinitely.

-----

## Links Across the Hierarchy

|Direction|Level            |Document                                                                                                                    |
|---------|-----------------|----------------------------------------------------------------------------------------------------------------------------|
|↑ Up     |Sprint Execution |[`ARCHITECT_VALUE_MODEL.md`](https://github.com/vanHeemstraSystems/sprint-management/blob/main/ARCHITECT_VALUE_MODEL.md)    |
|↑↑ Root  |Personal Strategy|[`PERSONAL_STRATEGY.md`](https://github.com/vanHeemstraSystems/strategic-planning-management/blob/main/PERSONAL_STRATEGY.md)|
|→ Lateral|Knowledge Base   |[`note-management`](https://github.com/vanHeemstraSystems/note-management)                                                  |
|→ Lateral|Backlog          |[`backlog-management`](https://github.com/vanHeemstraSystems/backlog-management)                                            |
