---
name: db-migration
description: Write, test, and apply forward-only SQL migrations for MySQL or MariaDB apps such as Spento. Use when adding or changing tables, columns, indexes, or data through a migration, or when applying migrations to a live or shared database.
---

# Database migration

Migrations run once, in name order, and each applied file is recorded in `schema_migrations`. Spento's runner is `scripts/migrate.php`, and its files are `app/migrations/NNN_snake_case.sql`.

## Writing

1. Number the file by reading the last file in `app/migrations/` and adding one. Never reuse or renumber.
2. Never edit a migration that has been applied anywhere. The runner stores only the file name, not a checksum, so an edit changes history silently. Add a new file instead.
3. Start with a one-line comment saying what changes and why. Migration `011` is the example.
4. For data changes, search the code for the literal values the migration changes (for example `Otros`) and update them in the same change.
5. MySQL commits DDL as it runs, so a failed file can leave partial changes. Keep each file to one logical change.

## Testing

1. Run the suite that covers the change. For Spento: `npm run test:db`. It creates the throwaway `spento_test` database and needs local MySQL with a `config.php` user allowed to create databases.
2. Test against a copy of the real data first. Restore a dump into a scratch database, point `config.php` at it for the run, run `php scripts/migrate.php`, check the result, then restore `config.php`.
3. Never print `config.php`. It holds credentials.

## Applying

1. Get explicit approval for the named target database.
2. Back up first, outside the repo:
   `C:/laragon/bin/mysql/mysql-8.4.3-winx64/bin/mysqldump.exe -uroot -h127.0.0.1 -P3306 <db> > D:/backups/<db>-<date>.sql`
   Confirm the file is non-empty and ends with the dump footer before continuing.
3. Run `php scripts/migrate.php` with PHP 8.3: `C:/laragon/bin/php/php-8.3.36-Win32-vs16-x64/php.exe`. PHP 8.5 lacks `pdo_mysql` here.
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
