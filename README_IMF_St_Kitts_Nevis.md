# Financial Intelligence Pipeline: St. Kitts and Nevis

**Coursework project:** IMF country-report analysis\
**Country and period:** St. Kitts and Nevis, 2023--2024\
**Primary source:** International Monetary Fund (IMF), *St. Kitts and
Nevis: 2024 Article IV Consultation---Press Release and Staff Report*,
IMF Country Report No. 24/126, May 2024.\
**Input:** The supplied 61-page IMF PDF.

## 1. Project overview

This project develops a document-analysis pipeline to extract
macro-financial information from an IMF country report, identify
financial-stability themes, retrieve evidence-grounded passages, and
present findings through visualisations.

The workflow covers:

1.  PDF ingestion and page-level text extraction.
2.  Text cleaning and normalisation.
3.  Overlapping token-based chunking (approximately 400 tokens per
    chunk, with 50-token overlap).
4.  Executive and section-level summarisation using constrained prompts.
5.  Structured extraction of key economic and financial indicators.
6.  Seven-theme keyword analysis and qualitative
    strengths/vulnerabilities.
7.  Dense-vector embeddings and FAISS retrieval for Retrieval-Augmented
    Generation (RAG).
8.  Single-query and multi-query retrieval comparison.
9.  Dashboard visualisations and evaluation.

## 2. Repository structure

``` text
IMF_St_Kitts_Nevis_Project/
├── input/
│   └── 1knaea2024001.pdf
├── output/
│   ├── chunks.json
│   ├── executive_summary.json
│   ├── section_summaries.json
│   ├── financial_indicators.json
│   ├── thematic_distribution.json
│   ├── thematic_distribution.csv
│   ├── economic_strengths.json
│   ├── economic_vulnerabilities.json
│   ├── historical_trends.json
│   ├── historical_trends.csv
│   ├── extracted_clean_text.txt
│   └── prompt_templates.json
├── figures/
│   ├── 01_theme_distribution.png
│   ├── 02_growth_inflation_trends.png
│   ├── 03_indicator_coverage.png
│   └── 04_credit_npl_scatter.png
├── IMF_St_Kitts_Nevis_Analysis.ipynb
├── IMF_St_Kitts_Nevis_Analysis_Report_6pages.pdf
└── README.md
```

*The exact files present may vary depending on which notebook cells have
been executed.*

## 3. Requirements

The notebook is designed for Google Colab or a local Jupyter
environment. It uses:

-   `pypdf` --- PDF text extraction
-   `pandas` and `numpy` --- data processing
-   `matplotlib` and `seaborn` --- visualisation
-   `tiktoken` --- token-aware chunking
-   `sentence-transformers` --- local dense embeddings
-   `faiss-cpu` --- vector similarity search
-   `google-genai` --- optional Gemini generation and evaluation

A working internet connection may be required for package installation
and downloading the embedding model. Live LLM generation and judging
also require a valid API key.

## 4. How to run the notebook

### Google Colab

1.  Upload `IMF_St_Kitts_Nevis_Analysis.ipynb` to Google Colab.
2.  Upload the source IMF PDF when prompted, or place it at the path
    configured in the notebook.
3.  Run the dependency-installation cell.
4.  Run the notebook cells in order, from PDF ingestion through
    visualisation.
5.  To enable Gemini-based summarisation and LLM-as-a-Judge evaluation,
    add your API key securely to the Colab Secrets panel as
    `GEMINI_API_KEY`. Do not hard-code or publish the key.
6.  Run the summarisation and evaluation cells again after configuring
    the key.
7.  Run the embedding, FAISS, baseline RAG and multi-query RAG cells.
    The first embedding run may take time because the Sentence
    Transformers model must be downloaded.
8.  Review retrieved passages and page metadata against the original PDF
    before using generated answers in the final submission.

### Local Jupyter

Create and activate a Python virtual environment, install the packages
listed above, open the notebook, and update the PDF path and API-key
configuration cells for your machine. Execute all cells in order.

## 5. Key source-derived findings

-   Real GDP growth was 3.4% in 2023. The IMF projected 4.7% growth in
    2024.
-   Average consumer-price inflation was 3.6% in 2023; the IMF projected
    2.5% in 2024.
-   Public debt was 54.4% of GDP in 2023.
-   The aggregate banking-sector NPL ratio was 19.4% in 2023.
-   The banking-sector Tier 1 capital ratio was 6.8% in 2023.
-   The IMF narrative separately reported a 39% NPL ratio for the
    systemic bank in 2023 Q2. This is not the same measure as the 19.4%
    aggregate banking-sector ratio.
-   The report does not provide a national unemployment point estimate
    or a country-specific central-bank policy-rate figure in the
    requested form. These fields are retained as `null`, rather than
    estimated or replaced with zero.

**Important:** 2024 and 2025 values in the IMF tables are projections
where indicated; they must not be described as realised outcomes. The
inflation figure above is the period-average consumer-price measure.

## 6. Thematic analysis

The notebook scans the report for keywords and phrases in seven themes:

1.  Inflation and prices
2.  Interest rates and credit
3.  Housing and real estate
4.  Banking capital and liquidity
5.  Sovereign debt
6.  Labour-market dynamics
7.  Systemic financial risk

The counts are lexical mention counts, not semantic classifications or
direct measures of risk severity. Themes may overlap. For example, a
passage about the systemic bank's capital position may be counted under
both banking capital and systemic financial risk.

## 7. Visualisations

The dashboard includes four figures:

-   **Theme distribution:** compares keyword/phrase mentions across the
    seven themes.
-   **Growth and inflation trends:** shows historical values and IMF
    projections for real GDP growth and average consumer-price
    inflation.
-   **Indicator coverage:** shows which of the six requested numerical
    indicators have an explicit source value and which are unavailable
    in the requested form.
-   **Credit growth and NPL scatter plot:** compares annual
    private-sector credit growth with the aggregate banking-sector NPL
    ratio for 2019--2023.

The scatter plot is descriptive and does not establish causation.
Figures are saved at high resolution for inclusion in the report.

## 8. Evaluation approach

-   **Source verification:** key numerical values are checked against
    the IMF report tables and their printed page references.
-   **Structured-output validation:** extracted fields are checked for
    expected names, types and missing-value handling.
-   **LLM-as-a-Judge:** a separate judge prompt assesses factual
    alignment, completeness, consistency and schema compliance. It
    returns scores and supporting comments when the live API call is
    executed.
-   **RAG review:** retrieved passages include source-page metadata and
    should be manually checked against the PDF. Single-query and
    multi-query retrieval can be compared using a human-reviewed
    question set.

A deterministic comparison with manually verified values is not an
LLM-as-a-Judge score. Do not report a live judge score or
retrieval-quality metric unless the corresponding notebook cells have
actually been run and their outputs inspected.

## 9. Limitations and responsible interpretation

-   PDF extraction may flatten tables, footnotes or multi-column
    content; important values should be checked against the original
    page.
-   Keyword counts are approximate and can double-count overlapping
    themes.
-   Some requested indicators are not available in the source in the
    required form; `null` is used to preserve this limitation.
-   The aggregate banking-sector NPL ratio and the systemic bank's NPL
    ratio refer to different scopes and must not be conflated.
-   Forecasts must be distinguished from historical observations.
-   Embedding retrieval can return relevant-looking but incomplete
    context. Retrieved evidence needs source checking.
-   LLM outputs can omit context or introduce unsupported statements.
    Human review remains necessary.
-   The live Gemini generator/judge and FAISS/RAG experiments are only
    complete after the corresponding notebook cells have been
    successfully executed. If no API key is configured, the live LLM
    evaluation cannot be claimed as completed.

## 10. Primary reference

International Monetary Fund (2024). *St. Kitts and Nevis: 2024 Article
IV Consultation---Press Release; and Staff Report*. IMF Country Report
No. 24/126. Washington, DC: International Monetary Fund. May 2024.

Official report:
https://www.imf.org/-/media/files/publications/cr/2024/english/1knaea2024001.pdf

------------------------------------------------------------------------

**Academic integrity note:** This README describes the workflow and
source-derived results. Before submission, execute the notebook, insert
the actual LLM-judge and retrieval-evaluation outputs if required by the
assessment, and ensure that all figures and claims in the report match
the final notebook outputs.
