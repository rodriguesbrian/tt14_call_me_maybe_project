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

## Dataset
 
| File | Description | Rows |
|---|---|---|
| `telecom_dataset_new.csv` | Daily aggregated call logs by operator | 53,902 (raw) |
| `telecom_clients.csv` | Client company metadata (tariff plan, contract start) | 732 |
 
Each row in the telecom dataset represents a **daily aggregate** of calls sharing the same combination of `(user_id, date, direction, internal, operator_id, is_missed_call)` — not an individual call record.
 
**Key columns:**
 
- `user_id` — client company ID
- `operator_id` — operator ID (null for unattributed calls)
- `direction` — `in` (inbound) or `out` (outbound)
- `is_missed_call` — whether calls in the aggregate were missed
- `calls_count` — number of calls in the aggregate
- `call_duration` — total active call time (seconds)
- `total_call_duration` — total call time including waiting (seconds)

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


### Results Summary

| KPI | Operators evaluated | Operators flagged |
|---|---|---|
| Missed Call Rate | 55 | 4 |
| Waiting Time | 65 | 16 |
| Outbound Productivity | 248 | 28 |
| **Total (unique)** | **313** | **48 (~15.3%)** |

Four client companies concentrated multiple flagged operators,
suggesting structural management issues rather than isolated
individual underperformance.

---

## Limitations
 
- Analysis covers a 4-month window (August–November 2019); seasonal effects cannot be ruled out
- 41.8% of inbound companies and 35.6% of outbound companies have a single operator — excluded from evaluation due to absence of a valid reference group
- Companies with very small teams (2 operators) produce less statistically robust comparisons
- `operator_id` null rows (34.2% of all missed calls) are excluded from operator-level analysis, meaning company-level missed rate is underestimated in this framework
- Tariff plan and contract tenure were tested as segmentation variables; no statistically significant effect on KPIs was found
---

## Tech Stack
 
| Tool | Use |
|---|---|
| Python | Data analysis |
| Pandas | Data manipulation |
| NumPy | Numerical computation |
| SciPy | Statistical tests (Mann-Whitney U) |
| Statsmodels | Z-test for proportions |
| Matplotlib / Seaborn | Visualisation |
| Tableau | Executive dashboard |

---

## Repository Structure
 
```
TT14_CALL_ME_MAYBE_PROJECT/
├── data/
│   ├── raw/
│   │   ├── telecom_dataset_new.csv
│   │   └── telecom_clients.csv
│   └── processed/
├── notebook/
│   └── analysis.ipynb
├── reports/
├── .gitignore
├── README.md
└── requirements.txt
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

Only operators with at least 66% inbound calls are evaluated in the missed-call rate and waiting time analyses.

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

## Business Impact

This methodology enables managers to:

* Detect statistically significant underperformance instead of relying solely on raw KPIs.
* Reduce false positives by incorporating workload thresholds.
* Separate individual performance issues from structural organizational problems.
* Prioritize interventions at the company level when multiple operators exhibit independent inefficiency patterns.

The framework is designed to be scalable and can be applied to any call center where operators work within identifiable teams.

---

## Dashboard

🔗 [Call Me Maybe — Operator Efficiency Analysis](https://public.tableau.com/app/profile/brianrodrigues./viz/TT14-CallMeMaybe/Dashboard)

## Author

**Brian Rodrigues**

Data Analyst | Python • SQL • Statistics • Business Analytics

* LinkedIn: https://linkedin.com/in/rodriguesbrian
* GitHub: https://github.com/rodriguesbrian
