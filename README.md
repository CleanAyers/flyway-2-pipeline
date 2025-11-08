# flyway-2-pipeline - **CHILD REPOSITORY**

> **🚫 DO NOT EDIT `ro-shared-ddl/` FOLDER** - These files are managed by the parent repo

## 🚀 Quick Start

### First Time Setup
1. **Clone this repository**
2. **Set up Git aliases**:
   ```bash
   git config alias.syncshared '!git fetch parent-shared ro-shared-ddl && git subtree pull --prefix=ro-shared-ddl parent-shared ro-shared-ddl --squash && git add -A && git commit -m "chore(shared): sync ro-shared-ddl" || true && git push'
   ```
3. **Add parent remote**:
   ```bash
   git remote add parent-shared https://github.com/CleanAyers/shared-flyway-ddl.git
   ```
4. **Install Git protection hooks**:
   ```bash
   ./ro-shared-ddl/sh/setup_git_hooks.sh
   ```

### Daily Workflow
- **Sync shared files**: Run `git syncshared` to pull latest from parent
- **Edit child-specific files**: Only edit files outside of `ro-shared-ddl/`

## Repository Structure
```
flyway-2-pipeline/
├── README.md                     # This file
├── ro-shared-ddl/               # 🚫 DO NOT EDIT - Managed by parent
│   ├── .DO_NOT_EDIT_MANAGED_BY_PARENT
│   ├── sql/                     # Shared SQL migrations
│   └── sh/                      # Shared scripts
└── [your child-specific files]  # ✅ Edit these freely
```

## Important Rules
- **NEVER** edit files in the `ro-shared-ddl/` folder directly
- **ALWAYS** make shared changes in the parent repo's `shared/` folder
- Git hooks will warn you if you try to commit changes to managed files

# 🏗️ Cluster 2 – Flyway Pipeline

## Overview
This repository manages all **schema-level DDL migrations** for Cluster 2.  
It is part of the distributed Flyway structure defined in the [shared-flyway-ddl](https://github.com/CleanAyers/shared-flyway-ddl) parent repository.

**Purpose:**  
- Versioned SQL migrations `V__` prefix  
- Structural changes to tables, views, functions, and indexes  
- Separate lifecycle from `flyway-2-grants` (access control repository)  

---

## 📂 Structure
```
├── flyway-2-pipeline/                    # 🏗️ Cluster 2 schema migrations
│   ├── ro-shared-ddl/                    # Synced from parent
│   │   ├── sql/
│   │   │   └── V1__test.sql
│   │   └── sh/
│   │       └── child_pull_shared.sh
│   └── README.md
```

---

## 🚀 Usage
Run migrations locally or through CI/CD:

```bash
flyway -configFiles=conf/flyway.conf migrate
```

## 🧩 Notes

- All DDL scripts must be idempotent where feasible.
- Do not include grants or permissions here — those belong to flyway-2-grants.
- Sync schema baselines periodically with shared-flyway-ddl/global/baseline/.