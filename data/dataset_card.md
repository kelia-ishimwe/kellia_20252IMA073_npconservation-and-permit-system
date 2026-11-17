# Dataset Card — Phase II

## Dataset A: Rwanda national park visits (official annual statistics)

- **File:** `Rwanda_National_Park_Visits_2005_2024.csv`
- **Source:** National Institute of Statistics of Rwanda (NISR), *Rwanda Statistical Yearbook 2025*, Table 9.1; underlying figures credited to Rwanda Development Board (RDB).
- **Source publication:** https://statistics.gov.rw/statistical-publications/statistical-yearbook/rwanda-statistical-yearbook-2025
- **PDF source:** https://statistics.gov.rw/sites/default/files/documents/2025-12/RWANDA_STATISTICAL_YEARBOOK_2025.pdf
- **Coverage:** 2005–2024.
- **Format:** CSV, comma-separated.
- **Physical rows:** 20 annual rows (plus header).
- **Analytical observations:** 60 park-year values (20 years × 3 parks). These are aggregate annual counts, not individual visitors or permits.
- **Meaning of a record:** One calendar year with separate annual visit totals for Volcanoes, Akagera and Nyungwe National Parks.
- **Licence:** The NISR publication page/PDF should be checked for the applicable reuse terms. Do not assume a licence that is not stated.
- **Personal data:** None.

### Justification for selecting the 2025 yearbook

The Rwanda Statistical Yearbook 2025, published by the National Institute of Statistics of Rwanda (NISR), was selected because it was the latest officially available statistical yearbook at the time of data collection. It provides national park visitation statistics covering the period 2005–2024. The 2026 edition was not yet scheduled for release until 31 December 2026.

### Initial quality assessment (raw file; do not clean in Phase II)

- 20 data rows; 20 distinct years.
- No duplicate year values found.
- No missing values found in the current CSV copy.
- Visit measures are numeric.
- `index` is a row-order field, not a business identifier.
- `total_visits` is a derived total; verify it against the sum of the three park values during Phase IV.
- `source` repeats the source citation and should be represented as a source relationship in the logical design rather than repeated in every fact row.

## Dataset B: Rwanda biodiversity occurrences (GBIF)

- **Required file name:** `Rwanda_GBIF_Biodiversity.csv`
- **Current status:** **NOT DOWNLOADED — header-only placeholder.** No biodiversity occurrence rows have been fabricated or added.
- **Candidate source:** GBIF Rwanda occurrence records. The official Rwanda biodiversity report documents GBIF mobilization in Rwanda, including 7,210 bird records in one reported category. This historical figure is evidence of data availability, not the row count of this project's download.
- **GBIF portal:** https://www.gbif.org/occurrence/search?country=RW
- **GBIF data-use guidance:** https://www.gbif.org/terms/data-user
- **Candidate park-specific dataset:** Bat species occurrences in Nyungwe National Park, Rwanda (392 records in its archive; this alone does not meet a 500-record minimum): https://ipt.vertnet.org/resource?r=bci_rwanda
- **Official biodiversity report:** https://www.rema.gov.rw/fileadmin/user_upload/Rwanda_6th_National_Report_to_the_Convention_on_Biological_Diversity.pdf
- **Format expected:** GBIF Simple CSV/TSV or Darwin Core Archive exported to CSV.
- **Record meaning:** One published biodiversity occurrence record (observation, specimen or other occurrence evidence), not necessarily one unique animal.
- **Collection date, exact download DOI, licence and actual row count:** Fill these from the GBIF download receipt/metadata after downloading.
- **Personal data:** Review downloaded fields. Remove or mask personal contact fields if any appear, while preserving a separate untouched original if required by the assignment and permitted by the data licence.

### Important limitation

The assignment's 500-record minimum is **not yet satisfied** by the current files. Dataset A has 20 physical rows. The GBIF placeholder has zero occurrence rows. Download a real GBIF export and replace the placeholder with the unchanged export before claiming the minimum is met. Do not pad the file, invent observations, or claim that Rwanda-wide records all come from the three national parks. Keep the original GBIF export unchanged in `data/raw/`; if you filter or transform it, put the derived file in a separate staging/working folder and document the process.
