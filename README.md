# dego-project-teamTXB13

DeGo Assignment TXB13 Team

# Team Members & Roles


| Name | Role | Responsibilities |
|------|------|------------------|
| Amal Boulila | Product Lead | Project coordination, README documentation, executive summary, presentation structure, governance synthesis |
| Milena | Data Scientist | Bias analysis, fairness metrics (Disparate Impact), statistical testing, visualizations |
| Schieszler Miki | Data Engineer | Data loading, JSON normalization, data cleaning pipeline, data quality checks |
| Gabriel Novais Vieira | Governance Officer | GDPR mapping, privacy assessment, AI Act classification, governance recommendations |

## Project Description


As the Data Governance Task Force for NovaCred, we conducted a comprehensive audit of 502 historical credit applications to assess:

1. Data quality issues  
2. Algorithmic bias in lending decisions  
3. Privacy and governance compliance  

Our analysis identified significant data quality weaknesses, measurable disparate impact by gender, and critical GDPR compliance gaps. We propose concrete governance interventions to mitigate regulatory and operational risk.

---

## Structure

- `data/` - Dataset files
- `notebooks/` - Jupyter analysis notebooks
- `src/` - Python source code
- `reports/` - Final deliverables


# 1. Data Quality Assessment

## Dataset Overview
- 502 applications
- 21 flattened columns
- Nested JSON structure normalized into tabular format

## Key Findings

### 1. Inconsistent Data Types
- `annual_income` stored as object instead of numeric.
- Required coercion using `pd.to_numeric()`.

Dimension: **Consistency**

---

### 2. Missing Values
- Missing financial fields in multiple records.
- Missing rejection reasons for some denied applications.

Dimension: **Completeness**

---

### 3. Duplicate Records
- Identified duplicate application IDs.
- Risk of double-counting in model training.

Dimension: **Accuracy**

---

### 4. Invalid or Suspicious Values
- Potential outliers in income and debt-to-income ratio.

Dimension: **Validity**

---

## Remediation Demonstrated
- Type conversion and validation
- Duplicate detection and removal
- Missing value reporting
- Numeric validation checks

---

# 2. Bias Detection & Fairness

## Gender Disparate Impact

We calculated the Disparate Impact (DI) ratio:

DI = Approval Rate (Unprivileged Group) / Approval Rate (Privileged Group)

Result:
- DI < 0.8 → indicates potential disparate impact under the four-fifths rule.

This suggests statistically significant gender-based disparities in approval decisions.

---

## Additional Bias Patterns

- Approval rate varies across age groups.
- ZIP code shows correlation with approval outcomes, indicating possible proxy discrimination.

ZIP code may act as a proxy for protected characteristics.


# 3. Privacy & Governance Assessment

### 1. Personally Identifiable Information (PII) identified: 
- Full Name 
- Email Address 
- Social Security Number (SSN) 
- IP Address 
- Date of Birth
- ZIP Code

### 2. Identified GDPR Risks
During the compliance audit of the raw dataset, we identified several critical regulatory vulnerabilities:
* **Article 5 (Storage Limitation):** The schema lacked retention schedules, creating an illegal "data swamp" of indefinitely stored historical records.
* **Article 6 (Lawful Basis):** A complete absence of timestamped consent mechanisms required for automated profiling.
* **Article 14 (Transparency):** Missing data lineage tracking to identify the vendor origin of external financial aggregates.
* **Article 17 (Right to Erasure):** The schema lacked the programmatic flags necessary to process and execute "Right to be Forgotten" deletion requests.

### Governance Recommendations
To architect a compliant, privacy-preserving pipeline, we suggested the following "Privacy by Design" controls into the dataset:
* **Automated Data Minimization & Anonymization:** Programmatically dropped direct identifiers (names, emails, IPs), pseudonymized SSNs, and generalized Dates of Birth.
* **Cryptographic Consent Mechanisms:** Enforced mandatory `consent_timestamp` gateways for all data ingestion payloads.
* **Automated Data Lifecycle:** Implemented programmatic 180-day `retention_until` schedules for rejected applications and `is_deleted` flags for erasure compliance.
* **Algorithmic Audit Trails (Traceability):** Tagged every automated decision with exact `model_version` metadata to satisfy EU AI Act requirements.
* **Mandatory Human-in-the-Loop (Oversight):** Routed rejected applications to a human review queue to prevent unmitigated algorithmic bias.
* **Ethical Feature Engineering:** Permanently deprecated highly intrusive behavioral tracking (`spending_behavior`) to reduce ethical and privacy risks.
* **Data Lineage Tracking:** Hardcoded `data_source` metadata to track the origin of external financial metrics.

# Conclusion

Our audit demonstrates that NovaCred faces:

- Data quality risks impacting model reliability
- Measurable disparate impact in lending decisions
- Significant GDPR compliance gaps
