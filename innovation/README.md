# Innovation plan — Oracle APEX dashboard

**Chosen innovation option:** User interface / interactive application and analytics dashboard.  
**Tool:** Oracle APEX, connected to the project's Oracle Database 21c schema.

## Planned pages
1. Overview dashboard: permit applications, approvals, payment totals and visitor counts.
2. Permit applications: submit, review and view status.
3. Capacity monitor: daily capacity, approved group size and remaining capacity.
4. Entry verification: look up a permit and record entry status.
5. Conservation records: record and review conservation observations/incidents.
6. Audit monitor: authorized staff view recorded changes and denied/invalid operations.

## Important implementation rule
The APEX interface will call PL/SQL procedures and query database views. Capacity checks, permit eligibility, status transitions and audit recording will be enforced in PL/SQL/database constraints, not only in page-level interface code.

## Current status
Planned only in Phase 0. No APEX application has been built or connected yet. Later phases must include setup instructions, screenshots and a live demonstration against the actual Oracle project database.
