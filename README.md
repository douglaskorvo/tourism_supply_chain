# Tourism service supply chains — data collection and audited analysis

Notebooks and aggregate reference outputs documenting how a tourism-review corpus was retrieved and analysed for a study of reported disruption exposure and customer impact in tourism service supply chains across ten emerging economies.

Reference analytical base: **51,037 reviews · 11,500 providers · 29 cities · 10 countries**.

## Contents

| File | Purpose |
|---|---|
| `data_collection.ipynb` | Documents the Google Places retrieval protocol. It runs in `DRY_RUN` mode by default, so it prints the requests without contacting Google or incurring API costs |
| `tssc_analysis.ipynb` | Complete audited pipeline: exclusions, frozen dictionaries, descriptive tables, clustered logistic models, Firth checks, FDR, TOST equivalence, robustness, leave-one-out analysis, country heterogeneity and figures |
| `reference_outputs/` | Aggregate reference tables `T0–T12` and the manifest of the frozen analytical run; no review text or reviewer identity is included |
| `requirements.txt` | Package versions recorded for the reference environment |
| `.zenodo.json` and `CITATION.cff` | Deposit and citation metadata |

## Running the analysis

Use Python 3.10 and install the recorded dependencies:

```bash
pip install -r requirements.txt
```

Place a lawfully obtained raw export in the working directory as `reviews_raw.csv`, or set the environment variable `TSSC_RAW_CSV`:

```bash
export TSSC_RAW_CSV=/path/to/reviews_raw.csv
jupyter notebook tssc_analysis.ipynb
```

Run all notebook cells. By default, outputs are written to `output/`. The input schema is:

```text
place_id, country, city, segment, author_name, rating, text, time_desc, lang
```

The frozen reference corpus has SHA-256:

```text
a4d34c3b3f89be548c6345f1a68370674441c7110431498996f1845714746fa0
```

When that input is used, the notebook compares the newly generated tables with `reference_outputs/` and reports whether `T0–T12` pass the numerical and structural checks. A newly retrieved platform corpus is expected to differ and therefore skips the frozen-reference comparison.

## Statistical specification

The primary models are logistic regressions with city fixed effects and supplier-clustered standard errors. Controls comprise log review length, approximate review age and machine-translation status.

The notebook additionally implements:

- BFGS estimation with recorded convergence status;
- Firth logistic regression for the rare exposure outcome and the interaction estimate;
- Benjamini–Hochberg false-discovery-rate correction;
- average marginal effects and two one-sided equivalence tests at ±0.5, ±1 and ±2 percentage points;
- six restricted-sample checks;
- leave-one-out supplier indicators to reduce focal-review same-source overlap;
- country-specific estimates and joint conventional and supplier-clustered tests;
- complete non-fixed-effect coefficient tables for Equations 1–3.

The collection stratum is assigned by the retrieval protocol and is independent of the focal review text. It is not a randomized treatment: providers and customers may select into tourism configurations. Results are therefore associational.

## Data availability and platform constraints

**No review-level data are distributed in this repository.** Google Maps Platform policies restrict redistribution and retention of Places content. Review text, reviewer display names, ratings and related platform content are therefore not deposited or available from this repository.

Reviewer display names were processed only for exact deduplication and appear in no released output. The repository deposits the computational method and aggregate, non-identifying reference results. An independent researcher with appropriate API credentials can reconstruct an equivalent corpus from the documented protocol, but the platform's relevance-ranked subset changes over time, so a new retrieval is not expected to reproduce the frozen corpus exactly.

## Measurement evidence and public-release scope

The dictionaries are frozen in the notebook, with documented corrections for Portuguese lexical polysemy, negation and non-disruptive uses of cancellation vocabulary. Their outputs quantify rule-based mentions in the retrieved review text, not objective incident incidence, contractual governance, observed recovery capacity or environmental performance.

Individual human-coding records, supplier-audit records and their validation metrics do not form part of this public release. Consequently, this repository neither recalculates nor infers those metrics. This statement defines the scope of the public deposit and does not characterize the status of any separately documented validation activity.

Service-recovery and environmental-language indicators are retained as descriptive boundary findings because recovery language co-occurs with only 44 reported disruptions and environmental language occurs in 0.47% of the reference corpus.

## Legacy API notice

The corpus was collected with Places API (Legacy) endpoints. Google froze the legacy service for new Cloud projects in March 2025. New replications may require Places API (New), whose request and response schemas differ; `data_collection.ipynb` preserves the historical protocol and runs without issuing requests unless explicitly configured.

## Contact

Douglas De Souza Rodrigues — Production Engineering Department, Fluminense Federal University  
[rodriguesdouglas@id.uff.br](mailto:rodriguesdouglas@id.uff.br) · [ORCID 0000-0001-7473-7425](https://orcid.org/0000-0001-7473-7425)

