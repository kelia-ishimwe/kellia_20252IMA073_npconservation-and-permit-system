# Logical Data Dictionary — Phase II

The dictionary describes logical meaning and constraints only; Oracle data types and sizes are intentionally omitted at this stage.

| Entity | Attribute | Meaning | Required | Unique |
|---|---|---|---|---|
| PARK | park_id (PK) | Internal identifier for a national park | Yes | Yes |
| PARK | park_name | Official park name | Yes | Yes |
| PARK | park_description | Brief description of the park | No | No |
| DATA_SOURCE | source_id (PK) | Internal identifier for a source | Yes | Yes |
| DATA_SOURCE | source_title | Name/title of publication or dataset | Yes | No |
| DATA_SOURCE | publisher | Organization publishing the source | Yes | No |
| DATA_SOURCE | source_url | Stable source URL or DOI landing page | Yes | Yes |
| DATA_SOURCE | licence_note | Licence/reuse terms as stated by publisher | No | No |
| DATA_SOURCE | date_collected | Date this project obtained the source | Yes | No |
| ANNUAL_PARK_VISIT | park_id (PK, FK) | Park for the annual visit statistic | Yes | Part of composite key |
| ANNUAL_PARK_VISIT | visit_year (PK) | Calendar year represented | Yes | Part of composite key |
| ANNUAL_PARK_VISIT | annual_visits | Aggregate number of visits for that park/year | Yes | No |
| ANNUAL_PARK_VISIT | source_id (FK) | Source supporting the statistic | Yes | No |
| TAXON | taxon_id (PK) | Internal taxon identifier | Yes | Yes |
| TAXON | gbif_taxon_key | GBIF taxon key, when supplied | No | Yes when present |
| TAXON | scientific_name | Scientific name from source | Yes | No |
| TAXON | taxon_rank | Rank such as species or genus | No | No |
| OCCURRENCE | occurrence_id (PK) | Internal occurrence row identifier | Yes | Yes |
| OCCURRENCE | gbif_id | GBIF occurrence identifier | No | Yes when present |
| OCCURRENCE | taxon_id (FK) | Taxon associated with occurrence | Yes | No |
| OCCURRENCE | source_id (FK) | GBIF dataset/source provenance | Yes | No |
| OCCURRENCE | event_date | Date of recorded occurrence, if supplied | No | No |
| OCCURRENCE | basis_of_record | Observation/specimen evidence category | No | No |
| OCCURRENCE | location_id (FK) | Location for the occurrence | No | No |
| LOCATION | location_id (PK) | Internal location identifier | Yes | Yes |
| LOCATION | locality | Published locality description | No | No |
| LOCATION | decimal_latitude | Published latitude, if supplied | No | No |
| LOCATION | decimal_longitude | Published longitude, if supplied | No | No |
| LOCATION | coordinate_uncertainty_m | Published coordinate uncertainty | No | No |
| VISITOR | visitor_id (PK) | Internal applicant/visitor identifier | Yes | Yes |
| VISITOR | email | Visitor contact email | Yes | Yes |
| PERMIT_APPLICATION | application_id (PK) | Permit request identifier | Yes | Yes |
| PERMIT_APPLICATION | visitor_id (FK) | Applicant submitting request | Yes | No |
| PERMIT_APPLICATION | park_id (FK) | Park requested | Yes | No |
| PERMIT_APPLICATION | visit_date | Requested visit date | Yes | No |
| PERMIT_APPLICATION | party_size | Number of visitors requested | Yes | No |
| PERMIT_APPLICATION | application_status | Current request status | Yes | No |
| PERMIT | permit_id (PK) | Issued permit identifier | Yes | Yes |
| PERMIT | application_id (FK) | Approved application | Yes | Yes |
| PERMIT | permit_status | Permit validity/status | Yes | No |
| PAYMENT | payment_id (PK) | Prototype payment record identifier | Yes | Yes |
| PAYMENT | application_id (FK) | Application being paid for | Yes | No |
| PAYMENT | payment_status | Prototype payment state | Yes | No |
| PARK_ENTRY | entry_id (PK) | Gate entry verification identifier | Yes | Yes |
| PARK_ENTRY | permit_id (FK) | Permit checked at gate | Yes | No |
| PARK_ENTRY | entry_time | Recorded entry time | Yes | No |
| CONSERVATION_OBSERVATION | observation_id (PK) | Ranger observation identifier | Yes | Yes |
| CONSERVATION_OBSERVATION | park_id (FK) | Park where observation is recorded | Yes | No |
| CONSERVATION_OBSERVATION | taxon_id (FK) | Taxon observed, when identified | No | No |
| CONSERVATION_OBSERVATION | observation_date | Date of observation | Yes | No |
| CONSERVATION_OBSERVATION | observation_notes | Non-sensitive observation notes | No | No |

## Relationships and cardinality

- `PARK` 1 — many `ANNUAL_PARK_VISIT`; each annual fact belongs to exactly one park.
- `DATA_SOURCE` 1 — many `ANNUAL_PARK_VISIT`; each fact row cites one source.
- `DATA_SOURCE` 1 — many `OCCURRENCE`; each imported occurrence retains provenance.
- `TAXON` 1 — many `OCCURRENCE`; an occurrence may be linked to one taxon when identified.
- `LOCATION` 1 — many `OCCURRENCE`; an occurrence may have zero or one mapped location.
- `VISITOR` 1 — many `PERMIT_APPLICATION`.
- `PARK` 1 — many `PERMIT_APPLICATION`.
- `PERMIT_APPLICATION` 1 — zero or one `PERMIT` (only approved requests receive a permit).
- `PERMIT_APPLICATION` 1 — zero or many `PAYMENT` records (depending on the prototype payment/audit design).
- `PERMIT` 1 — zero or many `PARK_ENTRY` checks.
- `PARK` 1 — many `CONSERVATION_OBSERVATION`.
- `TAXON` 1 — zero or many `CONSERVATION_OBSERVATION`.

**Scope warning:** application workflow entities are proposed logical entities for the prototype. They are not source records in either public dataset. GBIF occurrences should not be assigned to a park without a verified spatial match.
