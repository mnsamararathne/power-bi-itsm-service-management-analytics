# ITSM Dataset – Data Dictionary

## Dataset Overview

- **Dataset:** ITSM Dataset
- **Grain:** One row represents one IT service/support ticket
- **Number of Records:** 100,000
- **Number of Variables:** 22
- **Primary Key:** Ticket ID

## Data Dictionary

| Column | Description | Data Type | Analytical Role |
|---|---|---|---|
| Ticket ID | Unique identifier for each IT service ticket | Text | Primary Key |
| Priority | Priority assigned to the ticket | Text | Dimension |
| Source | Channel through which the ticket was raised | Text | Dimension |
| Created time | Date and time the ticket was created | Date/Time | Fact |
| First response time | Date and time of first response | Date/Time | Fact |
| Resolution time | Date and time the ticket was resolved | Date/Time | Fact |
| Agent Name | Support agent responsible for the ticket | Text | Dimension |