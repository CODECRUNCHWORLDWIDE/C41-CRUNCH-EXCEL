# Week 5 — Data Cleaning & Validation

> **Goal:** by Sunday you can take a raw, messy export — duplicate rows, stray whitespace, inconsistent category text, mixed date formats — and turn it into a tidy dataset with duplicates removed, categories standardized, input restricted with drop-downs, and outliers flagged in color. This is the skill that separates "I can write formulas" from "I can be handed a real file and trust the answer."

Welcome back to **C41 · Crunch Excel**. Weeks 1–4 assumed your data was already clean: typed correctly, one row per record, consistent categories. Real data is never like that. Every export from a CRM, every form response, every CSV a coworker emails you arrives with duplicate rows, `"California"` next to `"CA"` next to `" ca "`, dates as text in three different formats, and blank rows nobody meant to leave. Before you can pivot it, chart it, or model it, you have to **clean it** — and you have to clean it in a way that's repeatable, not a one-off manual fix you can't explain to anyone later.

This week also flips your relationship with input. Everything through Week 4 was about *reading* data correctly. This week is about *guarding the door* — using Data Validation so bad values can't get typed in the first place, dependent drop-downs so a "Subcategory" list only shows options that make sense for the chosen "Category," and conditional formatting so anything that slips through anyway lights up in color instead of hiding silently in row 4,000.

You'll build and clean one running dataset all week: a signup/order export for a fictional outdoor-gear retailer, **Crunch Outfitters**. It starts genuinely messy — on purpose — and by Sunday you'll have turned it into something a pivot table (Week 7) can trust without a second thought.

## Learning objectives

By the end of this week, you will be able to:

- **Audit** a raw dataset systematically for duplicates, blank/whitespace-only rows, inconsistent categories, and type mismatches — and explain what "tidy data" means and why it matters before any analysis.
- **Remove duplicates** with the Remove Duplicates tool (and know its exact-match limits), and catch *near*-duplicates that differ only by case or whitespace.
- **Standardize** inconsistent category and unit values at scale using Find & Replace, `TRIM`/`CLEAN`/`PROPER`/`UPPER`/`LOWER`, and `SUBSTITUTE`.
- **Use Flash Fill** (Excel) and its Google Sheets equivalents to extract and reshape values from pattern examples, without writing a formula for simple cases.
- **Build Data Validation rules** that restrict what can be typed into a cell — number ranges, date ranges, text length, and custom formula rules.
- **Build dependent (cascading) drop-down lists**, where the options in one drop-down depend on what was picked in another.
- **Apply conditional formatting** to flag bad, duplicate, or outlier data visually, so violations are obvious at a glance instead of hidden in a 2,000-row sheet.
- **Design a tidy dataset** — one row per observation, one column per variable, atomic values — that's genuinely ready for the pivot tables and charts of Week 7.

## Prerequisites

- Weeks 1–4 complete: comfortable with formulas, absolute/relative references, `IF`/`IFS`, `XLOOKUP`/`INDEX`+`MATCH`, and basic text/date functions (`LEFT`/`RIGHT`/`MID`, `TEXT`, `DATE`).
- Excel (365 or Excel for the web) **and/or** Google Sheets. Data Validation and conditional formatting exist in both; this week calls out every place their menus or formula syntax diverge.
- No add-ins required. Everything this week ships in the base app.

## Set up your workspace (do this first)

Open a fresh blank workbook (Excel: `crunch-week5.xlsx`; Sheets: blank at <https://sheets.google.com>, rename to `crunch-week5`). Add four sheets, renamed exactly as below:

1. **`RawSignups`** — the messy raw export you'll audit and clean in Lecture 1 and Lecture 2. You'll type the raw data in yourself, exactly as given (including the messy whitespace/casing) — that's the point.
2. **`Lookups`** — the clean reference lists (states, membership tiers, categories/subcategories) that back this week's drop-downs, built in Lecture 3.
3. **`OrderEntry`** — a small guarded entry form you'll build in Lecture 3 and Exercise 3.
4. **`Scratch`** — a scratch area for testing formulas before you commit them to real data. Keep this habit all course long; never test a new formula directly on data you care about.

Each lecture tells you exactly what to type into its sheet. Sanity check: four renamed, empty tabs before starting Lecture 1.

## Weekly schedule

The schedule below adds up to approximately **28 hours** (the course's full-time pace). Treat it as a target, not a stopwatch.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|------------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Auditing a raw export; what "tidy data" means | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Remove Duplicates, Flash Fill, TRIM/CLEAN | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Wednesday | Data Validation, dependent drop-downs, flags | 2h | 1.5h | 1h | 0.5h | 1h | 0h | 6h |
| Thursday | Standardize categories; dedupe drills | 0h | 0h | 1h | 0.5h | 1h | 1h | 3.5h |
| Friday | Real CSV dump + guarded entry form challenges | 0h | 0h | 1h | 0.5h | 1h | 1.5h | 4h |
| Saturday | Mini-project (raw dump → clean, validated data) | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **4h** | **4h** | **2h** | **3.5h** | **5h** | **5h** | **26.5h** |

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-finding-dirty-data.md](./lecture-notes/01-finding-dirty-data.md) | Auditing for duplicates, whitespace, inconsistent categories, type mismatches; what "tidy data" means | 2h |
| 2 | [lecture-notes/02-cleaning-techniques.md](./lecture-notes/02-cleaning-techniques.md) | Remove Duplicates, Flash Fill, Find & Replace, `TRIM`/`CLEAN` at scale, reshaping to one row per record | 2h |
| 3 | [lecture-notes/03-validation-and-drop-downs.md](./lecture-notes/03-validation-and-drop-downs.md) | Data Validation rules, dependent drop-down lists, conditional-formatting violation flags | 2h |
| 4 | [exercises/exercise-01-dedupe-a-list.md](./exercises/exercise-01-dedupe-a-list.md) | Dedupe a Customer List | 1h |
| 5 | [exercises/exercise-02-standardize-categories.md](./exercises/exercise-02-standardize-categories.md) | Standardize Category Values | 1.5h |
| 6 | [exercises/exercise-03-dependent-dropdowns.md](./exercises/exercise-03-dependent-dropdowns.md) | Build Dependent Drop-Downs | 1.5h |
| 7 | [challenges/challenge-01-clean-a-real-csv-dump.md](./challenges/challenge-01-clean-a-real-csv-dump.md) | Clean a Real-World CSV Dump | 1h |
| 8 | [challenges/challenge-02-validation-guarded-form.md](./challenges/challenge-02-validation-guarded-form.md) | Build a Validation-Guarded Entry Form | 1h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Turn a raw CSV dump into a clean, validated dataset | 2.5h |
| 10 | [homework.md](./homework.md) | Extra practice, spaced across the week | 5h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Official docs + the few tools worth your time | — |

## By the end of this week you can…

- Look at any raw export and, within a minute, list its specific data-quality problems by name (duplicates, whitespace, mixed casing, mixed date formats, blank rows).
- Deduplicate a list correctly — including catching near-duplicates Remove Duplicates alone would miss.
- Turn five spellings of the same category into one canonical value, across an entire column, in under a minute.
- Build a drop-down that only offers valid choices, and a second drop-down whose options depend on the first.
- Flag bad or outlier data in color automatically, so review is visual instead of row-by-row.
- Hand off a dataset you'd trust a pivot table to summarize without double-checking it first.

## Up next

[Week 6 — Tables & structured references](../week-06-tables-and-structured-references/) — once your data is clean and guarded, we turn the range into a real Excel/Sheets Table that expands itself and drives a summary sheet automatically.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
