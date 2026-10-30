# Schema And Migrations

Read during steps 2 and 4 when the repo has a database, stored files with a format, or migration code. Goal: separate history that existing databases still need from support code that is obsolete.

## Inventory

- Migration tool and runner: Prisma, Drizzle, Knex, TypeORM, Sequelize, Alembic, Django, Rails, Flyway, Liquibase, EF Core, sqlx, goose, golang-migrate, Wrangler D1, or hand-rolled.
- Where migrations live, how they are ordered, and whether files carry checksums.
- Applied-state record: `_prisma_migrations`, `__drizzle_migrations`, `alembic_version`, `django_migrations`, `schema_migrations`, `flyway_schema_history`, `__EFMigrationsHistory`, `d1_migrations`, or the repo's own table or file.
- Schema source of truth: ORM models, SQL files, a schema dump, or the migrations themselves. Note drift between them.
- Who runs migrations and when: deploy pipeline, app startup, manual runbook, tests, installer or first-run on user machines.
- Which databases exist: production, staging, per-tenant, self-hosted customers, desktop or mobile user databases, local dev, test fixtures. The oldest database still in the field decides what history is needed.
- Seeds, fixtures, backfill scripts, data fixers, and one-shot jobs.

## Classes

Put each item in one class.

1. **History still needed.** A migration that any supported database may not have applied yet, or that a fresh database needs because no baseline exists. Keep it. Never edit an applied migration file; many tools checksum them.
2. **Squashable history.** Every supported database is past point N, and fresh databases can start from a baseline. This is an `M` finding: generate the baseline from the real schema, mark it applied on existing databases the way the tool expects, and compare a fresh build with an authorized representative current schema. If that evidence is unavailable, keep the decision pending.
3. **Obsolete support code.** One-shot backfills and data fixers already applied everywhere, dual-write or dual-read shims after a finished cutover, readers for old column or file formats, compatibility views, rollout flags for a finished migration, custom runner wrappers that duplicate the tool. Group `D` when no database can still hold the old shape and no schema change follows; otherwise `M`.
4. **Dead schema.** Tables, columns, indexes, enums, triggers, or views with no reader and no writer. Removal is always `M`.
5. **Drift.** ORM, migrations, and live schema disagree. Report it with the exact difference. Do not delete to hide it.

Down migrations: keep them when the runbook or tool uses them for rollback. If they are unused, record that, but do not strip them from applied files when the tool checksums them.

## Database Evidence Bar

Code search alone is not enough for schema. Check:

- raw SQL strings, query builders, ORM relations and lazy loads, migrations that read the object, views, triggers, stored procedures, and database functions
- other services, workers, analytics, ETL, BI, exports, backups, and admin tools that share the database
- applied-state on each environment, and a row count or format check on the data the old code handles

Without access to applied-state or data, confidence is at most medium. Name the read-only query the owner must run and what result allows the change. Never connect to a production database or run queries against it without explicit authority. Read-only queries also need consent.

## Risks To Name

- data loss or irreversible drops
- table locks and downtime on large tables
- deploy order between code and migration, and fleets running mixed versions
- rollback path, and whether the drop is reversible from a backup
- checksum failures after edits to applied files
- CI or tests that build the database from scratch and would break on a squash

## Plan Shape For M Findings

Use a staged transition when existing data or clients need it. Establish the old and new contracts, supported versions, backups, and rollback first. Then, as applicable:

1. Prepare the new shape without breaking existing readers and writers.
2. Migrate or backfill data and move consumers in an order that preserves consistency. Verify each step against the actual deployment and concurrency model.
3. Confirm that supported clients and data no longer require the old shape. Wait any agreed period.
4. After explicit authorization and a verified recovery path, remove the old shape in a forward migration and retire only the support code no longer needed.

Each step names its files, deploy order, checks, rollback, and owner sign-off. Adapt the order to the actual system; do not stop old writes before the replacement is ready. See the [Prisma expand-and-contract example](https://docs.prisma.io/docs/guides/database/data-migration) for one implementation.
