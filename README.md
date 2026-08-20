# Tourism service supply chains — data collection and analysis

Notebooks documenting how the review corpus was retrieved and analysed for a study of tourism service supply chains in ten emerging economies (BRICS+).

Analytical base: **51,037 reviews · 11,500 providers · 29 cities · 10 countries**.

## Contents

| File | What it does |
|---|---|
| `data_collection.ipynb` | Retrieval protocol for the Google Places review corpus that produced `reviews_raw.csv`. Runs in `DRY_RUN` mode by default: prints the exact API requests without contacting Google and without incurring cost |
| `tssc_analysis.ipynb` | Cleaning, dictionary-based construct coding with negation handling, logistic models with city fixed effects and supplier-clustered standard errors, robustness checks and figures |
| `requirements.txt` | Package versions of the recorded run |

## Running the analysis

```
pip install -r requirements.txt
```

Open `tssc_analysis.ipynb`, point the configuration cell at your own raw export and run all cells. Runtime is under a minute.

## Data availability

**No review data is distributed in this repository.** Google Maps Platform policies permit indefinite storage of place identifiers only; review text, author names, ratings and related Places content may not be pre-fetched, cached or retained outside the permitted exceptions, and may not be exported for use outside the Services. The raw corpus is therefore neither redistributed nor available on request. Reviewer display names were retrieved by the platform and processed **only** for deduplication, and appear in no file released here.

What this repository provides is the method. `data_collection.ipynb` documents the retrieval protocol in full and `tssc_analysis.ipynb` contains the complete cleaning, coding and estimation code together with the frozen dictionaries, so an independent researcher with their own API credentials can reconstruct an equivalent corpus and re-run the analysis end to end.

Re-execution will **not** reproduce the same corpus, and the reported estimates are therefore not expected to replicate exactly: the platform returns a relevance-ranked subset that changes continuously as users post, edit and delete content.

## Specification

Logistic regression on a binary dissatisfaction outcome (1–2 stars), with city fixed effects and standard errors clustered by provider. The principal exposure is the collection stratum under which each provider was retrieved. That variable is exogenous to the review text, which removes circularity in measurement, but it is **not** exogenous to the outcome: providers select into business models and customers self-select into them.

Constructs are dictionary-based over the Portuguese-language text. `stage_breadth` counts how many chain stages a review mentions; it correlates with review length at *r* = 0.50 and is interpretable only under length adjustment.

## Measurement status

**The coding dictionaries have not been validated against independent human coding.** A held-out, double-coded validation protocol and blinded instruments were prepared, and the records inspected during dictionary development were recorded so that they could be excluded from the held-out pool. That validation had **not been executed** at the time of this release. No precision, sensitivity, specificity, F1 or inter-coder agreement statistics exist.

Construct prevalence is dictionary prevalence, not validated prevalence of the phenomenon.

The collection-stratum labels have likewise not been audited at provider level. They denote the query list that retrieved each provider: exogenous to the review text, which removes circularity in measurement, but **not** exogenous to the outcome.

Two constructs are reported as descriptive null findings rather than measures: service recovery, which co-occurs with a reported disruption in only 44 records, and environmental reference, which appears in 0.47% of reviews with no difference between strata.

## Legacy API notice

The corpus was collected with the Places API (Legacy) endpoints. Google froze that API in March 2025: it still operates for Cloud projects that already had it enabled, but it cannot be enabled in new Cloud projects. Anyone replicating the protocol from scratch needs Places API (New), whose request and response schemas differ.

## Contact

Douglas S. Rodrigues — Production Engineering Department, Fluminense Federal University
[rodriguesdouglas@id.uff.br](mailto:rodriguesdouglas@id.uff.br) · [ORCID 0000-0001-7473-7425](https://orcid.org/0000-0001-7473-7425)
