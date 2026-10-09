# ITSM Dataset — Data Profiling Report

**Project:** IT Service Management Performance & SLA Analytics  
**Dataset:** `ITSM_Dataset(1).csv`  
**Status:** Initial assessment — detailed Power Query validation pending

## 1. Dataset Overview

| Attribute | Initial finding |
|---|---|
| Rows | 100,000 |
| Columns | 22 |
| Data grain | Intended to be one record per IT service ticket |
| Candidate primary key | `Ticket ID` |
| Creation date range | 1 April 2024 – 11 August 2024 |
| Initial missing cells | 0 reported |
| Duplicate complete rows | 0 reported |
| Unique ticket IDs | 100,000 reported |

> These are initial findings. Recheck them in Power Query using the full dataset, and update this report if the results differ.

## 2. Column Names and Data Types

Inspect the type icon beside every column in Power Query. Confirm that dates/timestamps use a Date/Time type, latitude and longitude use Decimal Number, and identifiers are treated consistently.

| Field group | Fields to inspect |
|---|---|
| Ticket identifiers and status | `Ticket ID`, `Status`, `Priority`, `Source`, `Topic` |
| Assignment | `Agent Group`, `Agent Name`, `Support Level` |
| Timestamps | `Created time`, `First response time`, `Resolution time`, `Close time` |
| SLA deadlines | `Expected SLA to resolve`, `Expected SLA to first response` |
| SLA outcomes | `SLA For first response`, `SLA For Resolution` |
| Interactions and feedback | `Agent interactions`, `Survey results` |
| Product and geography | `Product group`, `Country`, `Latitude`, `Longitude` |

**Result:** Pending detailed Power Query inspection.

## 3. Missing Value Analysis

The initial file assessment reported no missing cells. Confirm this with **View → Column quality** and **Column profile**, with profiling set to the entire dataset.

Check for both nulls and text values that are empty or contain only whitespace.

| Check | Result |
|---|---|
| Null values | 0 reported initially; verify in Power Query |
| Empty/whitespace text | Pending |
| Missing response/resolution timestamps | Pending |
| Missing SLA deadlines | Pending |
| Missing survey responses | Pending |

A missing survey response may mean the customer did not complete a survey; do not automatically treat it as a data error.

## 4. Duplicate Analysis

The initial assessment reported 100,000 unique ticket IDs and zero fully duplicated rows.

Validate `Ticket ID` independently by grouping by that field and counting rows per ID. Filter for counts greater than 1.

| Check | Result |
|---|---|
| Fully duplicated rows | 0 reported initially |
| Duplicate `Ticket ID` values | 0 reported initially; verify in Power Query |

Do not remove records automatically. Investigate duplicate IDs first if any are found.

## 5. Unique Values and Cardinality

Use **Column distribution** and **Column profile** to inspect distinct and unique counts for fields such as:

- `Status`
- `Priority`
- `Source`
- `Topic`
- `Agent Group`
- `Agent Name`
- `Support Level`
- `Product group`
- `Country`
- `SLA For first response`
- `SLA For Resolution`
- `Survey results`

**Result:** Record distinct counts and any inconsistent labels after inspecting the full dataset.

## 6. Categorical Variable Distributions

Review the value distribution for status, priority, source, topic, agent group, support level, product group, country, survey results and both SLA outcome columns.

The initial assessment found that both SLA outcome fields contained only `Met`. If confirmed, the existing labels alone cannot distinguish SLA breaches. Validate the labels against actual timestamps and SLA deadlines before using them in KPIs.

**Result:** Detailed category counts pending.

## 7. Numerical Variable Distributions

Inspect the minimum, maximum, average and distribution for `Agent interactions`, `Latitude` and `Longitude`.

Checks to perform:

- Agent interaction counts should not be negative.
- Latitude should be between -90 and 90.
- Longitude should be between -180 and 180.
- Check whether coordinates are missing, constant, or implausible for the recorded country.

**Result:** Pending detailed validation.

## 8. Date and Time Validation

The initial assessment reported a creation-date range from **1 April 2024 to 11 August 2024**, with no values that failed datetime parsing.

Validate these rules:

- First response should not precede ticket creation.
- Resolution should not precede ticket creation.
- Closure should not precede ticket creation.
- Where both exist, resolution should generally not occur after closure.
- Check whether non-Closed tickets have a close timestamp.
- Check whether Closed tickets have a close timestamp.

Initial investigation found a potential inconsistency: **79,985 records had a close timestamp while their status was not `Closed`**. Confirm this in Power Query and investigate the dataset's status definitions before deciding how to handle those rows.

## 9. SLA Data Validation

The dataset includes first-response and resolution timestamps, expected SLA deadlines, and recorded SLA outcome labels.

Recalculate SLA outcomes where both the actual timestamp and its corresponding deadline are available:

- First response met: `First response time <= Expected SLA to first response`
- Resolution met: `Resolution time <= Expected SLA to resolve`

These rules assume the expected SLA fields are absolute datetime deadlines. Confirm the field meaning and timezone assumptions before finalising the measures.

Initial assessment found that both recorded SLA outcome columns contained only `Met`. Recalculated outcomes and breach counts are **pending**.

For tickets without a response or resolution timestamp, do not automatically classify the SLA as met. For unresolved tickets, determine whether the deadline had passed by a clearly defined reporting cutoff date.

## 10. Business-Rule Validation

Create temporary Power Query validation columns in a referenced profiling query. Suggested checks:

| Validation | Rule |
|---|---|
| Response before creation | `First response time < Created time` |
| Resolution before creation | `Resolution time < Created time` |
| Closure before creation | `Close time < Created time` |
| Resolution after closure | `Resolution time > Close time`, where both exist |
| SLA first-response check | Compare actual response with expected deadline |
| SLA resolution check | Compare actual resolution with expected deadline |
| Status/closure consistency | Compare `Status` with presence of `Close time` |
| Coordinate validity | Check geographic ranges |
| Negative interactions | `Agent interactions < 0` |

Treat these as investigation flags, not automatic reasons to delete records. Some exceptions may be explained by the dataset's lifecycle definitions or construction.

## 11. Initial Data Quality Findings

| Finding | Initial status | Impact |
|---|---|---|
| Dataset contains 100,000 records and 22 columns | Observed | Suitable size for Power BI modelling practice |
| Ticket IDs appear unique | Reported; verify | Supports ticket-level analysis if confirmed |
| No missing cells reported | Reported; verify | Does not rule out empty strings or logically missing values |
| No fully duplicated rows reported | Reported; verify | Does not replace duplicate-key validation |
| Recorded SLA labels all appear as `Met` | Observed; verify | Could make label-based SLA KPIs misleading |
| Many non-Closed records have close timestamps | 79,985 initially identified | Requires lifecycle-rule investigation |
| Creation dates span April–August 2024 | Observed | Short time window limits year-over-year analysis |

## 12. Recommended Next Actions

1. Confirm the column types and profiling results in Power Query.
2. Verify missing values and duplicate `Ticket ID` values using the full dataset.
3. Review categorical distributions and distinct counts.
4. Add temporary data-quality flags for timestamp order and status/closure consistency.
5. Recalculate SLA results from timestamps and deadlines where valid.
6. Investigate the dataset's provenance and any licence or attribution requirements.
7. Document findings before applying cleaning transformations.
8. Preserve the raw source query and perform transformations in a separate referenced query.

## Notes and Limitations

This report records initial observations and planned checks. Items marked **pending** must be completed in Power Query before they are reported as final results. Do not invent missing SLA breaches, agent reassignments, reopen history, or other events that are not supported by the source data.
