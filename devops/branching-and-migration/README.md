---
description: >-
  Manage parallel development, selective merges, controlled migrations, and
  targeted rollback across Rierino environments.
icon: code-pull-request
---

# Branching & Migration

This section covers how Rierino handles change isolation, release movement, and recovery. It lets teams develop in parallel on a shared environment, merge selected assets, move changes between environments, and restore earlier versions when needed.

### What this section covers

* [Managing Branches](managing-branches.md) explains Rierino’s branch model, branch-aware development, selective merges, and how branches are accessed from the UI and APIs.
* [Migrating Changes](migrating-changes.md) covers online migration, offline export and import, CI/CD-based promotion, and bulk database migration tradeoffs.
* [Rolling Back Changes](rolling-back-changes.md) explains how to restore selected assets to an earlier snapshot using history states.

### How the workflow fits together

1. Create a branch for isolated changes.
2. Build and test updated assets on that branch.
3. Merge selected branch assets into main.
4. Migrate approved changes to target environments.
5. Roll back specific assets if a release needs correction.

This flow supports smaller releases. It also avoids full-environment promotion when only a subset of assets is ready.

### Key ideas

#### Branches run on the same environment

Rierino branches do not require separate deployments. Users and API clients can switch branches directly on the shared environment.

This makes branch testing faster. It also reduces infrastructure overhead.

#### Merges and migrations are selective

You can choose specific assets instead of moving everything together. This is useful for micro-releases and staged rollout.

#### Rollback depends on historical snapshots

Rollback works by restoring the latest snapshot before a chosen date. This relies on history states being available for the relevant domains.

### Common use cases

* Build a new saga or query on a feature branch without affecting main.
* Test branch-specific UI and API behavior through the admin UI or headers.
* Merge only one API, query, or model from a larger branch.
* Export a release package when environments cannot connect directly.
* Revert a subset of released assets after a bad deployment.

### Start here

* Start with [Managing Branches](managing-branches.md) if you need parallel development.
* Go to [Migrating Changes](migrating-changes.md) when moving approved assets to test or production.
* Go to [Rolling Back Changes](rolling-back-changes.md) when you need targeted recovery after release.
