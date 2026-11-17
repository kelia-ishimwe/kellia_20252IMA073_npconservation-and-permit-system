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
  
  ## Phase I – Business Process Modeling

### Selected Business Process
**Tourist Park Entry & Conservation Activity Permit Issuance:** This process governs how visitors and tour operators apply for national park entry and specialized conservation permits, how daily ecological carrying capacities and tiered fees are validated, how payments are settled, and how park rangers verify permits and log visitor check-ins at park gates.

### Modeling Tool Used
* **Tool:** diagrams.net (draw.io) 

### Deliverables & Links
* **Published Interactive Diagram:** [View Business Process Diagram](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=_business_process_diagram.drawio&dark=auto#R%3Cmxfile%3E%3Cdiagram%20name%3D%22Visitor%20Permit%20Application%2C%20Approval%20and%20Park%20Entry%22%20id%3D%22ZMmBCR6YFPHnzRGDK3WM%22%3E7V1tk6K4Fv411sx%2BmCleBPEjIvZ27YzdV3t6a%2FbLFoNRuYPEC6jd%2B%2BtvQghISCuNQXBqe2t3NYQQz3POyTlPXuip1ublLnS2669wAfyeIi1eeuq4pyiyNlTR%2F3DJa1oy1GVSsgq9RVqWF8y9f0BaKKWlO28BokLFGEI%2F9rbFQhcGAXDjQpkThvBQrLaEfvGpW2cFSgVz1%2FHLpX96i3hNSg1Nyst%2FB95qTZ8sS%2BmVjUMrpwXR2lnAw1GRavdUK4QwJp82LxbwsfSoXMh9kzeuZh0LQRBXuUGDf%2FxnJ7%2Fc%2FfW7CeFy60l%2FP9uflHIracNR%2FEplEMJdsAC4Gbmnjg5rLwbzrePiqwcEOypbxxs%2Fvbz0fN%2BCPgyTe9WJYdmWhcqjOIQ%2FwdGVkaH1kRjRHTCIU%2BBl%2FN3xvVWAvrioSwBVHpEO7R1%2Fl3aop%2Bg%2B6u3oB%2Fqwwh%2FG5pOJqszsJ3v6dP8w7akmrYMEklVDVczQXXt7gD6Bl60Xol%2BF9GS79T3XiT0YRPhrgAvXXhTDEBVjGW5BuPFi9CEELgwXSS0Xf%2FKCVaKR%2BD9rQCT5E1%2FdxWt0d%2Fz6IUruitFvQc3jChA96xV9%2BJh1ZTL%2B7TMqGONWAhinz3MCdI%2BPayKjQg1g7fUCJ3C9pEtIjPgxC6ZbBw89eBfnXfgn%2BV2fUxmCMAYvRzinqnIH4AbEIX7Y%2BkibqZYfcs3X9bSMtqKm31MDNwwtNb7U8FZZ07luog%2Bper5DVdXGVTX546mqnvydUFUfLLF%2BRehRSCO%2BJN9Ijcq6%2B%2FBoz8xEdxXpi31nT8d8DU7vDtkS3LXED5Jef0DdVuzBWFU%2B0JpI7NIn9K8VAgerU9Y6uTUzkd4A1zzRrqzpmiVl7c7SdmcIZg%2FsL2nZnuiWlLf8LW3523ZxWY8tXTEUI2t3nLY7poaFNfPIN7zxHFb0c2TKi3ycQc1ghKUI%2FG8HAjcxWB8eEtN2ojXgVPWCJUSWnroG0gvkAbxl6o6OmrhDEjg4r1EykqBxBLXlI%2FcYoHp74njidSTMzI1%2B0cwN6Vpm3q9i5tHB2%2FjIQWJ7xj4OweRgq5bOG30UOyG1YVV7jxcgf9QLpJ2RGa8g82z%2B%2BX5%2B%2F%2FQwqwWOLJfRUbKgIm2HccL0Kg%2Bc9GkzFCw5wcoHzOMG5af13%2FUsqpVghF1xVNKH7GfWVxGtrCKnVOlIb1DjKHAE57XEibYkmlx6L3g4YbVkrNnGuM%2FTEkMZqclYQa7QoFH%2BrBU1ZchRpJLizJ%2FM2RNyN9jpYt1%2B9iIvTkb%2BgxPEURp6ZJf3%2BDLGIgtEaiicrpUHfa2gAfQr1QClIVeg18a5mTCg6ACOwoASbjOwQuEjCDOn7sNV4u5F%2BefMK9BWGEzUEx7gIkwGN4vJHA32bnwUouMwv2g6JJonsYYonIqmo0pXwsloBafL3SIfOzsgxrTP%2FN8CxI7nR0cIkrQt2v2gmRqKwqJYGJBKEUht0BRyw%2FZHtwpJexFGpcbYRjIcip61hjACiVlClEmHmR1mNYgjrT2sDZQSprToJYO4GNrIRkMQU%2Bd9q0Eu10anSfZqfkGlj%2BbsjyPkvppT887%2Bak%2BPQ5n59%2FmT%2FbVeSKy8PySWT3naMzExLwTX3%2Fe0K0TFMocRPKl9ony%2BaRsTrrPQLcMeTer6%2FJxOoCN2IVvO%2FYI2mvW0ceOOno7YTSW%2BMoeLvWX8rDVwf%2BbguQ4mx%2BLX0ogNXlCoTCIvQrPitObj7DdhgKpFUx3I2pUA5TCWvwKgLgwiEO5ZOwxRtBV6LmXRRSLImKTBJjuNIcghoyojuIabH7uoTqqj8GOvsT7SNVHMgrlH4bPzwwelTIgYYYyVb1Iv0iojOCgCKEsyQyI1hmBVrki0DTaUCM0dMiDS%2BaijqavikGjVHRL7ZfqH9aAyO%2BkjNwYfhwK6oUB5yIOQBMcSCYpnPcoQWQ%2FTuT17TqeAMigfJpN7y65JHQ9rUMdaU3mtXJU6oqA3gLDEQdjE%2F3BHy8Ks33sQtmdf71GuI10CHgc7o8%2B4UbU4DtIIUjx0VdkkDnRdjGRmYO%2BBQxJ7umCLnSeeUs9srhjgUCYpqudOjbPhjCwPGRwbc6dVuaWbwTFLEUkuEabA5inG8fjYZEwqKwYzQ9qUH1U45NE1QLw8MOWDaG63Idzns%2FAh%2BG%2FC07%2BF4TeBGDJxTb9fNEStKUPkrd66ZUO0YLD0wk0ekCaJxRupoSgvegY8ZJFNoVeZqeGg1%2FG0kBhj7dyPM3k8YHDSmNFu0JiRVeZfyjDd8lSKyc0JiVvFi5DqwKqUYKVF2QS0caWZTeXGVwhxHejkfmpOLTsbBB%2FN78x8yZ35ZP9pfq8Fn8wZ%2FM4lgv1%2BYwBW5WSU7i0imNFZ5YyCyXkz53WDf4%2Bo8Y1d4TFQigA1F5xUXXfTQXweQ%2BiCKEp6n8LB0JshiHchSeuinS8OLjYcMfQrUZxKVV6FB1e3o5FHimGGXrRzMb7LnS8yQmFSuSEToeiNQcfhVW5%2BKJs%2BPN1P7i26lh2Tmvd4ZKszbnHI6HPjljZsbNyqyp5QWDuetE3xQnMs8nxVFVwe2doucMgkkecn87aF%2FDwJLgWl4eemF%2FpN2Z9alUoRjGgF98m4ypMbTQweuvfLwiC4JKvl8D4JslEpB33jvCawErDwBiK8TpXuZ8pmA5nJJlQxWKS7G7KNS5nueFG0A0wj6MMuiD2%2F0C%2BXMAe0UY9sl3KBh5JQYbsZVGYVppwFtxUWzF%2BmYVWJntv2GYl6gQUvGs68BkEKbmu7jipUusGQQJSqEQ9sVQ6IA2xnyAWjMrmQojizn2bfj1DOAjTG%2FF242eKtVU2xDYokXWnKRK1KIok24IYWEMySnaIFF%2FwRSwaz6tYRgisQgDBZgJt7748WUykCZAXXJ1qlDtjnAwJFYuZWGgvI1V%2BRW0oXHMzM6V3dhQT99%2FNHg8ZWZalV%2BSNV8OaGhsbUZxCSMTUzNHTRWxTXStaduuQt52GpCp2dOWkMuarMEg%2B5jlMVBexqkxOcpXMsDcjy7I2BVZVX4oB1gxFOZmkzsEy2nySHOpDFy9k1uFx6br3NYRxo2aMUhkxW0tgWI7Xqeh7RHrShoMb0%2FWRHPPoxidzpYhAS6hT3%2BCU5C62YRDSCUpFzQUtzB2NweKly0NIZk5SlyjYpJvvXGGDYbV6DxsJJas9gUTpkqAwV3IUuONGYVoYUNUuFCMN4DVd4IZudl45yY5XYOJTBTkr%2BOAaoZOig0HUFTnkLnQ9PCHxySMXxzxUva1mgrDkLAjom60GrslYEynrQeVkbrcpaFShrDhfVMVnLb%2Fj4Kwm7L1DYMmcQ7pq0lValrYmUttJ9aautSlsXKW21%2B9LutyrtgUhpczLsa0m7EhFyHozU1I8i%2FO%2F2vFWAjH8B4ixuKCzkuBCf5FYzDJP5ZVphC72A7KVJW37EBUcsiMyewjFkzjU9dwM9ojLXDtKHXFcEnLbQHwpUHyr8DnvTYSu6oBj6KWjP30BXUTaqC5rINL9DrqSqbzdKrsOm%2B%2Bp6%2Bfw%2F2ZnVhkNhDnw471CYG5ThNZRIJH8hd58sktvxKCpznIusG6eVgb1BVa%2BhDCIJFllo1r8gR9m%2BoRnZnukLNOPodPQW4kJNJN8id39gb1naIgkX3qxHK4rOTEC%2ByyMyq3WyjZO95Ph3fOB8dCFgdR2nUvCDfUm5gh8UyRBlL2LosDG2ysdpIhkipUU%2BTlBKrJbs8eKU%2BDJ8RHJKvwA%2BTXBK9ZzjoHQi2eB0VMne0E%2B3HDXrTUVSXkr3Uwyl1QlpTShD1P0ZaaXVKWldJAejtDgnLcY30n0n7dO5Q7qOkO7PUwe9xj0dfcmYEG1QhaYVjdie2mrcSDdn%2FGt7hS0j7cclKM5gDjS6ivGJZFDU7k80kxXY1485mdP%2F6AEszWIrkq9Rux9Cqq2GkLpI%2BkNtMYQU5FgHnQlqmLNY6OvR3sz3Sjeo1zBWkXzOL6A%2B5XnKtsZlSXuf%2FpRvSFcqNqs%2FIvkmtftrT9VLpyQvO5z1bdkKOnXCmAz5Jx1zz1fC%2B3ROv99U63E2bpTeb8q8FwYZHHPWtWQmO8yzU5Ppm2GqvP10tIu8gBz4RI9%2Bwq%2F%2BlfIX5WUb8wrH4lmkIIT75GW6ZMPQI3lThR1QON%2B9%2BaS82VJjNlsOmI3rn2ps8kJf89dGE2PP376t2v8H%3C%2Fdiagram%3E%3C%2Fmxfile%3E)
* **Business Process Diagram (PDF):** [docs/business_process/business_process_diagram.pdf](docs/business_process/business_process_diagram.pdf)
* **Process Explanation Document (PDF):** [docs/business_process/business_process_explanation.pdf](docs/business_process/business_process_explanation.pdf)
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
