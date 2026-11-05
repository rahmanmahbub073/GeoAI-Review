# Earth-Observation-and-Geospatial-Intelligence-for-Disaster-Research-in-Bangladesh: data and code
Earth Observation and Geospatial Intelligence for Disaster Research in Bangladesh: A Systematic Evidence Synthesis, Critical Appraisal, and Future Agenda

This repository holds the data and code for:

> Rahman, M.M., Li, G., Adnan, M.K. (2026). *<Final paper title>*. Submitted to *International Journal of Disaster Risk Reduction*.

It contains the coded evidence base of **312 peer-reviewed studies** (306 primary studies and 6 reviews, January 2017 to October 2026) and the notebook that regenerates **every data figure, statistical table and number in the paper**. Figures 1, 2 and 20 are schematic diagrams and are not produced by code.

---

## Repository structure

```
bd-geoai-disaster-review/
├── README.md                  this file
├── LICENSE                    MIT licence (code)
├── LICENSE-DATA.md            CC BY 4.0 licence (data) and third-party terms
├── CITATION.cff               how to cite (read by GitHub's "Cite this repository")
├── .zenodo.json               metadata for the archived Zenodo release
├── environment.yml            conda environment (pinned versions)
├── requirements.txt           pip alternative
├── Makefile                   one-command reproduction
│
├── data/
│   ├── README.md              datasheet: files, variables, missing-value codes
│   ├── raw/                   extraction workbook (+ CSV export of every sheet)
│   ├── processed/             normalised analysis dataset (312 × 699)
│   ├── codebook/              derived-variable codebook
│   └── external/              boundaries, census population, river courses
│
├── prisma/                    search strings, database yields, screening log,
│                              full-text exclusions, PRISMA 2020 checklist
├── reliability/               second-coder sample and agreement (Cohen's κ)
├── notebooks/
│   └── analysis.ipynb         reproduces all figures, tables and numbers
├── scripts/
│   ├── fetch_external_data.py re-downloads boundary files (provenance)
│   ├── export_sheets_to_csv.py writes every workbook sheet to UTF-8 CSV
│   └── coder_agreement.py     computes Cohen's κ between two coders
└── outputs/
    ├── figures/               generated figures (PNG/PDF)
    └── tables/                generated tables, incl. all_tables.xlsx (main + S1–S19)
```

---

## Quick start

```bash
git clone https://github.com/<your-username>/bd-geoai-disaster-review.git
cd bd-geoai-disaster-review

# Option A: conda
conda env create -f environment.yml
conda activate bd-geoai-review

# Option B: pip
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Reproduce everything (writes to outputs/)
make reproduce
```

`make reproduce` runs `notebooks/analysis.ipynb` from top to bottom and saves an executed copy as `outputs/analysis_executed.ipynb`. Expected run time: about **<N> minutes** on a standard laptop.

Without `make`:

```bash
jupyter nbconvert --to notebook --execute notebooks/analysis.ipynb \
  --output-dir outputs --output analysis_executed.ipynb
```

---

## Data files

| File | Description | Size |
|---|---|---|
| `data/raw/extraction_workbook.xlsx` | Coded extraction: original layer, standardised (`_Std`) layer, standardisation notes, author–affiliation sheet (1,411 authorship records) | 312 studies × 60 fields |
| `data/raw/csv/*.csv` | The same sheets as UTF-8 CSV files, for long-term preservation | — |
| `data/processed/normalised_dataset.csv` | Analysis-ready dataset: one row per study | 312 × 699 |
| `data/codebook/codebook.csv` | Definition, type, allowed values, source field and derivation rule for each of the 699 variables | 699 rows |
| `outputs/tables/all_tables.xlsx` | All main and supplementary tables (S1–S19) | — |
| `prisma/` | Search strings, per-database yields, screening decisions and full-text exclusions | see `prisma/README.md` |

See [`data/README.md`](data/README.md) for variables, missing-value codes and known limitations.

---

## From data to paper

Each output in the paper is produced by one section of the notebook.

| Paper item | Notebook section | Output file |
|---|---|---|
| Table 4 (corpus profile) | `<§ 3.1>` | `outputs/tables/table04_corpus_profile.csv` |
| Fig. 3 (publication dynamics) | `<§ 3.2.1>` | `outputs/figures/fig03_publication_dynamics.pdf` |
| Fig. 8 (geographic evidence map) | `<§ 3.3.2>` | `outputs/figures/fig08_evidence_map.pdf` |
| Table 9 (CAS compliance) | `<§ 3.6.4>` | `outputs/tables/table09_cas_items.csv` |
| … | … | … |
| Tables S1–S19 | `<§ S>` | `outputs/tables/all_tables.xlsx` |

<!-- Fill in one row per figure and table. Reviewers use this table to check results. -->

---

## Coding conventions (summary)

- **Unit of observation:** one study, identified by DOI (or full citation where no DOI exists).
- **Absent information** is never counted as evidence of absence:
  - `NR`: not reported (the study did not state a value that could apply);
  - `NA`: not applicable (the field does not apply to the study);
  - blank: not coded.
- **Binary indicators** are set to 1 only when a practice is positively evidenced.
- **`_Std` fields** use a closed vocabulary, documented in `data/codebook/codebook.csv` and Supplementary Table S17.
- **Derived heuristics** (disaster-management phase, focus tier, spatial scale, landscape setting, district matching) are rule-based inferences and are labelled as such in the codebook.
- **Primary vs review:** analyses of methods, validation and appraisal use the 306 primary studies (`is_review == 0`); corpus-level descriptors use all 312.

---

## Reproducibility notes

- **Random seed.** Permutation-based tests (global Moran's *I*, 999 permutations) use a fixed seed (`SEED = <value>` in the first notebook cell). Another seed gives slightly different permutation *p*-values.
- **Package versions** are pinned in `environment.yml` and `requirements.txt`.
- **Consistency check.** The notebook checks that publication years in the workbook match the reference list for all 312 studies.
- **Continuous check.** A GitHub Actions workflow (`.github/workflows/reproduce.yml`) re-runs the notebook on every push.

---

## What is not included

- **Full texts of the included studies** are not redistributed, for copyright reasons. Every study can be retrieved from its DOI or citation in `data/raw/extraction_workbook.xlsx`.
- **Records excluded before full-text assessment** are listed with their screening decision in `prisma/screening_log.csv`, not as full bibliographic records.

---

## External data and attribution

| Data | Source | Licence | In repo? |
|---|---|---|---|
| District (ADM2) and division (ADM1) boundaries | geoBoundaries (Runfola et al., 2020) | CC BY 4.0 | Yes, the exact files used; `scripts/fetch_external_data.py` re-downloads the current release |
| Population by division, Census 2022 | Bangladesh Bureau of Statistics (BBS, 2023) | Public statistics | Yes, `data/external/bbs_census2022_division_population.csv` |
| Major named river courses | © OpenStreetMap contributors | ODbL 1.0 | `<Yes/No>`; if included, this file stays under ODbL, not CC BY |

---

## Licence

- **Code** (`notebooks/`, `scripts/`, `Makefile`): [MIT](LICENSE).
- **Data** (`data/`, `prisma/`, `reliability/`, `outputs/`): [CC BY 4.0](LICENSE-DATA.md), except the third-party files listed above, which keep their original licences.

---

## How to cite


---

## Contact

Md Mahbubur Rahman, Aerospace Information Research Institute, Chinese Academy of Sciences: `<email>`.
Questions and corrections are welcome as [GitHub issues](https://github.com/<your-username>/bd-geoai-disaster-review/issues).
