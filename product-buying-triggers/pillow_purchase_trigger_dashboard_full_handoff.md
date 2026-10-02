# Side-Sleeper Pillow — Purchase Trigger Dashboard
## Full project handoff / working notes

> **Writing principle:** Keep this clear, useful, and human. The underlying data can be complex. The story should not be.

---

# 1. The project

### The business question

The goal is to build a dashboard for the CEO of a pillow company.

The product is a **side-sleeper pillow designed for anyone**, with particular relevance to:

- women going through menopause
- women in the postpartum period
- people experiencing sleep disruption, pain, discomfort, or other problems that could make a new pillow relevant

The dashboard should help the CEO understand:

> **What triggers someone to buy a side-sleeper pillow?**

More specifically:

> **What sequence of events moves someone from “I have a sleep problem” to “I need to buy a pillow”?**

The dashboard should focus on **“why now?”**, not just **“who is the customer?”**

---

# 2. An important distinction

Public data will generally **not** prove that a particular event caused someone to buy a pillow.

For example, we should not conclude:

> “Postpartum depression causes women to buy side-sleeper pillows.”

That is not what the data can establish.

Instead, the dashboard should **triangulate evidence** from several sources:

- population data
- health data
- sleep data
- life-stage data
- spending data
- search behavior
- consumer reviews and language

The goal is to build a clear chain of evidence:

> **Who has the problem → what the problem is → what changed → how severe it is → whether they can buy → what they search for → why they might buy now**

Keep **facts, derived metrics, and hypotheses separate**.

---

# 3. The trigger framework

These are the main possible purchase-trigger categories.

## 1. Pain / physical discomfort

Examples:

- Neck pain
- Shoulder pain
- Hip pain
- Back pain
- Pressure or alignment problems

Possible question:

> Is physical discomfort creating a reason to replace or upgrade a pillow?

---

## 2. Sleep disruption

Examples:

- Short sleep
- Poor sleep
- Trouble staying asleep
- Trouble falling asleep
- Insomnia-related problems
- Repeated waking

Possible question:

> Does worsening or persistent sleep disruption create a “why now?” moment?

---

## 3. Temperature

Examples:

- Hot flashes
- Night sweats
- Overheating
- Looking for cooling products

Possible question:

> Does nighttime heat create a need for a different pillow?

---

## 4. Life transitions

Examples:

- Menopause
- Postpartum period
- New baby
- Caregiving
- Aging
- Changes in work or household responsibilities

Possible question:

> Does a life-stage change create a new sleep problem or a new reason to invest in comfort?

---

## 5. Failed solutions

Examples:

- Previous pillow did not work
- Multiple pillows tried
- Pillow loses shape
- Pillow is too firm or too soft
- Pillow does not provide enough support

Possible question:

> Does failure of an existing solution create the purchase moment?

This is especially useful to investigate through reviews and consumer language.

---

## 6. Comfort and support

Examples:

- Better neck support
- Better alignment
- Pressure relief
- General comfort
- Better side-sleeping support

Possible question:

> Is the purchase driven by optimization rather than a severe problem?

---

## 7. Prevention / optimization

Examples:

- Improving sleep before a problem becomes severe
- Investing in better sleep
- Replacing an old pillow proactively

Possible question:

> Are some purchases motivated by prevention rather than a crisis?

---

# 4. The core questions the dashboard should answer

### WHO?

- Who experiences the relevant problems?
- Where do they live?
- What are their ages?
- What are their household characteristics?
- What is their income?
- Are there children in the household?
- Are there recent births?
- What life stage are they in?

### WHAT PROBLEM?

- What sleep problems exist?
- What physical discomfort exists?
- How common is pain?
- How common is short sleep?
- How common is poor sleep?
- What menopause-related symptoms occur?
- What postpartum challenges occur?

### WHAT LIFE EVENT?

- Is the person going through menopause?
- Did they recently have a baby?
- Do they have young children?
- Are they caregiving?
- Are work or childcare demands affecting sleep?

### HOW SEVERE?

- How often does the problem occur?
- How many poor-health days are reported?
- How short is sleep?
- How persistent are symptoms?
- Is the problem increasing?

### CAN / WILL THEY BUY?

- What is household income?
- What are spending patterns?
- How much discretionary spending exists?
- What household costs compete with the purchase?

### WHAT DO THEY SEARCH FOR?

- What problems are people actively searching for?
- Which searches are growing?
- Where are those searches happening?
- What searches appear before product searches?

### WHAT DO REAL CUSTOMERS SAY?

- Why did someone buy?
- What problem were they trying to solve?
- What had they already tried?
- Why did the old pillow fail?
- What language do they use to describe the problem?

### WHY NOW?

The most important CEO question:

> **What sequence of events creates the purchase moment?**

---

# 5. Recommended data sources

## A. U.S. Census / American Community Survey (ACS)

### What it is for

Use Census/ACS to understand:

- who lives where
- household structure
- life stage
- income
- geography
- purchasing capacity

Census is mainly the **“who and where?”** layer.

### Fields to consider

- Age
- Sex
- Race/ethnicity
- State
- County
- Census tract
- ZIP Code Tabulation Area (ZCTA), where appropriate
- Median household income
- Income distribution
- Poverty status
- Employment status
- Occupation
- Education
- Marital status
- Household type
- Presence of children
- Number of children
- Age of children
- Birth in previous 12 months
- Fertility
- Housing tenure
- Home value
- Housing costs
- Disability
- Internet/broadband access
- Vehicle access
- Population density
- Population growth
- Migration

### Specific ACS variables used in the prototype

```text
B01001_001E = Total population
B01001_002E = Male population
B01001_026E = Female population
B19013_001E = Median household income
B11005_001E = Households
B11005_003E = Households with children under 18
```

ACS subject table **S1301** is useful for fertility and recent births.

### Access

- Census API is the recommended approach.
- The current Census API requires a key.

### Historical data

- ACS 1-year data goes back roughly to 2005.
- ACS 5-year data reaches farther back.

### Why it matters

Census can help answer:

> “Where is the potential market, and what does that market look like?”

---

# 6. CDC Behavioral Risk Factor Surveillance System (BRFSS)

## What it is for

BRFSS helps measure:

- health problems
- sleep problems
- physical discomfort
- mental-health context
- demographics
- geography

This is primarily the **“what problem?”** layer.

### Fields to consider

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
- Arthritis
- Chronic conditions
- BMI
- Physical activity
- Health insurance/access
- Disability
- State
- Survey year

### Variables used in the prototype

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

### Useful derived variables

```text
frequent_poor_physical_health = PHYSHLTH >= 14

frequent_poor_mental_health = MENTHLTH >= 14

short_sleep = SLEPTIM1 < 7

very_short_sleep = SLEPTIM1 < 6
```

These are useful working definitions, not clinical diagnoses.

### Historical data

BRFSS data goes back to **1984**.

### Access

BRFSS is primarily distributed as annual files.

It is not an API-first source.

### Important technical caution

BRFSS variables and modules can change from year to year.

Before production ingestion:

1. Check the codebook.
2. Confirm that each variable exists.
3. Confirm its meaning.
4. Confirm its valid values.
5. Confirm how missing values are coded.
6. Version the configuration by year.

### Important statistical caution

Do **not** join individual BRFSS records directly to Census people.

Instead, aggregate BRFSS into shared dimensions such as:

- Year
- State
- Age band
- Sex
- Other appropriate demographic groups

Then compare those aggregated estimates with Census population data.

BRFSS is a complex survey, so weighted estimates should be used where appropriate.

### Why it matters

BRFSS can help answer:

> “Where and among whom are the relevant health and sleep problems concentrated?”

---

# 7. Study of Women's Health Across the Nation (SWAN)

## What it is for

SWAN is useful for understanding menopause-specific symptoms over time.

### Fields to consider

- Age
- Menopausal status/stage
- Menstrual status
- Hot flashes
- Night sweats
- Vasomotor symptoms
- Sleep quality
- Sleep disturbance
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

### How to use it

Use SWAN to understand:

> “What happens to sleep and symptoms during the menopause transition?”

Do not treat it as a general U.S. population time series.

It is specialized longitudinal research data.

### Access

Public-use research files are available.

It is a file-download source rather than a simple API source.

---

# 8. CDC Pregnancy Risk Assessment Monitoring System (PRAMS)

## What it is for

PRAMS helps understand postpartum and maternal circumstances.

### Fields to consider

- Maternal age
- Race/ethnicity
- Education
- Income-related variables, where available
- Marital status
- Employment
- Number of previous births
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
- Social support
- Partner/family support

### Important caution

Data availability varies by:

- state
- year
- variable

Historical data exists, but variables are not uniform across all years and jurisdictions.

### Access

Usually through a data portal or downloadable files.

There is no simple public API comparable to the Census API.

### Why it matters

PRAMS can help answer:

> “What does the postpartum life stage look like when it comes to sleep, health, stress, and daily demands?”

---

# 9. AHRQ Medical Expenditure Panel Survey (MEPS)

## What it is for

MEPS can connect sleep problems with:

- health
- healthcare use
- healthcare spending
- chronic conditions
- pain
- medication
- insurance

### Fields to consider

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

One useful recent sleep variable is:

```text
ADSLEEP42
```

It measures frequency of trouble sleeping.

### Historical data

Public-use MEPS data goes back to **1996**.

Current files extend through **2024**.

### Access

Public-use downloadable files.

Not an API-first source.

### Why it matters

MEPS can help answer:

> “How much health burden and healthcare use is associated with the problem?”

---

# 10. Bureau of Labor Statistics — American Time Use Survey (ATUS)

## What it is for

ATUS helps understand:

- sleep
- work
- childcare
- caregiving
- time pressure

### Fields to consider

- Total sleep time
- Time of day of sleep
- Work hours
- Employment status
- Childcare
- Primary childcare
- Secondary childcare
- Household activities
- Personal care
- Leisure
- Caregiving
- Age
- Sex
- Presence of children
- Age of children
- Employment
- Earnings
- Household composition

### Historical data

ATUS data runs from **2003 through 2025**.

### Access

Microdata is downloadable.

No BLS API is required for the microdata.

### Why it matters

ATUS can help answer:

> “Are sleep problems connected to work, childcare, or caregiving demands?”

---

# 11. BLS Consumer Expenditure Survey (CE)

## What it is for

Consumer Expenditure data helps understand:

- spending behavior
- household income
- purchasing capacity
- discretionary spending

### Fields to consider

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

### Access

Microdata is available through downloadable files.

The BLS API is more useful for published time series than for microdata.

### Why it matters

Consumer Expenditure data can help answer:

> “Can this group reasonably support the purchase?”

---

# 12. Google Trends

## What it is for

Google Trends adds something the survey datasets cannot:

**actual search behavior.**

It can help identify the sequence:

> **symptom → problem → solution → product**

### Product/problem searches

```text
side sleeper pillow
pillow for side sleepers
best pillow for side sleepers
side sleeping shoulder pain
side sleeping hip pain
side sleeping neck pain
```

### Menopause searches

```text
menopause sleep
menopause insomnia
hot flashes at night
night sweats sleep
cooling pillow
menopause pillow
```

### Postpartum searches

```text
postpartum sleep
postpartum insomnia
postpartum pillow
new mom sleep
breastfeeding sleep
postpartum shoulder pain
postpartum back pain
```

### Analyze

- Relative search interest
- Year-over-year change
- Seasonality
- Geography
- Related searches
- Search sequences

### Important distinction

Keep Google Trends separate from population statistics.

Search behavior tells us:

> “What are people actively looking for?”

Survey data tells us:

> “What problems do people report?”

Those are related but different signals.

### Access / technical note

Treat Google Trends as a separate behavioral ingestion source.

Do not make the core architecture dependent on an unofficial API.

---

# 13. Public Product Reviews / Reddit / Public Forums

## What they are for

These sources help us understand **consumer language**.

They are not representative population datasets.

Their value is qualitative.

### Themes to extract

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

### Why this matters

A survey might tell us:

> “Sleep quality is poor.”

A review might tell us:

> “My shoulder hurts every morning and my old pillow collapses during the night.”

The second statement gives the product team language they can actually use.

---

# 14. Source-to-question map

| Question | Best sources |
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
| What might make someone buy now? | Combination of all sources |

---

# 15. Dashboard structure

The dashboard can be organized around six questions.

## WHO

**Sources:** Census / ACS

Show:

- Population
- Age
- Sex
- Income
- Household structure
- Children
- Geography
- Recent births

---

## WHAT PROBLEM

**Sources:** BRFSS / MEPS / SWAN / PRAMS

Show:

- Poor sleep
- Short sleep
- Pain
- Physical health problems
- Mental-health context
- Hot flashes
- Night sweats
- Postpartum challenges

---

## WHAT LIFE EVENT

**Sources:** Census / PRAMS / SWAN / ATUS

Show:

- Menopause
- Recent birth
- Children in household
- Caregiving
- Work demands
- Childcare demands

---

## HOW SEVERE

**Sources:** BRFSS / MEPS / SWAN

Show:

- Frequency
- Duration
- Number of poor-health days
- Sleep duration
- Symptom burden

---

## CAN / WILL THEY BUY?

**Sources:** Census / Consumer Expenditure

Show:

- Income
- Spending
- Housing costs
- Household composition
- Discretionary spending

---

## WHAT DO THEY SEARCH FOR?

**Sources:** Google Trends / reviews / Reddit

Show:

- Search growth
- Search geography
- Related searches
- Consumer vocabulary
- Common purchase reasons

---

# 16. The CEO-facing view: “WHY NOW?”

A useful top-level dashboard could be called:

# WHY NOW?

The CEO should be able to scan something like:

| Trigger | Evidence | Target population | Momentum |
|---|---|---|---|
| Sleep disruption | Short sleep / poor sleep | Relevant age/life-stage groups | Trend |
| Physical discomfort | Pain / poor physical health | Side sleepers / relevant groups | Trend |
| Temperature | Hot flashes / night sweats | Menopause population | Trend |
| Life transition | Birth / menopause / caregiving | Relevant life stages | Trend |
| Failed solution | Review language | Existing pillow users | Trend |
| Purchasing power | Income / spending | Relevant markets | Trend |

### Important

Do not present a trigger as causal unless the evidence supports that conclusion.

A trigger score can be useful as a way to organize evidence, but it should not pretend to be a validated prediction model.

---

# 17. Data pipeline architecture

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

---

# 18. Recommended MVP technology

Start small.

### Use:

- **Python** for ingestion and cleaning
- **Parquet** for raw and processed data
- **DuckDB** for analysis
- **dbt** for transformations, if needed
- A **BI dashboard** for the final presentation

You do **not** need a large database or cloud warehouse for the first version.

A relational database can come later if the project grows.

### If the project scales

A larger architecture could become:

```text
Object Storage
      ↓
BigQuery / Snowflake
      ↓
dbt
      ↓
BI Tool
```

But do not build that first unless there is a real need.

---

# 19. Suggested data model

## Dimensions

```text
dim_geography
dim_age
dim_year
dim_life_stage
dim_trigger
dim_source
```

## Fact tables

```text
fact_population
fact_sleep
fact_health
fact_menopause
fact_postpartum
fact_spending
fact_search
```

## Derived tables

```text
fact_trigger_score
fact_market_size
fact_trigger_momentum
```

---

# 20. Simpler MVP data model

If the full star schema feels like too much at first, start with:

```text
population
health
life_stage
spending
search_trends
```

Use shared dimensions for:

- geography
- year
- age
- sex

Then expand as the dashboard becomes more defined.

---

# 21. Keep raw data separate from interpretation

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

This is important.

For example:

```text
BRFSS reports short sleep
        ↓
standardized metric: short_sleep_rate
        ↓
derived finding: short sleep is concentrated in X group
        ↓
hypothesis: sleep disruption may create a pillow purchase opportunity
```

The last step is a hypothesis, not a fact.

---

# 22. Historical data rules

Keep historical raw files.

Do not overwrite them.

Recommended directories:

```text
data/
  raw/
  processed/
  warehouse/
```

Use versioned source/year files where practical.

For example:

```text
data/raw/brfss/2022/
data/raw/brfss/2023/
data/raw/brfss/2024/
data/raw/brfss/2025/
```

---

# 23. Create a data dictionary

Every source variable should map to a common business concept.

For example:

```text
BRFSS SLEPTIM1
      ↓
sleep_hours
      ↓
sleep disruption
```

Another example:

```text
Census B19013_001E
      ↓
median_household_income
      ↓
purchasing capacity
```

This will make the pipeline much easier to maintain.

---

# 24. API / download summary

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

# 25. First batch: do not download everything

Start with:

1. **2024 Census ACS**
2. **2025 BRFSS**

Get the pipeline working end to end.

Then add historical data.

### Why these two?

Census answers:

> **Who and where?**

BRFSS answers:

> **What health and sleep problems are present?**

Together, they create the first useful version of the market picture.

---

# 26. Prototype Python pipeline

The first prototype script is:

```text
pillow_market_pipeline.py
```

It should:

1. Read `CENSUS_API_KEY` from the environment.
2. Pull 2024 ACS state-level data.
3. Save Census data as Parquet.
4. Download the 2025 BRFSS file.
5. Read the SAS transport file with pandas.
6. Select the required variables.
7. Save BRFSS as Parquet.

## Census fields in the prototype

```text
NAME
B01001_001E
B01001_002E
B01001_026E
B19013_001E
B11005_001E
B11005_003E
```

The Census API endpoint is conceptually:

```text
ACS 1-year
geography: state
year: 2024
```

## BRFSS fields in the prototype

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

## Python dependencies

```text
pandas
requests
pyarrow
```

## Important implementation note

Before running the production pipeline, verify:

- the current CDC download endpoint
- the relevant BRFSS codebook
- variable availability
- variable definitions
- valid values
- missing-value codes

CDC file paths and BRFSS variables can change.

The prototype should therefore become **version-aware** before it becomes a production pipeline.

---

# 27. YAML configuration

A `variables.yml` file should hold source configuration instead of hard-coding everything in Python.

It should include:

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

## Example derived variables

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

## Example trigger categories

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

## Prototype trigger weights

The first YAML version included these placeholder weights:

```text
sleep                 1.0
physical_discomfort   1.0
temperature           1.0
menopause             1.0
postpartum            1.0
mental_health         0.5
purchasing_power      0.75
life_stage            1.0
```

### Important

These are **placeholders**.

They are not validated causal weights.

Do not present them to the CEO as scientifically established weights.

## Data-quality rules

The prototype included BRFSS missing-value codes such as:

```text
7
9
77
99
777
999
```

The production pipeline should also use variable-specific range checks and codebooks rather than relying on one universal missing-value rule.

---

# 28. Production configuration structure

Because BRFSS changes by year, production configuration should eventually be split by source and year.

For example:

```text
variables/
  census.yml
  brfss/
    2022.yml
    2023.yml
    2024.yml
    2025.yml
```

This is safer than assuming one BRFSS configuration works forever.

---

# 29. First data pull: expected time

For the first Census + BRFSS batch, estimate:

# 30–90 minutes of hands-on work

The actual downloads should be relatively fast.

The time goes into:

- setup
- reading documentation
- cleaning
- standardizing
- validation

### Rough breakdown

| Task | Time |
|---|---:|
| Python setup | 5–10 min |
| Census API pull | 1–5 min |
| BRFSS download | 2–10 min |
| Read/convert BRFSS | 5–20 min |
| Clean/standardize | 10–20 min |
| Save Parquet | 1–5 min |
| Validate | 10–20 min |

### Full historical build

A first serious pass across all sources could take roughly:

**Half a day to 1–2 days**

The main challenge is not downloading the files. It is understanding and harmonizing them.

---

# 30. Recommended work order

## Phase 1 — Prove the pipeline

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

## Phase 2 — Add historical data

Add:

- Earlier ACS years
- Earlier BRFSS years

Build year-specific configurations where variables change.

## Phase 3 — Add specialized sources

Add:

- SWAN
- PRAMS
- MEPS
- ATUS
- Consumer Expenditure

## Phase 4 — Add behavioral evidence

Add:

- Google Trends
- Product reviews
- Reddit/public forums

## Phase 5 — Build the trigger layer

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

# 31. What the finished dashboard should help the CEO answer

- Where are the largest groups experiencing relevant sleep problems?
- Which problems are most common?
- Which problems are getting worse or gaining attention?
- Which life stages coincide with sleep disruption?
- Where do menopause-related sleep problems overlap with purchasing capacity?
- What postpartum problems show up alongside sleep disruption?
- What are people searching for when the problem appears?
- What words do actual consumers use when they explain a purchase?
- Which signals suggest a **“why now?”** moment?

---

# 32. What success looks like

The dashboard should not produce a magical number that claims:

> “This person will buy a pillow.”

That is more certainty than the public data can support.

Instead, it should show a **clear chain of evidence**.

For example:

```text
A large population has short sleep
        ↓
A meaningful share reports physical discomfort
        ↓
The problem is concentrated in a particular life stage
        ↓
Searches for the problem are increasing
        ↓
People describe buying pillows to solve that problem
        ↓
The relevant population has purchasing capacity
        ↓
Potential "WHY NOW?" signal
```

That is much more useful than a black-box score.

---

# 33. The guiding principle

Keep the dashboard simple enough that a CEO can understand it in a few minutes.

The underlying data can be complex.

The story should not be.

## The final story

**Who has the problem?**

**What is the problem?**

**What changed?**

**How big is it?**

**How severe is it?**

**Can they buy?**

**What are they looking for?**

**Why now?**

---

# 34. One-page handoff summary

If the coworker only reads one section, read this:

### Goal

Build a **“Why Now?” purchase-trigger dashboard** for a side-sleeper pillow.

### Start with

**Census ACS + BRFSS**

Census = **who and where**

BRFSS = **what health/sleep problems**

### Then add

- SWAN = menopause
- PRAMS = postpartum
- MEPS = sleep + health + spending
- ATUS = sleep + time pressure + caregiving
- Consumer Expenditure = purchasing capacity
- Google Trends = search behavior
- Reviews/Reddit = consumer language

### Architecture

```text
Sources
  ↓
Python ingestion
  ↓
Raw Parquet
  ↓
Clean/standardize
  ↓
DuckDB
  ↓
Derived metrics
  ↓
Trigger layer
  ↓
CEO dashboard
```

### First batch

Pull:

- 2024 ACS
- 2025 BRFSS

Expected hands-on time:

**30–90 minutes**

### Most important rule

Do not confuse:

**data → interpretation → hypothesis**

Keep them separate.

### Most important business question

> **What sequence of events makes someone decide they need a new pillow now?**
