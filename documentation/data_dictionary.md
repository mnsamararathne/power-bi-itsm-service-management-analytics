# ITSM Dataset – Data Dictionary

## Dataset Overview

- **Dataset:** ITSM Dataset
- **Domain:** IT Service Management (ITSM)
- **Grain:** One row represents one IT service/support ticket
- **Number of Records:** 100,000
- **Number of Variables:** 22
- **Primary Key:** Ticket ID

---

## Data Dictionary

| # | Column | Description | Target Data Type | Analytical Role |
|---|---|---|---|---|
| 1 | Status | Current/final status of the service ticket | Text | Dimension |
| 2 | Ticket ID | Unique identifier assigned to each service ticket | Text | Primary Key |
| 3 | Priority | Priority level assigned to the ticket based on its importance/severity | Text | Dimension |
| 4 | Source | Channel through which the service ticket was submitted | Text | Dimension |
| 5 | Topic | Subject or issue category associated with the ticket | Text | Dimension |
| 6 | Agent Group | Support group responsible for handling the ticket | Text | Dimension |
| 7 | Agent Name | Name/identifier of the support agent assigned to the ticket | Text | Dimension |
| 8 | Created time | Date and time when the ticket was created | Date/Time | Fact / Timestamp |
| 9 | Expected SLA to resolve | SLA deadline by which the ticket is expected to be resolved | Date/Time | Fact / SLA Target |
| 10 | Expected SLA to first response | SLA deadline by which the first response is expected | Date/Time | Fact / SLA Target |
| 11 | First response time | Actual date and time of the first response to the ticket | Date/Time | Fact / Timestamp |
| 12 | SLA For first response | Indicates whether the first-response SLA requirement was achieved | Text | Dimension / KPI |
| 13 | Resolution time | Actual date and time when the ticket was resolved | Date/Time | Fact / Timestamp |
| 14 | SLA For Resolution | Indicates whether the resolution SLA requirement was achieved | Text | Dimension / KPI |
| 15 | Close time | Date and time when the ticket was formally closed | Date/Time | Fact / Timestamp |
| 16 | Agent interactions | Number of interactions performed by support agents while handling the ticket | Whole Number | Measure |
| 17 | Survey results | Customer satisfaction/feedback result associated with the ticket | Text | Dimension / KPI |
| 18 | Product group | Product or service group associated with the ticket | Text | Dimension |
| 19 | Support Level | Support tier responsible for handling the ticket, such as L1, L2 or L3 | Text | Dimension |
| 20 | Country | Country associated with the service ticket | Text | Geographic Dimension |
| 21 | Latitude | Latitude coordinate associated with the country/location | Decimal Number | Geographic Attribute |
| 22 | Longitude | Longitude coordinate associated with the country/location | Decimal Number | Geographic Attribute |

---

## Dataset Grain

The dataset is structured at the **service-ticket level**.

> **One row = One IT service/support ticket**

`Ticket ID` is expected to uniquely identify each record and will be validated during the data profiling and data quality assessment stages.

---

## Key Analytical Groups

### Ticket Characteristics
- Ticket ID
- Status
- Priority
- Source
- Topic
- Product group

### Service Desk Operations
- Agent Group
- Agent Name
- Support Level
- Agent interactions

### Time and Lifecycle
- Created time
- First response time
- Resolution time
- Close time

### SLA Management
- Expected SLA to first response
- SLA For first response
- Expected SLA to resolve
- SLA For Resolution

### Customer Experience
- Survey results

### Geographic Information
- Country
- Latitude
- Longitude

---

## Planned Data Quality Validation

The following checks will be performed before data transformation and modelling:

- Validate uniqueness of `Ticket ID`
- Check for missing and blank values
- Check duplicate records
- Validate categorical values
- Validate date/time data types
- Validate chronological ticket lifecycle
- Compare actual first response time against expected first-response SLA
- Compare actual resolution time against expected resolution SLA
- Validate `Agent interactions` for invalid or negative values
- Validate latitude and longitude ranges
- Assess categorical cardinality and distributions

---

## Planned Derived Metrics

The following analytical variables may be derived during Power Query transformation and/or Power BI modelling:

- First Response Time
- Resolution Duration
- Closure Duration
- Resolution-to-Close Duration
- Response SLA Variance
- Resolution SLA Variance
- Calculated Response SLA Status
- Calculated Resolution SLA Status
- Ticket Creation Date
- Year
- Quarter
- Month
- Week
- Day of Week
- Hour of Day
- Weekend Flag

These variables will only be created after the raw dataset has been profiled and the appropriate business rules have been established.