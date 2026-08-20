# Tourism service supply chains — data collection and analysis

Notebooks documenting how the review corpus was retrieved and analysed for a study of tourism service supply chains in ten emerging economies (BRICS+).

Analytical base: **51,037 reviews · 11,500 providers · 29 cities · 10 countries**.

## Contents

| File | What it does |
|---|---|
| `data_collection.ipynb` | Retrieval protocol for the Google Places review corpus that produced `reviews_raw.csv`. Runs in `DRY_RUN` mode by default: prints the exact API requests without contacting Google and without incurring cost |
| `tssc_analysis.ipynb` | Cleaning, dictionary-based construct coding with negation handling, logistic models with city fixed effects and supplier-clustered standard errors, robustness checks and figures |
| `data/reviews_analytical_deidentified.csv` | Derived analytical dataset, 51,037 records: coded construct indicators and covariates, hashed review identifiers, pseudonymised provider identifiers. **No review text, no reviewer names** |
| `data/provider_level_deidentified.csv` | Provider-level aggregate, 11,500 providers |
| `data/development_records_excluded.csv` | The 90 records inspected while the dictionaries were written, excluded from the held-out validation pool |
| `requirements.txt` | Package versions of the recorded run |

## Running the analysis

```
pip install -r requirements.txt
```

Open `tssc_analysis.ipynb`, set the raw-export path in the configuration cell and run all cells. Runtime is under a minute.

## Data availability

The collection and analysis notebooks, the frozen coding dictionaries and the derived analytical dataset are openly available here and archived at Zenodo.

**The raw review corpus is not distributed and is not available on request.** Google Maps Platform policies permit indefinite storage of place identifiers only; review text, author names, ratings and related Places content may not be pre-fetched, cached or retained outside the permitted exceptions, and may not be exported for use outside the Services.

What is distributed instead is the derived analytical dataset in `data/`: coded construct indicators and covariates for 51,037 records, with hashed review identifiers and study-specific provider pseudonyms, containing no review text and no reviewer names. Reviewer display names were retrieved by the platform and processed **only** for deduplication; they appear in no file released here.

`data_collection.ipynb` documents the retrieval protocol in full, so an independent researcher with their own API credentials can reconstruct an equivalent corpus. Re-execution will **not** reproduce the same corpus: the platform returns a relevance-ranked subset that changes continuously as users post, edit and delete content.

## Specification

Logistic regression on a binary dissatisfaction outcome (1–2 stars), with city fixed effects and standard errors clustered by provider. The principal exposure is the collection stratum under which each provider was retrieved. That variable is exogenous to the review text, which removes circularity in measurement, but it is **not** exogenous to the outcome: providers select into business models and customers self-select into them.

Constructs are dictionary-based over the Portuguese-language text. `stage_breadth` counts how many chain stages a review mentions; it correlates with review length at *r* = 0.50 and is interpretable only under length adjustment.

## Measurement status

**The coding dictionaries have not been validated against independent human coding.** A held-out, double-coded validation protocol and blinded instruments were prepared, together with the list of records inspected during dictionary development, which are excluded from the held-out pool. That validation had **not been executed** at the time of this release. No precision, sensitivity, specificity, F1 or inter-coder agreement statistics exist.

Construct prevalence is dictionary prevalence, not validated prevalence of the phenomenon.

The collection-stratum labels have likewise not been audited at provider level. They denote the query list that retrieved each provider: exogenous to the review text, which removes circularity in measurement, but **not** exogenous to the outcome.

Two constructs are reported as descriptive null findings rather than measures: service recovery, which co-occurs with a reported disruption in only 44 records, and environmental reference, which appears in 0.47% of reviews with no difference between strata.

## Legacy API notice

The corpus was collected with the Places API (Legacy) endpoints. Google froze that API in March 2025: it still operates for Cloud projects that already had it enabled, but it cannot be enabled in new Cloud projects. Anyone replicating the protocol from scratch needs Places API (New), whose request and response schemas differ.

## Contact

Douglas S. Rodrigues — Production Engineering Department, Fluminense Federal University
[rodriguesdouglas@id.uff.br](mailto:rodriguesdouglas@id.uff.br) · [ORCID 0000-0001-7473-7425](https://orcid.org/0000-0001-7473-7425)
