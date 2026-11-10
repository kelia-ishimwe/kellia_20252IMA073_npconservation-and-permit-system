# National Parks Conservation and Permit System

**Student:** ISHIMWE HAKORIMANA Kellia  
**Student ID:** 20252IMA073  
**Course:** Database Development with PL/SQL (INSY 8311)  
**Instructor:** Eric Maniraguha

## Project overview
The National Parks Conservation and Permit System is an academic Oracle Database 21c and PL/SQL project that proposes a centralized way to manage selected national-park operations: visitor permit applications, permit review and approval, payment records, daily visitor capacity, entry verification, conservation activities/incidents and audit history.

This is a student prototype. It is not an official Rwanda Development Board system and is not connected to government booking/payment services.

## Problem statement
National park operations involve visitor access, permit processing, payments, capacity management and conservation monitoring. When related records are held in separate workflows, staff may find it difficult to obtain a consistent view of an application’s status, confirm payment, check capacity, verify entry and review conservation records. Rwanda already has official online permit-booking services; this project therefore does not claim that online permits do not exist. It explores an academic database prototype that integrates selected operational and conservation records and demonstrates database-enforced rules.

## Objectives
### Main objective
To design and implement an Oracle-based National Parks Conservation and Permit System that supports structured permit administration, visitor-capacity control and conservation record management.

### Specific objectives
1. Model the permit application, review, payment, approval and park-entry processes.
2. Design normalized relational tables and relationships for parks, activities, permits, visitors, payments, capacity and conservation records.
3. Implement PL/SQL procedures, functions, cursors, packages, triggers and exception handling for the selected business rules.
4. Record important database actions in an audit log.
5. Build an Oracle APEX interface/dashboard connected to the Oracle database to display operational information.

## Data source
Primary official source: National Institute of Statistics of Rwanda, *Rwanda Statistical Yearbook 2025*, Table 9.1 (Parks Visits from 2005–2024), with source data credited to Rwanda Development Board:
https://statistics.gov.rw/sites/default/files/documents/2025-12/RWANDA_STATISTICAL_YEARBOOK_2025.pdf

`data/rwanda_national_park_visits_2005_2024.csv` contains 60 park-year observations (20 years × 3 parks). These are real aggregate statistics, not individual visitor or permit records. Since the official series alone is below the “few hundred records” target, the project plans to seek permission to collect at least 200 anonymous visitor-experience survey responses. The blank template is in `data/anonymous_visitor_survey_template.csv`. No responses have been fabricated or collected yet.

## Planned tables
- `PARK` — park details and status.
- `VISITOR` — minimum visitor profile needed for permit processing; use test or consented, non-identifying data only.
- `PERMIT_TYPE` — available permit/activity categories.
- `PERMIT_APPLICATION` — requested visit date, group size and application status.
- `PERMIT` — approved permit reference and validity.
- `PAYMENT` — payment reference, amount, date and payment status (prototype records only).
- `PARK_CAPACITY` — capacity limits by park, activity and date/time slot.
- `PARK_ENTRY` — permit verification and entry/exit status.
- `CONSERVATION_INCIDENT` — conservation incident/observation records.
- `STAFF_USER` — system user roles.
- `AUDIT_LOG` — recorded sensitive actions and status changes.

Final table names and columns will be confirmed during Phase II after normalization and ERD design.

## Planned PL/SQL features
- **Procedures:** submit an application, review/approve/reject an application, record a payment, verify a permit at entry and record a conservation incident.
- **Functions:** calculate remaining capacity, validate permit eligibility and calculate application/payment status summaries.
- **Cursors:** produce park-wise visitor and permit summaries for a selected date range.
- **Packages:** group permit operations and reporting functions into logical packages.
- **Triggers:** prevent approval when capacity would be exceeded; prevent entry with an invalid/unpaid/expired permit according to the project's defined rules; write important changes to the audit log.
- **Exceptions:** handle missing permits, duplicate references, invalid status transitions and capacity violations.

## Innovation
**Option:** Interactive user interface and analytics dashboard.  
**Tool:** Oracle APEX.

The APEX application will connect to the same Oracle database and provide role-appropriate pages for application entry/review, permit/payment status, capacity summaries, park-entry verification and conservation reporting. Business rules remain in PL/SQL; APEX will call database procedures and display database results. The connection and live demonstration will be implemented in later phases. Static screenshots alone will not be treated as proof of a working connection.

## Planned repository structure
```text
kellia_20252ima073_nationalparks/
├── README.md
├── data/
│   ├── rwanda_national_park_visits_2005_2024.csv
│   └── anonymous_visitor_survey_template.csv
├── docs/
│   ├── research/
│   │   └── research_log.md
│   ├── proposal/
│   │   └── phase0_proposal.docx
│   └── presentation/
│       └── phase0_presentation.pptx
└── innovation/
    └── README.md
```

## References
- National Institute of Statistics of Rwanda. (2025). *Rwanda Statistical Yearbook 2025*.
- Oracle. (2024). *Oracle Database PL/SQL Language Reference, 21c*.
- Oracle. (n.d.). *Oracle APEX documentation*.
- Pásková, M., Wall, G., Zejda, D., & Zelenka, J. (2021). Tourism carrying capacity reconceptualization: Modelling and management of destinations. *Journal of Destination Marketing & Management, 21*, 100638.
- Rwanda Development Board. (n.d.). *Visit Rwanda bookings: Online permits/bookings*.
- Schwartz, Z., Stewart, W., & Backlund, E. A. (2012). Visitation at capacity-constrained tourism destinations: Exploring revenue management at a national park. *Tourism Management, 33*(3), 500–508.
- Štekerová, K., Zelenka, J., & Kořínek, M. (2022). Agent-based modelling in visitor management of protected areas. *Sustainability, 14*(19), 12490.
