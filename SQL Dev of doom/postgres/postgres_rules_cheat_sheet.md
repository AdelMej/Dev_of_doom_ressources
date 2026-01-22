# PostgreSQL RULE Cheat Sheet 🧠

## What is a RULE?
A **RULE** in PostgreSQL is part of the **query rewrite system**.
It rewrites SQL statements *before* planning and execution.

Think:
> "When someone runs THIS query, silently replace it with THAT query."

---

## When RULEs Run
- BEFORE planning
- BEFORE execution
- BEFORE triggers
- NOT row-by-row

They operate on the **query**, not on rows.

---

## Common Uses (Historically)
- Updatable views (Postgres auto-generates rules)
- Legacy systems
- Old replication hacks

Modern PostgreSQL discourages manual use.

---

## Basic Syntax

```sql
CREATE RULE rule_name AS
ON INSERT | UPDATE | DELETE
TO table_name
DO [ INSTEAD | ALSO ] <sql_statement>;
```

---

## Example: Prevent Deletes

```sql
CREATE RULE no_delete_users AS
ON DELETE TO app.users
DO INSTEAD NOTHING;
```

---

## INSTEAD vs ALSO

### INSTEAD
- Replaces the original query
- Original query is NOT executed

### ALSO
- Original query executes
- Additional query is added

⚠️ ALSO can cause duplicated writes.

---

## RULE vs TRIGGER

| Feature | RULE | TRIGGER |
|------|------|---------|
| Execution | Query rewrite | Runtime |
| Scope | Statement-level | Row / Statement |
| Order | Before planning | After planning |
| Predictability | ❌ Low | ✅ High |
| Recommended | ❌ No | ✅ Yes |

---

## Why RULEs Are Dangerous ☠️
- Hard to debug
- Non-obvious behavior
- Can rewrite one query into many
- Break assumptions about row counts
- Fire even when no rows match

Postgres docs literally warn about this.

---

## RULEs and Views
- PostgreSQL auto-creates RULEs for updatable views
- This is the **only sane modern use case**
- Do not delete view rules unless you know what you're doing

---

## How to List RULEs

```sql
SELECT *
FROM pg_rules
WHERE schemaname = 'app';
```

---

## When NOT to Use RULEs
❌ Business invariants  
❌ Validation  
❌ Concurrency control  
❌ Authorization  
❌ Auditing  

Use:
- Constraints
- Triggers
- RLS

---

## When RULEs Are Acceptable
✅ Auto-generated view rules  
✅ Legacy systems you cannot refactor  
✅ Read-only query rewriting (rare)

---

## Golden Rule (pun intended)
If you think you need a RULE, you probably want a TRIGGER.

---

## Recommended Alternatives
- CHECK / UNIQUE / FK constraints
- BEFORE triggers for invariants
- AFTER triggers for side effects
- RLS for authorization

---

## TL;DR
- RULE = query rewrite magic
- Powerful but confusing
- Avoid unless dealing with views
- Triggers are almost always better

---

Happy hacking 🧑‍💻
