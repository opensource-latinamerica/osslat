---
title: "Zero Downtime Database Migrations with gh-ost: A Production Must-Have"
date: 2025-11-14T15:30:00-06:00
featured: true
draft: false
comment: false
toc: true
reward: false
pinned: false
carousel: true
categories:
  - blog
tags:
  - database
  - mysql
  - gh-ost
  - migrations
  - zero-downtime
authors:
  - oss.lat
images:
  - /images/blog/gh-ost.png
---

In today's always-on world, taking your production database offline for a schema change is often unacceptable. Even a few seconds of downtime can mean lost revenue, frustrated users, or broken services. This is especially true for large, heavily-trafficked MySQL databases.

Traditional `ALTER TABLE` operations in MySQL can be blocking, meaning they lock the table and prevent writes (and sometimes reads) for the duration of the operation. For big tables, this can take minutes or even hours.

Enter **`gh-ost`** (GitHub Online Schema Transit). Developed by GitHub, `gh-ost` is a battle-hardened, trigger-based online schema migration tool for MySQL. It allows you to perform complex schema changes on large tables with **absolutely zero downtime** for your application.

### How Does gh-ost Achieve Zero Downtime?

`gh-ost` works by creating a copy of your table, applying the schema changes to the copy, and then subtly synchronizing data before atomically swapping the tables. Here’s a simplified breakdown:

1.  **Create a "Ghost" Table:** `gh-ost` creates an empty *ghost table* with the new schema, identical to your original table but with the desired modifications.
2.  **Apply Triggers:** It then creates **triggers** on your *original table* for `INSERT`, `UPDATE`, and `DELETE` operations. These triggers capture changes made to the original table and apply them to the ghost table.
3.  **Copy Existing Data:** In the background, `gh-ost` copies all existing data from your original table to the ghost table in small, manageable chunks. This process is throttled to minimize impact on your database.
4.  **Continuous Synchronization:** As data is being copied, the triggers continue to forward any new changes from the original table to the ghost table, ensuring the ghost table stays up-to-date.
5.  **Cut-Over (Atomic Swap):** Once the ghost table is fully synchronized, `gh-ost` performs an **atomic swap** using a `RENAME TABLE` operation. This is an extremely fast, non-blocking operation that essentially renames your original table to a discarded name and renames the ghost table to your original table's name. Your application immediately starts using the new table.
6.  **Cleanup:** The old, original table (now renamed) can then be dropped.

### Key Benefits of gh-ost

* **Zero Downtime:** This is its primary and most significant benefit. Your application remains fully available throughout the migration.
* **Safety and Control:**
    * **Throttle Mechanisms:** `gh-ost` actively monitors your database load (replication lag, CPU usage, etc.) and throttles its operations if the database becomes too busy.
    * **Hooks:** You can define custom hooks (scripts) to be executed at various stages of the migration, allowing for advanced automation and validation.
    * **Graceful Cancellation:** You can safely cancel a migration at almost any point, reverting to the original table without data loss.
    * **Checksums & Verification:** It can perform data integrity checks.
* **Non-Blocking:** It avoids long-lasting table locks.
* **Auditable:** Provides detailed logs of its operations.
* **Battle-Tested:** Used by GitHub for years on some of the world's largest MySQL deployments.

### When to Use gh-ost

`gh-ost` is ideal for:
* Large production MySQL tables where `ALTER TABLE` would cause unacceptable downtime.
* Complex schema changes (adding columns, changing column types, adding/dropping indexes, etc.).
* Environments where continuous deployment and high availability are critical.

### Getting Started

`gh-ost` is an open-source tool, typically installed as a Go binary. You run it from a client machine (or a dedicated migration server) that has network access to your MySQL database.

```bash
# Example gh-ost command for adding a column
gh-ost \
  --host=your_db_host \
  --port=3306 \
  --user=your_user \
  --password=your_password \
  --database=your_database \
  --table=your_table \
  --alter="ADD COLUMN new_column VARCHAR(255) NOT NULL DEFAULT ''" \
  --execute
```