---
name: supabase-best-practices
description: Use for Supabase and Postgres schema design, normalization, migrations, supabase db workflows, local reset/init, and database changes that must stay reproducible and migration-driven.
---

# Supabase Best Practices

Use this skill when work touches a Supabase-backed database schema, migration flow, or local database bootstrap path. This skill is intentionally opinionated: prefer sound relational design, require migrations for schema changes, and keep local environments reproducible from committed migrations and seeds.

## When To Use

- When adding or changing Supabase or Postgres tables, columns, constraints, indexes, views, functions, triggers, or policies
- When reviewing schema quality, data modeling, or normalization
- When editing `supabase/migrations`, `supabase/seed.sql`, or local bootstrap scripts
- When the user mentions Supabase, `supabase db`, migrations, reset/init flows, or database changes
- When a feature requires new relational data and there is a risk of denormalized design

If the task is only reading data or making a pure application-layer change with no database impact, this skill is usually not needed.

## Core Rules

1. **Design the schema like Postgres, not like a spreadsheet.**
Prefer proper relations, keys, constraints, and join tables over duplicated or loosely structured data. Supabase is built on Postgres; treat Postgres capabilities as the default foundation.

2. **Target BCNF first. Fall back to 3NF only with a reason.**
Push for Boyce-Codd Normal Form whenever practical. If BCNF would create unacceptable complexity, document why and ensure the design is still at least Third Normal Form. Do not introduce transitive dependencies or partial dependencies casually.

3. **Do not denormalize by default.**
Avoid convenience columns that duplicate canonical data. Only keep derived or duplicated values when they are clearly justified by performance, operational simplicity, or external integration requirements, and make the source of truth explicit.

4. **Use database constraints to protect invariants.**
Prefer foreign keys, unique constraints, check constraints, exclusion constraints when appropriate, and explicit nullability rules over relying on application code alone.

5. **Prefer indexes and query tuning before denormalization.**
If reads are slow, first ask whether the schema is normalized correctly, whether the query shape is sound, and whether the right indexes exist. The database is usually better at optimizing normalized data plus good indexes than an application is at maintaining duplicated state. Only fall back to denormalization after indexes, constraints, query shape, and execution plans have been reviewed.

6. **Push work into the database when it keeps the design simple.**
Use Postgres features to do as much work as possible without making the system overly complex: filtering, joining, aggregation, constraints, generated values, and well-scoped views are usually better handled in the database. Do not move logic into the application prematurely just because SQL feels less familiar.

7. **Manually optimize queries when the default plan is not good enough.**
Trust the database optimizer by default, but do not hesitate to improve query performance deliberately when evidence shows the current shape is suboptimal. Refine indexes, predicates, joins, and query structure when you know the system is leaving performance on the table.

8. **Every schema change needs a migration.**
Do not treat the Supabase dashboard or ad hoc SQL as the source of truth. If the schema changes, generate and commit a migration. The migration must land in the same change as the code that depends on it.

9. **Init and reset must reproduce the schema automatically.**
A fresh local setup must reach the expected schema by running committed migrations and seed data through the documented bootstrap path. If init or reset skips migrations, fix that before considering the work complete.

10. **Keep security in view, but keep this skill scoped.**
When schema changes affect access boundaries, check the relevant RLS policies and grants. Do not expand the task into a full auth or storage redesign unless the work actually requires it.

## Required Workflow

1. **Model the data before writing SQL.**
Identify entities, candidate keys, relationships, cardinality, and invariants. Check whether the proposed structure introduces repeated groups, partial dependencies, or transitive dependencies. Default to normalized tables plus join tables for many-to-many relations.

2. **Pressure-test normalization.**
Ask whether each non-key attribute depends on the key, the whole key, and nothing but the key. If not, split the table. Prefer BCNF. If you intentionally stop at 3NF, note the tradeoff clearly in your reasoning or change summary.

3. **Define integrity rules in the database.**
Add the foreign keys, uniqueness, checks, defaults, and index support the model needs. Do not leave relational integrity implied.

4. **Tune for performance with indexes before changing the model.**
When performance matters, inspect the query shape and add or refine indexes before reaching for duplicated columns or other schema shortcuts. Prefer improving access paths and predicates over weakening the data model.

5. **Let the database do the work it is good at.**
Push joins, filtering, aggregation, and data integrity into Postgres where that keeps the system understandable. Avoid moving database work into application code unless that actually simplifies the architecture or measurably improves performance.

6. **Generate a migration for every schema-affecting change.**
Use the project’s normal Supabase migration workflow so the resulting SQL is committed and reviewable. Do not leave schema drift trapped in a local database or the dashboard.

7. **Make local bootstrap deterministic.**
Verify the documented local setup path applies migrations on init or reset. In Supabase projects, prefer workflows built around repeatable commands such as `supabase db reset` and committed seed data over one-off manual repair steps.

8. **Keep migrations safe and reviewable.**
Write migrations that are explicit about data movement, backfills, destructive steps, and constraint changes. If a migration is risky, stage it in a sequence that preserves correctness and rollback clarity.

9. **Use manual query optimization when evidence justifies it.**
If a critical query still underperforms after basic indexing and sound schema design, optimize it deliberately. Review execution plans, adjust indexes, and rewrite the query shape where needed instead of assuming the default path is good enough.

10. **Check adjacent policies only when schema changes demand it.**
If tables, ownership paths, or access boundaries changed, verify the related RLS policies, grants, and dependent database objects still line up with the new schema.

## Verification Checklist

- Confirm the resulting schema is in BCNF, or document why 3NF is the deliberate stopping point
- Confirm canonical data lives in one place and duplicated columns are justified
- Confirm foreign keys, unique constraints, checks, and indexes reflect the intended invariants
- Confirm slow paths were evaluated for index and query-shape improvements before denormalization
- Confirm database-side filtering, joining, and aggregation are being used where they simplify the system
- Confirm every schema change has a committed migration
- Confirm a clean local init or reset applies migrations automatically
- Confirm seed data or bootstrap docs still work after the schema change
- Confirm related RLS or grants were reviewed when access boundaries changed

## Common Anti-Patterns To Avoid

- Adding a column that copies data from another table just to simplify reads
- Storing comma-separated lists or JSON blobs where a relational table should exist
- Reaching for denormalization before checking indexes or query plans
- Moving relational work into application code when Postgres can handle it more simply
- Relying on dashboard edits without generating migrations
- Editing the local database until it works, then forgetting to encode the change in migrations
- Shipping application code that assumes a schema change before the migration exists
- Leaving local setup dependent on undocumented manual SQL
- Treating RLS breakage as someone else’s problem after changing table structure

## Outcome

By the time you finish using this skill, the project should have:

- A schema design that is normalized and defensible
- Committed migrations for every schema-affecting change
- A documented local init/reset flow that reproduces the schema automatically
- Clear verification that the database structure, bootstrap path, and nearby policies still work together
