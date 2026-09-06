# Hospital-Database-Data-
This repository documents my SQL practice using the Hospital Database from sql-practice.com, a platform for sharpening real-world query skills. Rather than just solving problems in isolation, I wanted to capture the schema I was working against and turn it into something reproducible and shareable.

# SQL Practice — Hospital Database

This repository documents my SQL practice using the **Hospital Database** from [sql-practice.com](https://www.sql-practice.com), a platform for sharpening real-world query skills. Alongside solving practice problems on the site, I wanted to capture the schema I was working against and turn it into something reproducible and shareable — plus track the SQL fundamentals I've been building.

I've worked through questions across all three difficulty tiers on the platform — **Easy, Medium, and Hard** — covering everything from basic filtering to multi-table joins, subqueries, CTEs, and window functions.

![Schema overview](images/schema-overview-infographic.png)

## Overview

| Metric | This repo's sample data | Live sql-practice.com DB |
|---|---|---|
| Total Tables | 4 | 4 |
| Total Rows | 503 | 4,530+ |
| Total Columns | 17 | 17 |
| Date Range | 2018-01-01 to 2024-12-31 | 2000-01-01 to 2024-12-31 |

The database models a hospital system: patients get admitted, admissions are tied to attending doctors, and patients/doctors are linked to province lookup data.

> The live site's database has 4,530+ rows. The CSVs and `load_data.sql` in this repo are a **synthetic sample dataset** built from the same schema (`generate_data.py`, seeded for reproducibility) so the structure can be shared, loaded locally, and version-controlled.

## Entity Relationship Diagram

![Full ERD](images/full-erd.jpeg)

**Relationships:**
- `patients.province_id` → `province_names.province_id`
- `admissions.patient_id` → `patients.patient_id`
- `admissions.attending_doctor_id` → `doctors.doctor_id`

## Tables

### `patients` (150 rows in this sample; 2,000+ on the live site)

| Column | Type | Notes |
|---|---|---|
| patient_id | INT (PK) | |
| first_name | TEXT | |
| last_name | TEXT | |
| gender | CHAR(1) | |
| birth_date | DATE | |
| city | TEXT | |
| province_id | CHAR(2) (FK) | → province_names |
| allergies | TEXT | ~35% NULL |
| height | INT | ~10% NULL |
| weight | INT | ~10% NULL |

![Patients table](images/patients-table.png)

### `admissions` (300 rows in this sample; 1,500+ on the live site)

| Column | Type | Notes |
|---|---|---|
| admission_id | INT (PK) | |
| patient_id | INT (FK) | → patients |
| admission_date | DATE | |
| discharge_date | DATE | ~5% NULL |
| diagnosis | TEXT | ~2% NULL |
| attending_doctor_id | INT (FK) | → doctors |

![Admissions table](images/admissions-table.jpeg)

### `doctors` (40 rows in this sample; 800+ on the live site)

| Column | Type | Notes |
|---|---|---|
| doctor_id | INT (PK) | |
| first_name | TEXT | |
| last_name | TEXT | |
| specialty | TEXT | |

![Doctors table](images/doctors-table.jpeg)

### `province_names` (13 rows in this sample — all Canadian provinces/territories; 230+ on the live site)

| Column | Type | Notes |
|---|---|---|
| province_id | CHAR(2) (PK) | |
| province_name | TEXT | |

![Province names table](images/province-names-table.jpeg)

## Missing / Null Values

- `patients.allergies` — ~35% NULL
- `patients.height` — ~10% NULL
- `patients.weight` — ~10% NULL
- `admissions.discharge_date` — ~5% NULL
- `admissions.diagnosis` — ~2% NULL

*(Percentages are approximate.)*

## Files in this repo

| File | Description |
|---|---|
| `schema.sql` | `CREATE TABLE` statements for all 4 tables |
| `load_data.sql` | `INSERT` statements generated from the CSVs |
| `generate_data.py` | Script that generated the sample data (reproducible, `seed=42`) |
| `patients.csv`, `doctors.csv`, `admissions.csv`, `province_names.csv` | Sample data |
| `images/` | Screenshots from sql-practice.com documenting the schema |

## Usage

```bash
# regenerate the CSVs (optional — sample data is already committed)
pip install faker
python3 generate_data.py

# load into a database, e.g. SQLite
sqlite3 hospital.db < schema.sql
sqlite3 hospital.db < load_data.sql
```

## SQL Concepts Practiced

- SELECT, WHERE, LIKE
- ORDER BY, LIMIT
- GROUP BY, HAVING
- COUNT, SUM, AVG, MIN, MAX
- JOINS (INNER, LEFT, RIGHT)
- Subqueries
- CTEs (WITH clause)
- Window Functions
- Date Functions
- CASE, IFNULL, COALESCE

## Practice Coverage

I've completed questions across all three difficulty levels on sql-practice.com:

| Difficulty | Questions | Focus |
|---|---|---|
| Easy | 25+ completed | Basic queries, filtering, sorting |
| Medium | 25+ completed | Joins, aggregations, subqueries |
| Hard | 25+ completed | CTEs, window functions, complex logic |

## Why this repo

Since I couldn't redistribute the live site's dataset, I built the schema and a synthetic sample dataset that follows the exact same structure, types, and null-value patterns. That means anyone cloning this repo can spin up the same database locally in SQLite (or another engine), run the included schema and insert scripts, and start practicing immediately — without needing to make an account on the original site. It's meant less as a finished analysis and more as a working sandbox and a record of the SQL fundamentals I've been building.

## Source

- Practice platform: [sql-practice.com](https://www.sql-practice.com) — Hospital Database
- Great for mastering real-world SQL queries and interview preparation.
