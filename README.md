# How We Die — Interactive Mortality Charts (1999–2023)

This project builds an interactive set of charts to explore **underlying cause of death** in the United States by **age**, **sex**, and **time period**. 
The goal is to make it easy to answer questions like:

-   Does the leading cause of death change across the lifespan?
-   How do patterns differ for women vs men?
-   How have broad causes shifted over time (1999–2023)?
-   Within a broad cause (e.g., Cancer), which subcategories matter most at different ages or years?

The visualization is designed for exploration: you can filter, isolate causes, zoom, and switch metrics.

------------------------------------------------------------------------

## Charts Overview

### Section 1: By age at death

Two linked charts:

1.  **Broad causes at age of death**\
    Stacked areas show how the *composition* of deaths changes from age 0 to 100.

2.  **Detailed causes at age of death (drill-down)**\
    After selecting a broad cause, this chart shows how *subcategories within that broad cause* vary by age.

### Section 2: By year

Two linked charts:

3.  **Broad causes over time**\
    Stacked areas show how the *composition* of deaths changes over calendar years (1999–2023), optionally filtered to an age range.

4.  **Detailed causes over time (drill-down)**\
    After selecting a broad cause, this chart shows subcategories within that cause over time.

------------------------------------------------------------------------

## Metrics shown

Each chart supports three metrics:

-   **Share of deaths**\
    Proportion of deaths at each x-value (age or year) attributed to each cause.

-   **Deaths (count)**\
    Count of deaths at each x-value.

    -   For “By age” views: shown as **average yearly deaths** within the selected period.

-   **Mortality rate (per 100,000)**\
    Crude rate using population denominators from CDC WONDER (see caveats about older ages).

All metrics are derived from the same underlying death counts; only the y-axis meaning changes.

------------------------------------------------------------------------

## Interactions

-   **Legend click**: isolate a single cause (solo view).\
-   **Shift + legend click**: toggle multiple causes on/off (additive selection).
-   **Show all**: restores all causes with data in the current slice.
-   **Tooltip**: click in the chart to show details for that x-position and hovered layer.
-   **Zoom (slider + inside zoom)**: zoom/pan along the x-axis.
-   **Reset zoom**: returns zoom to the full x-range.

### Normalize to 100%

The **Normalize to 100%** button is available for *Share of deaths* mode.

-   It rescales percentages using **only the causes currently shown**, so the visible areas sum to 100% at each age/year.
-   It **does not** allocate or redistribute deaths from hidden causes; doing so would require **speculative assumptions** about how to assign those deaths.
-   In practice, this mode is useful for comparing the *mix* among a subset of causes, even when their combined share is small.

------------------------------------------------------------------------

## Data sources

### Mortality (deaths)

-   **CDC / NCHS NVSS mortality data (Underlying Cause of Death)**
-   ICD-10 underlying cause codes are used to classify each death into a cause group.

### Population denominators (for rates)

-   **CDC WONDER population estimates** (used to compute crude mortality rates per 100,000)

------------------------------------------------------------------------

## ICD codes

An **ICD code** is a standardized diagnostic classification code from the **International Classification of Diseases (ICD)** maintained by the **World Health Organization (WHO)**. ICD provides a shared language for recording diseases, injuries, and causes of death so that health statistics can be compared over time and across places.

### ICD in this project

This visualization uses **ICD-10** (the 10th revision of ICD).

For mortality data, each death record is assigned an **underlying cause of death**: the single condition or event that initiated the chain of events leading directly to death. That underlying cause is recorded as an ICD-10 code (for example, broad “chapters” like circulatory diseases, or more specific codes for particular conditions).

Using ICD codes allows the analysis to:

- group deaths into consistent cause categories
- keep definitions stable across years
- support comparisons by age, sex, and (in future work) across countries that also use ICD-10 for mortality coding

### Translating ICD into user-friendly categories

ICD labels are designed for clinical/statistical use, not for general audiences. Many categories are technical, overlapping, or hard to interpret without medical context, so this visualization translates ICD ranges into **plain-language “broad causes”** and **readable subcategories**.

This translation involves some subjective interpretation, because the ICD official wording can be unclear or unintuitive to non-specialists and because there is rarely a single right way to group codes for visualization.

**Example (terminology translation):**  
ICD uses terms like **“Diseases of the genitourinary system”** (ICD-10 N00–N99). “Genitourinary” is precise but not a common everyday word, 
and it bundles conditions affecting both urinary and reproductive systems. 

In the visualization, this broad cause is labeled **“Kidneys & urinary”** to make the category understandable to most users, 
while still mapping back to the same underlying ICD-10 code range in the code.

### Adjustments to improve interpretability (category placement)

ICD’s structure is not always aligned with how people think about causes of death. In a few cases, categories were placed to improve interpretability and to match common public-health reporting conventions.

**Example (Alzheimer’s / dementia):**  
Alzheimer’s disease and related dementias appear across ICD codes that can sit in different parts of the ICD hierarchy (e.g., Alzheimer’s disease is coded in the nervous system chapter, while dementia codes can appear elsewhere). For readability, 
the visualization treats **“Dementia & Alzheimer’s”** as a distinct broad cause so users can see these patterns clearly without needing to understand ICD chapter boundaries.

These kinds of decisions are documented in the project code (mapping tables / ranges). The intent is not to redefine ICD, but to create categories that are easier to interpret while remaining faithful to the underlying codes.

## ICD mapping approach

ICD-10 already has a hierarchy (chapters → blocks → categories), 
but the raw codes (and even many official tabulation lists) still produce **too many distinct causes** for an interactive stacked-area chart. 
Showing dozens to hundreds of categories at once becomes visually overwhelming, and many causes would be too small to interpret.

Mapping ICD-10 into a smaller set of **broad causes** and **readable subcategories** makes the visualization:

- readable at a glance (a manageable number of layers)
- stable across time (consistent definitions across years)
- comparable across sex/age slices without hundreds of categories

### What the mapping does

- **Broad causes** are defined as ranges of ICD-10 codes (e.g., circulatory, cancer, respiratory).
- **Detailed causes** are subcategories nested within each broad cause (still based on ICD-10 underlying cause).

In order to avoid presenting too many categories at once:

- smaller/rarer causes are sometimes grouped into broader buckets (including a residual “other” category)
- grouping decisions are informed by the **amount of data** (so categories shown are meaningful on the chart), and by **conceptual similarity** (grouping related conditions together)

These choices involve some subjective judgment about how many categories to show and where to draw boundaries, 
but the guiding principle is to keep the chart interpretable while staying faithful to ICD-based definitions.

### COVID-19

COVID-19 is identified using **ICD-10 U07.1** and appears starting in 2020 in this dataset. In “All years” views, COVID values can look visually smaller because earlier years contribute zeros.

------------------------------------------------------------------------

## Caveats and interpretation notes

-   **Underlying cause only**: each death is assigned one underlying cause. Contributing causes/comorbidities are not shown.
-   **Crude rates**: mortality rates are crude (not age-standardized). Comparisons across time or countries can be influenced by differences in age structure.
-   **Older ages (85+)**: single-year population denominators are available at older ages in WONDER exports; rates are omitted for 85+. (may be updated at a later point with a different data source)
-   **Small counts and volatility**: rare causes (especially at younger ages) can vary substantially year-to-year. 
-   **Registration vs occurrence timing**: this project assumes the year variable is consistent within the derived dataset; if comparing to other sources, confirm whether the year is “year of death” vs “year of registration.”
-   **Broad group definitions**: any ICD grouping involves judgment calls at boundaries and residual categories; the mapping is chosen for readability and stable interpretation.
