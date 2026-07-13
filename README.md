# Detecting Operational Inefficiency in a Telecom Call Center

### Statistical identification of underperforming operators using Python · Hypothesis Testing · Feature Engineering

---

## About the Project

Traditional call center analyses usually rely on descriptive KPIs such as average waiting time, abandoned calls, or outbound volume. While useful, these metrics alone do not distinguish between normal operational variation and statistically significant underperformance.

This project develops a statistical framework to identify inefficient operators by comparing each employee exclusively against their own team. Rather than ranking employees globally, the methodology preserves each client's operational context and highlights operators whose performance deviates significantly from their peers.

The final outcome is a prioritization model that distinguishes isolated operator issues from structural problems affecting entire client companies.

---

## Business Questions

The analysis is structured around three operational questions:

| # | Question                                                                    | Business Objective                       |
| - | --------------------------------------------------------------------------- | ---------------------------------------- |
| 1 | Which inbound operators lose significantly more calls than their teammates? | Detect abnormal missed-call behaviour    |
| 2 | Which operators keep customers waiting significantly longer?                | Identify customer experience bottlenecks |
| 3 | Which outbound operators show abnormally low productivity?                  | Detect inefficient outbound performance  |

---

## Key Findings

### Analysis 1 — Missed Call Rate

Operators were evaluated using a Two-Proportion Z-Test comparing their answered and missed calls against the remainder of their team.

Only operators who simultaneously satisfied all business rules were flagged:

* Minimum workload threshold
* Predominantly inbound activity
* Statistically significant difference (p < 0.05)
* Missed-call rate above the team's 85th percentile

This approach minimizes false positives caused by low call volume.

---

### Analysis 2 — Waiting Time

Waiting times were compared within each operational team using non-parametric statistical tests after validating distribution characteristics.

The analysis identified operators whose waiting times were consistently above their teammates, indicating operational inefficiency rather than random variation.

---

### Analysis 3 — Outbound Productivity

Outbound performance was evaluated through completed call volume while controlling for team context.

Operators with statistically lower productivity than their peers were identified after applying minimum workload filters to eliminate unreliable comparisons.

---

### Global Conclusions

The most important finding was not the number of inefficient operators, but their distribution.

No operator showed poor performance across multiple KPIs simultaneously.

Instead, different inefficiency profiles emerged:

* High missed-call operators
* High waiting-time operators
* Low outbound-productivity operators

At the company level, several client organizations concentrated multiple flagged operators, suggesting structural management or operational issues rather than isolated employee performance.

This shifts the focus from individual correction toward organizational intervention.

---

## Technical Stack

| Tool        | Purpose                               |
| ----------- | ------------------------------------- |
| Python      | Complete analytical workflow          |
| Pandas      | Data cleaning and feature engineering |
| NumPy       | Numerical computation                 |
| SciPy       | Statistical hypothesis testing        |
| Statsmodels | Two-Proportion Z-Test                 |
| Matplotlib  | Data visualization                    |
| Seaborn     | Exploratory analysis                  |

---

## Project Structure

```text
telecom-call-center-analysis/

├── data/
│   ├── telecom_dataset.csv
│   └── clients_dataset.csv
│
├── notebooks/
│   └── analysis.ipynb
│
├── outputs/
│   ├── figures/
│   └── flagged_operators.csv
│
└── README.md
```

---

## Methodological Decisions

### Team-based comparison

Operators are never compared across different companies.

Each employee is evaluated only against teammates serving the same client, ensuring comparable operational conditions.

---

### Minimum workload filter

Very low-volume operators are excluded because small samples generate unstable statistical estimates.

The minimum threshold combines:

* Local first quartile (P25) of team volume
* Special rule for two-person teams
* Global floor defined from exploratory analysis

---

### Inbound dominance

Only operators with at least **66% inbound calls** are evaluated in the missed-call analysis.

This prevents outbound-focused operators from being incorrectly classified.

---

### Statistical validation

Performance differences are only considered meaningful when supported by statistical evidence.

Descriptive KPIs alone are insufficient to classify operator inefficiency.

---

## Statistical Tests

| Analysis                | Statistical Test      | Purpose                                 |
| ----------------------- | --------------------- | --------------------------------------- |
| Missed Call Rate        | Two-Proportion Z-Test | Compare operator failure rate with team |
| Waiting Time            | Mann–Whitney U Test   | Compare waiting-time distributions      |
| Outbound Productivity   | Mann–Whitney U Test   | Compare outbound productivity           |
| Distribution Assessment | Shapiro–Wilk Test     | Evaluate normality assumptions          |

---

## Dataset

**Domain:** Telecom / Call Center Operations

**Main tables**

* Telecom interactions
* Client information

**Variables analysed**

* Inbound and outbound calls
* Missed calls
* Waiting time
* Operator ID
* Client company
* Call direction
* Call volume

---

## Business Impact

This methodology enables managers to:

* Detect statistically significant underperformance instead of relying solely on raw KPIs.
* Reduce false positives by incorporating workload thresholds.
* Separate individual performance issues from structural organizational problems.
* Prioritize interventions at the company level when multiple operators exhibit independent inefficiency patterns.

The framework is designed to be scalable and can be applied to any call center where operators work within identifiable teams.

---

## Author

**Brian Rodrigues**

Junior Data Analyst | Python • SQL • Statistics • Business Analytics

* LinkedIn: https://linkedin.com/in/rodriguesbrian
* GitHub: https://github.com/rodriguesbrian

