# Pillow Purchase Trigger Dashboard

## Goal

Build a dashboard that helps the CEO understand **why someone buys a side-sleeper pillow now**.

The product is designed for anyone who sleeps on their side, with particular relevance to women going through menopause and women in the postpartum period.

The dashboard should not stop at demographics. It should connect:

> **Who has the problem → what the problem is → what changed → how severe it is → whether they can buy → what they search for**

A useful framing is:

**What sequence of events moves someone from “I have a sleep problem” to “I need to buy a pillow”?**

Public data will not prove that a particular event caused a pillow purchase. Instead, we should triangulate population, health, life-stage, spending, search, and consumer-language data.

---

# 1. The Trigger Framework

Focus the dashboard on these possible purchase triggers:

1. **Pain / physical discomfort**
   - Neck
   - Shoulder
   - Hip
   - Back

2. **Sleep disruption**
   - Short sleep
   - Poor sleep
   - Trouble staying asleep
   - Insomnia-related problems

3. **Temperature**
   - Hot flashes
   - Night sweats
   - Overheating

4. **Life transitions**
   - Menopause
   - Postpartum period
   - New baby
   - Caregiving
   - Aging

5. **Failed solutions**
   - Previous pillow did not work
   - Multiple pillows tried
   - Need for better support

6. **Comfort and support**
   - Better alignment
   - Pressure relief
   - General comfort

7. **Prevention / optimization**
   - Improving sleep before a problem becomes severe
   - Investing in better sleep

A key caution: postpartum depression should be treated as a **context or segment**, not assumed to cause pillow purchases.

---

# 2. Recommended Data Sources

## A. U.S. Census / American Community Survey (ACS)

**Purpose:** Understand who lives where, household structure, life stage, and purchasing capacity.

### Useful fields

- Age
- Sex
- Race/ethnicity
- State
- County
- Census tract or ZIP Code Tabulation Area, where appropriate
- Median household income
- Income distribution
- Poverty status
- Employment status
- Occupation
- Education
- Marital status
- Household type
- Presence and number of children
- Age of children
- Birth in previous 12 months
- Housing tenure
- Home value
- Housing costs
- Disability
- Internet/broadband access
- Vehicle access
- Population density
- Population growth/migration

### Prototype variables

- `B01001_001E` — Total population
- `B01001_002E` — Male population
- `B01001_026E` — Female population
- `B19013_001E` — Median household income
- `B11005_001E` — Households
- `B11005_003E` — Households with children under 18

ACS subject table **S1301** is useful for recent births and fertility.

### Technical notes

- Use the Census API.
- Keep historical data.
- ACS 1-year data reaches back to roughly 2005.
- ACS 5-year data reaches farther back.
- The current Census API requires a key.

---

## B. CDC Behavioral Risk Factor Surveillance System (BRFSS)

**Purpose:** Measure health and sleep problems by demographic group and geography.

### Useful fields

- Age
- Sex
- Race/ethnicity
- Income
- Education
- Employment
- Marital status
- General health
- Physical health days
- Mental health days
- Sleep duration
- Arthritis and chronic conditions
- BMI
- Physical activity
- Health insurance/access
- Disability
- State
- Survey year

### Prototype variables

- `_STATE`
- `_AGEG5YR`
- `SEXVAR`
- `INCOME3`
- `EDUCA`
- `EMPLOY1`
- `GENHLTH`
- `PHYSHLTH`
- `MENTHLTH`
- `SLEPTIM1`

### Derived variables

```text
frequent_poor_physical_health = PHYSHLTH >= 14
frequent_poor_mental_health    = MENTHLTH >= 14
short_sleep                    = SLEPTIM1 < 7
very_short_sleep               = SLEPTIM1 < 6
```

### Technical notes

- Historical data goes back to 1984.
- BRFSS is mainly distributed as annual files rather than through a simple API.
- Variable availability can change by year.
- Production ingestion should validate variables against the codebook for each year.

### Important statistical note

Do not join individual BRFSS records directly to Census people.

Instead, aggregate BRFSS into shared dimensions such as:

- Year
- State
- Age band
- Sex
- Other appropriate demographic groups

Then compare those aggregates with Census population data.

BRFSS is a complex survey, so weighted estimates should be used where appropriate.

---

## C. Study of Women's Health Across the Nation (SWAN)

**Purpose:** Understand menopause-specific symptoms over time.

### Useful fields

- Age
- Menopausal status/stage
- Menstrual status
- Hot flashes
- Night sweats
- Vasomotor symptoms
- Sleep quality/disturbance
- Insomnia-related measures
- Mood/depressive symptoms
- Anxiety-related measures
- Physical symptoms
- Pain
- BMI/body composition
- Employment
- Relationship/marital status
- Socioeconomic variables
- Race/ethnicity
- Visit/year

### Technical notes

SWAN is specialized longitudinal research data, not a general population time series.

Use it to understand **menopause-related symptom patterns**, not to estimate the entire U.S. market.

Data is provided through public-use research files rather than a simple API.

---

## D. CDC Pregnancy Risk Assessment Monitoring System (PRAMS)

**Purpose:** Understand postpartum and maternal circumstances.

### Useful fields

- Maternal age
- Race/ethnicity
- Education
- Income-related variables, where available
- Marital status
- Employment
- Previous births
- Recent birth
- Infant characteristics
- Breastfeeding
- Maternal depression/depressive symptoms
- Anxiety/stress, where available
- Maternal health problems
- Pregnancy complications
- Pre-pregnancy BMI
- Postpartum healthcare
- Infant-care behaviors
- Sleep-related variables, where available
- Social, partner, and family support

### Technical notes

- Data availability varies by state, year, and variable.
- Historical data exists, but variables are not uniform across all years and jurisdictions.
- Access is generally through data portals/downloads rather than a simple public API.

Use PRAMS to understand **postpartum life and health context**.

---

## E. AHRQ Medical Expenditure Panel Survey (MEPS)

**Purpose:** Connect sleep problems with health, healthcare use, and spending.

### Useful fields

- Trouble sleeping frequency
- Age
- Sex
- Race/ethnicity
- Income
- Employment
- Education
- Marital status
- Family composition
- Physical health
- Mental health
- Chronic conditions
- Pain/health conditions
- Healthcare utilization
- Prescription medications
- Medical expenditures
- Insurance/access

One useful sleep variable in recent files is `ADSLEEP42`, which measures how often someone has trouble sleeping.

### Technical notes

- Public-use data goes back to 1996.
- Current files extend through 2024.
- Data is distributed as downloadable files rather than an API-first source.

MEPS is useful when the question is:

**How much health burden and healthcare use is associated with the problem?**

---

## F. Bureau of Labor Statistics — American Time Use Survey (ATUS)

**Purpose:** Understand sleep, work, childcare, caregiving, and time pressure.

### Useful fields

- Total sleep time
- Time of day of sleep
- Work hours
- Employment status
- Childcare
- Primary and secondary childcare
- Household activities
- Personal care
- Leisure
- Caregiving
- Age
- Sex
- Presence/age of children
- Employment
- Earnings
- Household composition

### Technical notes

- Historical data runs from 2003 through 2025.
- Microdata is downloadable.
- No BLS API is required for the microdata.

ATUS can help answer questions such as:

**Are sleep problems concentrated in people who also have high childcare, work, or caregiving demands?**

---

## G. BLS Consumer Expenditure Survey (CE)

**Purpose:** Understand spending behavior and purchasing capacity.

### Useful fields

- Household income
- Age/sex of reference person
- Household size
- Number of children
- Marital status
- Education
- Employment
- Housing tenure
- Total expenditures
- Household furnishings/equipment
- Bedding-related categories, where available
- Healthcare
- Personal care
- Apparel
- Recreation
- Other discretionary spending
- Region

### Technical notes

- Long historical series are available.
- Microdata is available by file download.
- The BLS API is more useful for published time series than for microdata.

---

## H. Google Trends

**Purpose:** Capture actual search behavior and identify possible trigger sequences.

This is important because survey data tells us what people report. Search data shows **what they are actively looking for**.

### Example search terms

#### Product/problem

- side sleeper pillow
- pillow for side sleepers
- best pillow for side sleepers
- side sleeping shoulder pain
- side sleeping hip pain
- side sleeping neck pain

#### Menopause

- menopause sleep
- menopause insomnia
- hot flashes at night
- night sweats sleep
- cooling pillow
- menopause pillow

#### Postpartum

- postpartum sleep
- postpartum insomnia
- postpartum pillow
- new mom sleep
- breastfeeding sleep
- postpartum shoulder pain
- postpartum back pain

### Analyze

- Relative search interest
- Year-over-year change
- Seasonality
- Geography
- Related searches
- Search sequence

A useful sequence to look for is:

**symptom → problem → solution → product**

For example:

**shoulder pain → side sleeping pain → better support → side sleeper pillow**

Keep search behavior separate from survey statistics. They answer different questions.

---

## I. Public Product Reviews and Reddit/Public Forums

**Purpose:** Learn the language people use when they explain why they bought a pillow.

These sources are not representative population data. Their value is qualitative.

### Extract themes such as

- Problem mentioned
- Trigger event
- Existing solution
- Why the previous pillow failed
- Desired benefit
- Body location of discomfort
- Sleep position
- Temperature complaint
- Pregnancy/postpartum context
- Menopause context
- Discovery mechanism
- Purchase reason
- Price sensitivity
- Return reason
- Satisfaction
- Exact consumer language

This can help turn abstract survey variables into **real customer language**.

---

# 3. What Each Source Tells the CEO

| Question | Primary sources |
|---|---|
| Who is the market? | Census / ACS |
| Where is the market? | Census / ACS |
| What problems do people have? | BRFSS / MEPS |
| How severe are the problems? | BRFSS / MEPS / SWAN |
| What happens during menopause? | SWAN |
| What happens after childbirth? | PRAMS |
| How much sleep and time pressure exists? | ATUS |
| Can people afford to buy? | Census / Consumer Expenditure |
| What are people searching for? | Google Trends |
| What language do buyers use? | Reviews / Reddit |
| What might make someone buy now? | Combination of all of the above |

---

# 4. Dashboard Structure

A useful CEO-facing dashboard could be organized around:

### WHO

Use Census/ACS.

Show:

- Population
- Age
- Sex
- Income
- Household structure
- Children
- Geography
- Recent births

### WHAT PROBLEM

Use:

- BRFSS
- MEPS
- SWAN
- PRAMS

Show:

- Poor sleep
- Short sleep
- Pain
- Physical health problems
- Mental health context
- Hot flashes/night sweats
- Postpartum challenges

### WHAT LIFE EVENT

Use:

- Census
- PRAMS
- SWAN
- ATUS

Show:

- Menopause
- Recent birth
- Children in household
- Caregiving
- Work/childcare demands

### HOW SEVERE

Use:

- BRFSS
- MEPS
- SWAN

Show:

- Frequency
- Duration
- Number of poor-health days
- Sleep duration
- Symptom burden

### CAN / WILL THEY BUY?

Use:

- Census
- Consumer Expenditure

Show:

- Income
- Spending
- Housing costs
- Household composition
- Discretionary spending

### WHAT DO THEY SEARCH FOR?

Use:

- Google Trends
- Reviews
- Reddit/public forums

Show:

- Search growth
- Search geography
- Related searches
- Consumer vocabulary
- Common purchase reasons

---

# 5. Recommended CEO View

The top of the dashboard could be called:

## WHY NOW?

Each trigger can be summarized as:

| Trigger | Evidence | Target population | Momentum |
|---|---|---|---|
| Sleep disruption | Short sleep / poor sleep | Relevant age/life-stage groups | Trend |
| Physical discomfort | Pain / poor physical health | Side sleepers / relevant groups | Trend |
| Temperature | Hot flashes / night sweats | Menopause population | Trend |
| Life transition | Birth / menopause / caregiving | Relevant life stages | Trend |
| Failed solution | Review language | Existing pillow users | Trend |
| Purchasing power | Income / spending | Relevant markets | Trend |

Avoid presenting a trigger as causal unless the data supports that conclusion.

---

# 6. Data Pipeline

The simplest useful architecture is:

```text
DATA SOURCES
     ↓
API / FILE / WEB INGESTION
     ↓
RAW / BRONZE
     ↓
CLEAN / SILVER
     ↓
GOLD / MARTS
     ↓
SEMANTIC LAYER
     ↓
CEO DASHBOARD
```

## MVP stack

Start small:

- Python for ingestion and cleaning
- Parquet files for raw/processed data
- DuckDB for analysis
- dbt for transformations, if needed
- A BI tool for the dashboard

You do **not** need a large database or cloud warehouse for the first version.

A relational database can come later if the project grows.

---

# 7. Suggested Data Model

### Dimensions

- `dim_geography`
- `dim_age`
- `dim_year`
- `dim_life_stage`
- `dim_trigger`
- `dim_source`

### Fact tables

- `fact_population`
- `fact_sleep`
- `fact_health`
- `fact_menopause`
- `fact_postpartum`
- `fact_spending`
- `fact_search`

### Derived tables

- `fact_trigger_score`
- `fact_market_size`
- `fact_trigger_momentum`

A simpler MVP can use:

- `population`
- `health`
- `life_stage`
- `spending`
- `search_trends`

with shared dimensions for geography, year, age, and sex.

---

# 8. Important Data Rules

## Keep raw data

Never overwrite historical raw files.

Use:

```text
data/raw/
data/processed/
data/warehouse/
```

Keep each source/year versioned.

## Create a data dictionary

Map:

```text
source variable
      ↓
standard variable
      ↓
business concept
```

Example:

```text
BRFSS SLEPTIM1
      ↓
sleep_hours
      ↓
sleep disruption
```

## Separate facts from hypotheses

Use this structure:

```text
source data
    ↓
raw facts
    ↓
standardized facts
    ↓
derived metrics
    ↓
trigger hypotheses
    ↓
CEO view
```

This prevents an interpretation from being mistaken for a measured fact.

---

# 9. API and Download Summary

| Source | Main access method | API needed? |
|---|---|---|
| Census / ACS | API | Yes, recommended |
| BRFSS | Annual download | No |
| SWAN | Research file download | No |
| PRAMS | Portal/download | No simple public API |
| MEPS | Public-use download | No |
| ATUS | BLS microdata download | No |
| Consumer Expenditure | Microdata download | No for microdata |
| Google Trends | Search/trend data | Treat separately |
| Reviews / Reddit | Public web sources | Separate ingestion |

---

# 10. First Batch: What to Pull

Do not start by downloading everything.

Start with:

1. **2024 Census ACS**
2. **2025 BRFSS**

Get the pipeline working end to end before adding historical data.

### Why these two?

Census answers:

**Who and where?**

BRFSS answers:

**What health and sleep problems are present?**

Together, they create the first useful version of the market picture.

---

# 11. First Python Pipeline

The prototype Python script is called:

`pillow_market_pipeline.py`

It should:

1. Read `CENSUS_API_KEY` from the environment.
2. Pull 2024 ACS state-level data.
3. Save Census data as Parquet.
4. Download the 2025 BRFSS file.
5. Read the SAS transport file with pandas.
6. Select the required variables.
7. Save BRFSS as Parquet.

### Census fields in the prototype

```text
NAME
B01001_001E
B01001_002E
B01001_026E
B19013_001E
B11005_001E
B11005_003E
```

### BRFSS fields in the prototype

```text
_STATE
_AGEG5YR
SEXVAR
INCOME3
EDUCA
EMPLOY1
GENHLTH
PHYSHLTH
MENTHLTH
SLEPTIM1
```

### Python dependencies

```text
pandas
requests
pyarrow
```

Before running the production pipeline, verify the current CDC download endpoint and the variables against the relevant year's BRFSS codebook. CDC file paths and variable availability can change.

---

# 12. YAML Configuration

A `variables.yml` file should hold the source configuration instead of hard-coding everything in Python.

The configuration should include:

- Project metadata
- Shared dimensions
- Census variables
- BRFSS variables
- Derived variables
- Trigger categories
- Data-quality rules
- Range checks
- Refresh metadata
- Pipeline paths
- Historical-data rules

Example derived rules:

```yaml
derived:
  frequent_poor_physical_health:
    rule: "PHYSHLTH >= 14"

  frequent_poor_mental_health:
    rule: "MENTHLTH >= 14"

  short_sleep:
    rule: "SLEPTIM1 < 7"

  very_short_sleep:
    rule: "SLEPTIM1 < 6"
```

Example trigger categories:

```yaml
triggers:
  - sleep
  - physical_discomfort
  - temperature
  - menopause
  - postpartum
  - mental_health
  - purchasing_power
  - life_stage
```

The trigger weights in the first YAML version should be treated as **placeholders**. They are not validated causal weights.

For production, split configurations by source and year, especially for BRFSS:

```text
variables/
  census.yml
  brfss/
    2022.yml
    2023.yml
    2024.yml
    2025.yml
```

---

# 13. How Long the First Data Pull Should Take

For the first Census + BRFSS batch, plan for roughly:

**30–90 minutes of hands-on work.**

The downloads themselves should be relatively quick. Most of the time goes into setup, cleaning, and validation.

Approximate breakdown:

| Task | Time |
|---|---:|
| Python setup | 5–10 min |
| Census API pull | 1–5 min |
| BRFSS download | 2–10 min |
| Read/convert BRFSS | 5–20 min |
| Clean/standardize | 10–20 min |
| Save Parquet | 1–5 min |
| Validate | 10–20 min |

A full historical build across all sources is a different project. Expect roughly **half a day to two days** for the first serious pass, depending on how much historical harmonization is needed.

The first goal should be an end-to-end working pipeline, not a perfect historical dataset.

---

# 14. Recommended Work Order

### Phase 1 — Prove the pipeline

Pull:

- 2024 ACS
- 2025 BRFSS

Then:

- Validate the files
- Check missing values
- Check ranges
- Confirm geography
- Confirm age/sex dimensions
- Save Parquet
- Create a simple DuckDB model

### Phase 2 — Add historical data

Add:

- Earlier ACS years
- Earlier BRFSS years

Build year-specific configurations where variables change.

### Phase 3 — Add specialized sources

Add:

- SWAN
- PRAMS
- MEPS
- ATUS
- Consumer Expenditure

### Phase 4 — Add behavioral evidence

Add:

- Google Trends
- Product reviews
- Reddit/public forums

### Phase 5 — Build the trigger layer

Calculate:

- Market size
- Problem prevalence
- Symptom severity
- Life-stage concentration
- Purchasing capacity
- Search momentum
- Consumer-language themes

Then combine those into a **trigger view**.

---

# 15. What Success Looks Like

The finished dashboard should help the CEO answer questions like:

- Where are the largest groups experiencing relevant sleep problems?
- Which problems are most common?
- Which problems are getting worse or gaining attention?
- Which life stages coincide with sleep disruption?
- Where do menopause-related sleep problems overlap with purchasing capacity?
- What postpartum problems show up alongside sleep disruption?
- What are people searching for when the problem appears?
- What words do actual consumers use when they explain a purchase?
- Which signals suggest a **“why now?”** moment?

The goal is not to manufacture a single score that claims to predict purchases.

The goal is to build a **clear chain of evidence** from problem to potential purchase trigger.

---

# 16. Guiding Principle

Keep the dashboard simple enough that a CEO can understand it in a few minutes.

The underlying data can be complex.

The story should not be.

**Who has the problem?  
What is the problem?  
What changed?  
How big is it?  
Can they buy?  
What are they looking for?  
Why now?**
