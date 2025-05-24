# OODBMS_CONVERTER

Bidirectional converter between Oracle Object‑Oriented types and Relational tables.

## About the project

This repository contains the source code, demo schema and documentation for my diploma thesis entitled **"Development of Algorithms for Conversion Between Object‑Oriented and Relational Database Models in Oracle"**.  
The main deliverable is a PL/SQL package **`OODBMS_CONVERTER`** that automates conversion in both directions, helping DBAs and developers to explore, migrate and refactor Oracle schemas.

## Features

- 🔄 **`type_to_table`** — create relational tables (optionally recursively) from an object‑type hierarchy.  
- 🔄 **`table_to_type`** — generate object types (with `REF` collections) from existing relational tables, supporting 1 : 1, 1 : N and M : N relationships.  
- 📜 **`get_type_ddl`** — return pure DDL for the relational table that corresponds to a given object type.  
- 📊 **`draw_hierarchy`** — ASCII visualization of object‑type inheritance trees.  
- 🔍 **`get_relations` / `get_relation_between_tables`** — analyse foreign‑key relationships inside a schema.  
- ⚡ Internal associative‑array cache and PL/SQL **`RESULT_CACHE`** for repeated calls.

## Repository layout

| Path | Purpose |
|------|---------|
| `header.txt` | Package specification (`CREATE PACKAGE OODBMS_CONVERTER …`). |
| `body.txt` | Package body with full implementation. |
| `schema.txt` | Demo relational schema used in the thesis. |
| `object.txt` | Sample object‑type hierarchy (`PERSON_T`, `EMPLOYEE_T`, `VIP_EMPLOYEE_T`). |
| `helper_query.txt` | Example scripts showing typical calls. |
| `ВКР_отчет_only.pdf` | Full thesis report (Russian). |

## Prerequisites

* Oracle Database 19c (works from 12c) with PL/SQL enabled  
* Privileges: `CREATE TYPE`, `CREATE TABLE`, `CREATE PROCEDURE`  
* SQL\*Plus, SQLcl or SQL Developer

## Installation

```bash
# Clone repository
$ git clone https://github.com/yourusername/oodbms_converter.git
$ cd oodbms_converter
```

```sql
-- As a schema owner:
@header.txt
@body.txt

-- (Optional) load demo schema
@schema.txt
```

## Usage

### Convert an object‑type hierarchy to tables

```sql
-- Create tables for PERSON_T and all its subtypes
EXEC OODBMS_CONVERTER.type_to_table('PERSON_T', 'PERSON_T_TAB', cascade => TRUE);
```

### Visualise a hierarchy

```sql
EXEC OODBMS_CONVERTER.draw_hierarchy('PERSON_T');
```

### Convert relational tables back to object types

```sql
-- Generate object types for USERS and all related tables
SELECT OODBMS_CONVERTER.table_to_type('USERS', cascade => TRUE) FROM DUAL;
```

More one‑liners can be found in **`helper_query.txt`**.

## Performance

Repeated calls to `table_to_type` and `type_to_table` are memoised with PL/SQL result caching and an in‑package associative‑array cache.  
See Chapter 6 of the thesis for benchmarks and profiling details.

## Roadmap

- [ ] Support partitioned tables  
- [ ] Automatic generation of constructors and MAP/ORDER methods  
- [ ] CLI wrapper in Python

## License

Distributed under the MIT License. See `LICENSE` for details.

## Author

**[Your Name]** — 2025 — Faculty of Computer Science, [University]
