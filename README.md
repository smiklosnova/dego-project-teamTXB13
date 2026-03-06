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

# Dataset Evolution

## 1 Raw Dataset

The raw dataset contains **full personally identifiable information (PII)** and inconsistent schema structures.

Example attributes:

- full_name
- email
- SSN
- IP address
- gender
- date_of_birth
- zip_code
- financial data
- spending behavior

### Issues Observed

- Sensitive personal identifiers stored in plaintext
- Inconsistent gender encoding ("M", "Male", "F")
- Multiple date formats
- Missing timestamps
- Nested JSON structures
- No governance metadata
- No retention policy

This dataset represents a **high-risk governance scenario**.

---

# 1 Data Quality Assessment

## Dataset Overview

- 502 loan applications
- Nested JSON structure
- Financial, demographic, behavioral, and decision data

After normalization, the dataset contains **flattened columns suitable for analysis and modeling**.

---

## Key Data Quality Findings

### 1 Inconsistent Data Types

Example issue:

`annual_income` stored inconsistently across records.

Remediation:

- Converted using `pd.to_numeric()`.

Data Quality Dimension: **Consistency**

---

### 2 Missing Values

Examples:

- Missing `processing_timestamp`
- Missing rejection reasons for denied loans
- Missing derived demographic features

Data Quality Dimension: **Completeness**

---

### 3 Duplicate Records

Duplicate application IDs were detected.

Risk:

- Model training bias
- Incorrect approval statistics

Data Quality Dimension: **Uniqueness**

---

### 4 Invalid or Ambiguous Dates

Multiple date formats were identified:

- `YYYY-MM-DD`
- `DD/MM/YYYY`
- `YYYY/MM/DD`

Some entries produced **ambiguous or unparsable dates**, resulting in missing derived ages.

Data Quality Dimension: **Validity**

---

### 5 Derived Data Quality Indicators

The cleaned dataset introduces additional validation features such as:

- `email_valid`
- `high_dti_flag`
- `parsed_dob`
- `demographics.age`

These features support downstream data validation and analytics.

---

# 2 Bias Detection & Fairness

## Gender Disparate Impact

We calculated the **Disparate Impact (DI) ratio**:


DI = Approval Rate (Unprivileged Group) / Approval Rate (Privileged Group)


Interpretation:

- **DI < 0.8 → Potential discrimination under the Four-Fifths Rule**

Results indicate **statistically significant gender disparities in approval outcomes**, suggesting possible algorithmic bias.

---

## Additional Bias Patterns

Our analysis revealed additional correlations:

- Approval probability varies across **age groups**
- **ZIP codes correlate with approval outcomes**

ZIP codes may function as **proxy variables for protected attributes** such as race or socioeconomic status.

This introduces **proxy discrimination risk** in automated decision-making.

---

# 3 Privacy & Governance Assessment

The raw dataset contains multiple **personally identifiable information (PII) attributes**, including:

- Full Name
- Email Address
- Social Security Number
- IP Address
- Date of Birth
- ZIP Code

These attributes expose **high privacy risk** if not governed properly.

---

# Identified GDPR Compliance Risks

## Article 5 — Storage Limitation

No retention policy existed, meaning personal data could be stored indefinitely.

Risk:

Creation of a **data swamp containing historical personal data**.

---

## Article 6 — Lawful Basis for Processing

No evidence of **user consent for automated decision-making**.

Risk:

Illegal profiling under GDPR.

---

## Article 14 — Transparency

The dataset lacked **data lineage information**, making it impossible to identify the source of external financial data.

---

## Article 17 — Right to Erasure

No mechanism existed to process **data deletion requests**.

---

# Governance Transformation (Compliant Dataset)

To address these risks, we implemented a **privacy-by-design transformation pipeline**, producing a compliant dataset.

Key governance improvements include:

---

## Data Minimization & Anonymization

Sensitive identifiers were removed or masked:

- SSN → pseudonymized (`***-**-XXXX`)
- Full names removed
- Emails removed
- IP addresses removed
- Date of birth generalized to **birth year**

This significantly reduces re-identification risk.

---

## Governance Metadata

A governance metadata block was introduced:


governance_metadata:
consent_timestamp
data_source
retention_until
is_deleted


Purpose:

- GDPR traceability
- data lifecycle management
- regulatory auditing

---

## Algorithmic Audit Trails

Each automated decision now includes:


audit_trail:
model_version
requires_human_review
human_reviewer_id


This ensures **algorithmic accountability and traceability**.

---

## Human-in-the-Loop Oversight

Rejected applications are flagged for **mandatory human review**.

Purpose:

- reduce algorithmic bias
- improve fairness
- comply with EU AI Act oversight requirements

---

## Data Lifecycle Management

Each record includes a retention schedule:


retention_until


Rejected applications are automatically scheduled for **deletion after 180 days**, preventing indefinite storage.

---

## Data Lineage Tracking

The compliant dataset records the origin of the data:


data_source: NovaCred_Web_Portal_v2


This ensures **full data provenance tracking**.

---

# Conclusion

Our audit demonstrates that NovaCred faces three major categories of risk:

### Data Quality Risks

- inconsistent formats
- missing values
- duplicate records
- ambiguous dates

### Algorithmic Fairness Risks

- gender-based disparate impact
- proxy discrimination through ZIP codes

### Regulatory Compliance Risks

- lack of consent mechanisms
- indefinite data storage
- missing audit trails
- absence of deletion mechanisms

Through the implementation of **data cleaning, bias auditing, and governance-by-design transformations**, we redesigned the dataset into a **privacy-preserving and regulation-ready architecture**.

This governance pipeline significantly improves:

- data reliability
- fairness oversight
- regulatory compliance
- operational accountability.

