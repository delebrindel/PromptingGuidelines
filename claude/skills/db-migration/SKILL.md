---
name: db-migration
description: Write, test, and apply forward-only SQL migrations for MySQL or MariaDB apps. Use when adding or changing tables, columns, indexes, or data through a migration, or when applying migrations to a live or shared database.
---

# Database migration

Migrations run once, in name order, and each applied file is recorded in `schema_migrations`. Use the project's migration runner and its numbered file pattern (for example `NNN_snake_case.sql`).

## Writing

1. Number the file by reading the last file in the migrations folder and adding one. Never reuse or renumber.
2. Never edit a migration that has been applied anywhere. The runner stores only the file name, not a checksum, so an edit changes history silently. Add a new file instead.
3. Start with a one-line comment saying what changes and why.
4. For data changes, search the code for the literal values the migration changes (for example a category name or status string) and update them in the same change.
5. MySQL commits DDL as it runs, so a failed file can leave partial changes. Keep each file to one logical change.

## Testing

1. Run the project's database test suite if it has one. It should use a throwaway database, never the live one.
2. Test against a copy of the real data first. Restore a dump into a scratch database, point the app's database config at it for the run, run the migration runner, check the result, then restore the config.
3. Never print config files that hold credentials.

## Applying

1. Get explicit approval for the named target database.
2. Back up first, outside the repo:
   `mysqldump -u<user> -h<host> -P<port> <db> > <backup-folder>/<db>-<date>.sql`
   Confirm the file is non-empty and ends with the dump footer before continuing.
3. Run the migration runner with the PHP or runtime version the project requires. Confirm the database driver extension (for example `pdo_mysql`) is loaded first.
4. Check the record: `SELECT name, applied_at FROM schema_migrations ORDER BY name;` must list the new file.
5. Load the app and check the changed data.
6. Shared hosting: follow the app's deploy notes. Back up with mysqldump over SSH or phpMyAdmin, then run the migration from the home directory.

## On failure

- The runner stops at the failing file and exits with status 1.
- Read the error. Decide with the user whether to finish the failed file's remaining statements by hand or restore the backup.
- Only after the schema matches the file's intent, record the name in `schema_migrations` and re-run. Get approval for each step on a live database.

## Rules

- Never run against a live database without approval and a verified backup.
- Destructive statements (`DROP`, `TRUNCATE`, column removal, `DELETE` without `WHERE`) need explicit confirmation first.

## Checklist

- [ ] Next number, new file, applied migrations untouched
- [ ] Header comment states the change and reason
- [ ] Hard-coded values updated in code
- [ ] Tests run against a scratch database
- [ ] Backup verified before the live run
- [ ] `schema_migrations` checked after the run
