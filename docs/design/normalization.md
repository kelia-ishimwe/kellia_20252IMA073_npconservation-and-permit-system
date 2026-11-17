# Normalization — Phase II

## 1. Data sources and grains

### Source selection rationale

The Rwanda Statistical Yearbook 2025, published by the National Institute of Statistics of Rwanda (NISR), was selected because it was the latest officially available statistical yearbook at the time of data collection. It provides national park visitation statistics covering the period 2005–2024. The 2026 edition was not yet scheduled for release until 31 December 2026.


**NISR annual visits:** the raw row represents one year and contains three park-specific measures. Its physical grain is one year; after unpivoting, the analytical grain is one park in one year.

**GBIF biodiversity:** the intended grain is one published occurrence record. The GBIF CSV in this package is currently only a header placeholder, so no GBIF sample values or occurrence counts are asserted. Confirm the actual exported columns and sample rows after downloading.

## 2. UNF (raw NISR structure)

The raw source has repeating park-measure columns in one annual row. Example values copied from the source for 2022–2024:

| year | volcanoes_national_park_visits | akagera_national_park_visits | nyungwe_national_park_visits | total_visits |
|---:|---:|---:|---:|---:|
| 2022 | 34570 | 50971 | 22435 | 107976 |
| 2023 | 47802 | 63341 | 24726 | 135869 |
| 2024 | 53197 | 67661 | 29667 | 150525 |

The park measures are repeated as separate columns. `total_visits` is derived from those measures.

## 3. 1NF

Convert the repeating park columns into one row per park-year. Each cell contains one value and each row has a single park and year.

| year | park_name | annual_visits |
|---:|---|---:|
| 2022 | Volcanoes National Park | 34570 |
| 2022 | Akagera National Park | 50971 |
| 2022 | Nyungwe National Park | 22435 |
| 2023 | Volcanoes National Park | 47802 |
| 2023 | Akagera National Park | 63341 |
| 2023 | Nyungwe National Park | 24726 |
| 2024 | Volcanoes National Park | 53197 |
| 2024 | Akagera National Park | 67661 |
| 2024 | Nyungwe National Park | 29667 |

Candidate key: `(park_name, year)`.

## 4. 2NF

In the 1NF relation, park attributes (such as park name and park description) depend only on `park_name`, not on the whole composite key `(park_name, year)`. This is a partial dependency. Move park details to `PARK`.

**PARK** (`park_id` PK, `park_name` UNIQUE, `park_description`)

**ANNUAL_PARK_VISIT** (`park_id` PK/FK, `visit_year` PK, `annual_visits`, `source_id` FK)

The measure `annual_visits` depends on the complete composite key `(park_id, visit_year)`. Park descriptive attributes are no longer stored in the visit fact table.

## 5. 3NF

Source details such as publisher, source title, URL, licence and access date depend on `source_id`, not on the park-year key. Store them once in `DATA_SOURCE`, and reference them from the visit fact table. Do not store `total_visits` because it is calculated from the park-level facts.

**DATA_SOURCE** (`source_id` PK, `source_title`, `publisher`, `source_url`, `licence_note`, `date_collected`)

**PARK** (`park_id` PK, `park_name` UNIQUE, `park_description`)

**ANNUAL_PARK_VISIT** (`park_id` PK/FK, `visit_year` PK, `annual_visits`, `source_id` FK)

This removes the transitive dependency on source metadata. A report can calculate a yearly total with `SUM(annual_visits)` grouped by year.

## 6. Additional logical entities for the conservation/permit system

These are proposed application entities for the system workflow, not rows claimed to exist in the NISR CSV or current GBIF placeholder:

- `VISITOR` — applicant/visitor account.
- `PERMIT_APPLICATION` — request for a park visit.
- `PERMIT` — approved permit issued from an application.
- `PAYMENT` — prototype payment status/transaction reference.
- `PARK_ENTRY` — gate verification and entry event.
- `CONSERVATION_OBSERVATION` — ranger/conservation observation.
- `TAXON` — standardized taxon information from GBIF.
- `OCCURRENCE` — one GBIF occurrence record.
- `LOCATION` — occurrence locality and coordinates, where provided.
- `DATA_SOURCE` — source provenance for imported data.

Do not join a GBIF occurrence to a park unless the record's locality/coordinates have been checked against an authoritative park boundary. Rwanda-wide occurrence data may be outside the three parks.

## 7. Raw-field mapping (NISR CSV)

| Raw field | Target | Mapping / treatment |
|---|---|---|
| `index` | Dropped from logical model | Row-order artifact; not a stable business key |
| `year` | `ANNUAL_PARK_VISIT.visit_year` | Integer year |
| `volcanoes_national_park_visits` | `ANNUAL_PARK_VISIT.annual_visits` | One row with Volcanoes `park_id` per year |
| `akagera_national_park_visits` | `ANNUAL_PARK_VISIT.annual_visits` | One row with Akagera `park_id` per year |
| `nyungwe_national_park_visits` | `ANNUAL_PARK_VISIT.annual_visits` | One row with Nyungwe `park_id` per year |
| `total_visits` | Dropped / calculated in query | Derived value; validate during Phase IV |
| `source` | `DATA_SOURCE` | Store source metadata once and reference by `source_id` |

## 8. GBIF field mapping — confirm against actual export

| GBIF field (if present) | Proposed target | Notes |
|---|---|---|
| `gbifID` | `OCCURRENCE.gbif_id` | Preserve as source record identifier |
| `occurrenceID` | `OCCURRENCE.occurrence_id` | Nullable if source omits it |
| `scientificName` | `TAXON.scientific_name` | Link to taxon record; check taxon key too if available |
| `taxonRank`, `kingdom`, `phylum`, `class`, `order`, `family`, `genus`, `species` | `TAXON` | Use the columns actually present in export |
| `eventDate`, `year` | `OCCURRENCE.event_date`, `occurrence_year` | Parse dates only in Phase IV |
| `locality`, `decimalLatitude`, `decimalLongitude`, `coordinateUncertaintyInMeters` | `LOCATION` | Preserve original values in raw; validate coordinates in Phase IV |
| `basisOfRecord`, `datasetName`, `license` | `OCCURRENCE` / `DATA_SOURCE` | Confirm exact column availability and licence at download |

## 9. Phase II quality note

The NISR file can be assessed now. GBIF quality counts, null counts, duplicates, date ranges, coordinate validity and licence must be measured on the actual downloaded file. Do not invent those numeric results. Keep raw data unchanged until Phase IV.
