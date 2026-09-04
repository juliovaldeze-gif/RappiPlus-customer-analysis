# RappiPlus Customer Analysis

## From Data to Business Decisions

RappiPlus is a subscription service within the Rappi ecosystem designed to increase purchase frequency and the value generated per user.

This project evaluates whether the service is achieving that objective by combining **Python, SQL, statistical analysis, cohort analysis, conversion funnel analysis, and Power BI** to transform raw customer data into actionable business insights.

The analysis focuses on three core business questions:

- Are users purchasing more?
- Is the business model generating profit?
- Where are conversion and retention opportunities being lost?

The project follows an end-to-end analytics workflow, from data preparation and validation to profitability analysis, user behavior, experimentation, and executive visualization.

---

## Business Objectives

The analysis was designed to help stakeholders understand:

- Overall business profitability
- Revenue, costs, and profit by segment
- Customer behavior throughout the conversion funnel
- Major user drop-off points
- Customer retention over time
- The impact of changes introduced through A/B testing
- Key insights that can support business decisions

---

## Analytics Workflow

Each stage builds on the previous one and answers a different business question.

| Step | Business Question | Method | Main Output |
|---|---|---|---|
| 1 | Can we trust the data? | Python data cleaning and validation | Clean datasets |
| 2 | Is the business profitable? | Profitability and KPI analysis | Revenue, cost, and profit metrics |
| 3 | Where are users dropping off? | SQL conversion funnel | Funnel and drop-off analysis |
| 4 | Are users coming back? | SQL cohort analysis | Retention insights |
| 5 | Do product changes improve results? | Python A/B testing | Statistical evaluation |
| 6 | How do we communicate the findings? | Power BI | Executive dashboard |

---

# 1. Data Preparation & Quality Analysis

The first stage evaluates the reliability of the available data before performing business analysis.

### Objectives

- Evaluate data quality
- Detect inconsistencies
- Identify missing or duplicated information
- Clean and structure the datasets
- Generate analysis-ready datasets

### Main Data Sources

The project begins with three primary datasets:

```text
datasets/
├── rappiplus_orders_raw.csv
├── rappiplus_catalog.csv
└── rappiplus_marketing_spend.csv
```

### `rappiplus_orders_raw.csv`

Each row represents an order placed on the platform.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `id_pedido` | Categorical | Unique order identifier | `order_0` |
| `id_usuario` | Categorical | Identifier of the user who placed the order | `user_6993` |
| `fecha_hora_pedido` | Date | Date when the order was placed | `2025-05-22` |
| `pais` | Categorical | Country where the order was placed | Argentina |
| `dispositivo` | Categorical | Device used to place the order | Desktop |
| `fuente_referencia` | Categorical | User acquisition channel | Organic |
| `nombre_producto` | Categorical | Name of the purchased product | `Jacket-Winter-M` |
| `categoria_producto` | Categorical | Product category | Fashion |
| `cantidad` | Numeric | Number of products purchased | `2` |
| `precio_unitario` | Numeric | Price per unit | `332.69` |
| `monto_descuento` | Numeric | Discount applied to the order | `0` |
| `monto_total` | Numeric | Total amount paid | `665.37` |

### `rappiplus_catalog.csv`

Each row represents a product available on the platform.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `nombre_producto` | Categorical | Product name | `Laptop-Gaming-16GB` |
| `categoria_producto` | Categorical | Product category | Electronics |
| `costo_unitario` | Numeric | Unit cost of the product | `280.68` |
| `proveedor` | Categorical | Product supplier | Fuller, Pena and Myers |

### `rappiplus_marketing_spend.csv`

Each row represents marketing investment for a specific country and channel.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `fecha` | Date | Date of the marketing investment | `2025-01-01` |
| `pais` | Categorical | Country where the campaign was executed | Mexico |
| `id_campaña` | Categorical | Unique campaign identifier | `organic_Mexico` |
| `canal` | Categorical | Marketing channel | Organic |
| `gasto` | Numeric | Campaign investment | `2446.25` |

> **Dataset note:** Column names remain in their original form to preserve compatibility with the notebooks, queries, and source files. Business documentation is presented in English.

---

# 2. Business Profitability Analysis with Python

This stage evaluates the financial performance of the business.

### Analysis

- Revenue calculation
- Cost calculation
- Profit calculation
- KPI development
- Identification of profitable and underperforming segments
- Comparison across relevant business dimensions

### Core KPIs

- **Revenue**
- **Cost**
- **Profit**
- **Average Order Value**
- **Profit Margin**

The cleaned datasets generated during the data-preparation stage are used as the primary sources for this analysis.

---

# 3. Conversion Funnel Analysis with SQL

This stage analyzes the customer journey through the platform and identifies where users abandon the purchasing process.

### Objectives

- Analyze the complete user journey
- Build the conversion funnel
- Calculate conversion between funnel stages
- Detect abandonment points
- Identify the largest drop-off

### Data Source

The analysis uses the `events` table stored in a database and accessed from the Jupyter Notebook.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `id_usuario` | Categorical | Unique user identifier | `user_6772` |
| `id_sesion` | Categorical | Unique session identifier | `6a97f2af-32ae-4186-8c92-04025be1a27b` |
| `nombre_evento` | Categorical | User event type | `first_visit` |
| `timestamp_evento` | Date | Date when the event occurred | `2025-05-17` |
| `pais` | Categorical | User country | Colombia |
| `dispositivo` | Categorical | Device used | Desktop |
| `fuente_referencia` | Categorical | Acquisition channel | Organic |
| `categoria_producto` | Categorical | Product category associated with the event | Fashion |

SQL is used to transform event-level information into business metrics that reveal customer progression and abandonment throughout the purchasing journey.

---

# 4. Cohort Retention Analysis with SQL

This stage measures whether customers continue interacting with the platform after registration.

### Objectives

- Analyze user behavior over time
- Build registration cohorts
- Measure retention by cohort
- Identify changes in customer engagement
- Generate actionable retention insights

### `users`

Each row represents a registered user.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `id_usuario` | Categorical | Unique user identifier | `user_0` |
| `fecha_registro` | Date | User registration date | `2025-01-29` |
| `pais` | Categorical | User country | Mexico |
| `dispositivo` | Categorical | Device used during registration | Mobile |
| `tipo_plan` | Categorical | User plan type | `free` |

### `user_activity`

Each row represents user activity after registration.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `id_usuario` | Categorical | Unique user identifier | `user_0` |
| `fecha_actividad` | Date | Date of user activity | `2025-02-05` |
| `dias_despues_registro` | Numeric | Days elapsed since registration | `7` |
| `activo` | Binary Numeric | Activity indicator: `1` = active, `0` = inactive | `0` |

The resulting cohort analysis provides a structured view of how engagement evolves after acquisition.

---

# 5. A/B Testing & Impact Evaluation with Python

This stage evaluates whether a product or interface change produces a statistically meaningful improvement in user behavior.

### Objectives

- Compare control and treatment groups
- Measure conversion performance
- Evaluate experimental results
- Apply statistical testing
- Translate the results into a business recommendation

### Data Source

`datasets/experiment_checkout_ui.csv`

Each row represents a user's participation in the A/B experiment.

| Column | Data Type | Description | Example |
|---|---|---|---|
| `id_usuario` | Categorical | Unique experiment user identifier | `exp_user_0` |
| `variante` | Categorical | Assigned experimental variant | `tratamiento` |
| `convirtio` | Binary Numeric | Conversion indicator: `1` = converted, `0` = did not convert | `0` |
| `dispositivo` | Categorical | User device | `mobile` |
| `pais` | Categorical | User country | Argentina |
| `duracion_sesion` | Numeric | Session duration in seconds | `114.41` |
| `timestamp` | Date | Interaction date | `2025-03-28` |

The statistical analysis is used to determine whether observed differences between the groups are likely to represent a real effect rather than random variation.

---

# 6. Power BI Dashboard & Insight Communication

The final stage converts the analytical results into an executive-level visualization.

The dashboard is designed to make the project's main findings accessible to business stakeholders and support data-driven decision-making.

### Dashboard Focus

- Business performance
- Revenue and profitability
- Customer behavior
- Conversion performance
- Marketing performance
- Retention trends
- Key business insights

### Power BI File

```text
DashBoard_RappiPlus.pbix
```

---

# Repository Structure

```text
RappiPlus-customer-analysis/
│
├── datasets/
│   ├── rappiplus_orders_raw.csv
│   ├── rappiplus_catalog.csv
│   ├── rappiplus_marketing_spend.csv
│   └── experiment_checkout_ui.csv
│
├── images/
│
├── DashBoard_RappiPlus.pbix
├── Proyecto_RappiPlus.ipynb
└── README.md
```

---

# Technologies & Skills

### Data Analysis
- Python
- Pandas
- NumPy
- Exploratory Data Analysis
- Data Cleaning
- Data Validation

### SQL
- Data Extraction
- Aggregations
- Conversion Funnel Analysis
- Cohort Analysis
- Retention Analysis

### Statistics
- A/B Testing
- Hypothesis Testing
- Conversion Analysis
- Statistical Interpretation

### Business Intelligence
- Power BI
- KPI Development
- Dashboard Design
- Data Visualization
- Business Insight Communication

### Business Analytics
- Revenue Analysis
- Cost Analysis
- Profitability Analysis
- Customer Behavior
- Funnel Analysis
- Retention Analysis
- Marketing Performance

---

# Professional Skills Demonstrated

This project demonstrates an end-to-end analytical workflow:

**Raw Data → Data Quality → Business KPIs → Customer Funnel → Retention → Experimentation → Dashboard → Business Insights**

It combines technical analysis with business interpretation, demonstrating the ability to transform multiple data sources into insights that support decision-making.

---

# Author

**Julio Cesar Valdez Espinosa**

Senior Data Analyst | Senior Oracle PL/SQL Developer | Technical Lead

Monterrey, Nuevo León, Mexico
